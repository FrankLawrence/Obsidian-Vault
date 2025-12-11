---
Tags: 
Created: 2024-10-21 00:33:46
---
(Links:: [[Advanced Network Programming]])
> [!question] Why does application level IO performance matter?
> With very efficient TCP/IP processing, the **bottleneck** is pushed up to the application.

> [!example]- HTTP server
> **Attempt 1**:
> ```c
> void enter_server_loop(int server_socket) {
> 	struct sockaddr_in client_addr;  
> 	socklen_t client_addr_len = sizeof(client_addr);
> 	while (1)  
> 	{
> 		int client_socket = accept( 
> 			server_socket,
> 			(struct sockaddr *)&client_addr,
> 			&client_addr_len);
> 		if (client_socket == -1)
> 			fatal_error("accept()");
> 
> 		handle_client(client_socket);
> 		close(client_socket); 
> 	}
> }
> ```
> > [!bug] Problems?
> > Not scalable :(
> 
> **Attempt 2**:
> ```c
> void enter_server_loop(int server_socket) {
> 	struct sockaddr_in client_addr;  
> 	socklen_t client_addr_len = sizeof(client_addr);
> 	while (1) {
> 		int client_socket = accept(
> 			server_socket,
> 			(struct sockaddr *)&client_addr,
> 			&client_addr_len);
> 
> 		if (client_socket == -1)
> 			fatal_error("accept()");
> 		
> 		int pid = fork();
> 		if (pid == 0) {
> 			signal(SIGINT, SIG_DFL); 
> 			connect_to_redis_server(redis_host_ip); 
> 			handle_client(client_socket); 
> 			close(client_socket);
> 			exit(0);
> 		}else{
> 			close(client_socket); 
> 		}
> 	}
> } 
> ```
> > [!bug] Problem
> > - High memory/resources consumption
> > - More work for the scheduler

```c
int server_socket = setup_listening_socket(server_port); 
for(int i=0; i < PREFORK_CHILDREN; i++)
	pids[i] = create_child(i, server_socket);
```
```c
pid_t create_child(int index, int listening_socket) {
	pid_t pid;
	pid = fork();
	if(pid < 0)
		fatal_error("fork()");
	else if (pid > 0)
		return (pid);
	printf("Server %d(pid: %ld) starting\n", index, (long)getpid());
	enter_server_loop(listening_socket);
}
```
> [!bug] Problem
> Contention on `accept()` ([Thundering herd problem](https://en.wikipedia.org/wiki/Thundering_herd_problem))

```c
void enter_server_loop(int server_socket) {
	struct sockaddr_in client_addr;  
	socklen_t client_addr_len = sizeof(client_addr); pthread_t tid;
	while (1) {
		int client_socket = accept( 
			server_socket,
			(struct sockaddr *)&client_addr,
			&client_addr_len); 
		if (client_socket == -1)
			fatal_error("accept()");  

		pthread_create(&tid, NULL, &handle_client, (void *)(intptr_t) client_socket);
	} 
}
```
> [!bug] Problems?
> - High memory/resources consumption (but less than a full process!)
> - More work for the Scheduler
```c
for (int i = 0; i < THREADS_COUNT; ++i) {
	create_thread(i);
}
```
```c
void *enter_server_loop(void *targ)
{
	struct sockaddr_in client_addr;
	socklen_t client_addr_len = sizeof(client_addr);
	while (1)
	{
		pthread_mutex_lock(&mlock);
		long client_socket = accept(
			server_socket,
			(struct sockaddr *)&client_addr,
			&client_addr_len);
		if (client_socket == -1)
			fatal_error("accept()");
		pthread_mutex_unlock(&mlock);
		handle_client(client_socket);
	}
}
```
> [!bug] Problem
> - Contention on the lock/mutex
> - Still work for the Scheduler

One thread/process per connection seems to be problematic... Can we handle multiple connections with a single process/thread?

# Event based system
## poll()
```c
while (1) {  
	poll_ret = poll(poll_fds, poll_nfds, -1); 
	if (poll_ret < 0)
		fatal_error("poll()");  
	for (int i = 0; i < poll_nfds; i++) {
		if (poll_fds[i].revents == 0) 
			continue;
		int client_socket = accept( 
			server_socket,
			(struct sockaddr *)&client_addr,
			&client_addr_len);
		if (client_socket > 0 ) {
			handle_client(client_socket); 
		}
	}
}
```
**High-level idea**:
- Give the kernel a bunch of file descriptors/sockets
- The kernel will notify when one of them is ready for I/O
- Use (read/write) only sockets that are ready (meaning, they won't block when you attempt to read/write)
- Block on `poll()` only if there is nothing to do (with a timeout)

> [!bug] Problems
> - `poll_fds` Array copied all the time in the kernel...
> - We have to iterate over all FDs (`poll_nfds`) to figure out what is usable...
## epoll()
```c
void add_to_epoll_fd_list(int fd, uint32_t ep_events) { 
	struct epoll_event event;  
	event.data.fd = fd;  
	event.events = ep_events;
	if (epoll_ctl(epoll_fd, EPOLL_CTL_ADD, fd, &event)) 
		fatal_error("add epoll_ctl()");
}
```
```c
while (1) {  
	epoll_ret = epoll_wait(epoll_fd, events, MAX_EVENTS, -1);
	 if (epoll_ret < 0)
		fatal_error("epoll_wait()"); 
	
	for (int i = 0; i < epoll_ret; i++) {
		if (events[i].events == 0) 
			continue;
		
		int client_socket = accept( 
			server_socket,
			(struct sockaddr *)&client_addr,
			&client_addr_len); 
		if (client_socket > 0 ) {
			handle_client(client_socket); 
		}
	}
}
```

| # FDs | `poll()` CPU time | `select()` CPU time | `epoll()` CPU time |
| ----- | ----------------- | ------------------- | ------------------ |
| 10    | 0.61              | 0.73                | 0.41               |
| 100   | 2.9               | 3                   | 0.42               |
| 1000  | 35                | 35                  | 0.53               |
| 10000 | 990               | 930                 | 0.66               |

## Programming language implementations
### Java NIO2
```java
ServerSocketChannel channel = ServerSocketChannel.open);
channel.bind(new InetSocketAddress("localhost", 2222));
channel.configureBlocking(false);

Selector selector = Selector.open;
SelectionKey key = channel.register (selector, SelectionKey.OP_ACCEPT);

while (true) {
	if (selector.select = 0) {
		Thread.sleep(1);
		continue;
	}
	Set<SelectionKey> selectedKeys = selector.selectedKeys();
	Iterator<SelectionKey> keyIterator = selectedKeys.iterator ();
	while (keyIterator.hasNext()) {
		SelectionKey selectionKey = keyIterator.next();
		if (selectionKey.isAcceptable()) {
			// a connection was accepted by a ServerSocketChannel.
		} else if (selectionKey.isConnectable()) {
			// a connection was established with a remote.
		} else if (selectionKey.isReadable()) {
			// a channel is ready for reading
		} else if (selectionKey.isWritable()) {
			// a channel is ready for writing
		keyIterator.remove();
	}
}
```
### Rust Tokio
```Rust
use mini_redis: {client, Result};

#[tokio::main]
async fn main () -> Result<()> {
	// Open a connection to the mini-redis address.
	let mut client = client:: connect("127.0.0.1:6379").

	// Set the key "hello" with value "world"
	client. set ("hello", "world".into ()).await?;

	// Get key "hello"
	let result = client.get ("hello") await?;

	printIn! ("got value from the server; result={:?}",

	Ok( ( ))
}
```
### Javascript: Node.js client
```javascript
const fetch = require('node-fetch'); 
(async () => {
	try {  
		const response = await fetch('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY')
		const json = await response.json()
		
		console.log(json.url);
		console.log(json.explanation); 
	} catch (error) {
		console.log(error.response.body); 
	}
})();
```
> [!example]- hello world server
> ```javascript
> import { createServer } from 'node:http';
> 
> const server = createServer((req, res) => {
> 	res.writeHead(200, { 'Content-Type': 'text/plain' }); 
> 	res.end('Hello World!\n');
> });
> 
> server.listen(3000, '127.0.0.1', () => { 
> 	console.log('Listening on 127.0.0.1:3000');
> });
> ```
> Javascript only has one line of execution (one thread). This example can handle thousands of connections/sec.

The above js code can be rewritten into more low level js code. It can show us where exactly we use `epoll()`.
```js
const https = require('https'); 
https.get('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY', (resp) => {
	let data = '''';

	// A chunk of data has been received.
	resp.on('data', (chunk) => { 
		data += chunk;
	});
	
	// The whole response has been received. Print out the result.
	resp.on('end', () => { 
		console.log(JSON.parse(data).explanation);
	});

}).on("error", (err) => { 
	console.log("Error: " + err.message);
});
```


---
References:
https://nostarch.com/tlpi
https://www.usenix.org/legacy/events/hotos03/tech/vonbehren.html
https://www.dre.vanderbilt.edu/~schmidt/PDF/reactor-siemens.pdf
https://blog.whatsapp.com/1-million-is-so-2011
https://levelup.gitconnected.com/deep-dive-into-node-js-asynchronous-architecture-cd61c643135e