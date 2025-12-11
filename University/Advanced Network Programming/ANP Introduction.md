---
Tags: 
Created: 2024-10-07 20:32:15
---
(Links:: [[Advanced Network Programming]])

# Introduction
How do applications use the network? One example is to use the **Socket Interface** API. 
```C
int sock = socket(AF_INET, SOCK_STREAM, 0);
struct sockaddr_in serv_addr;
[...]
serv_addr.sin_family = AF_INET;
serv_addr.sin_port = htons(PORT);

[...]  
connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr); // send data  
send(sock , “Hello World!”, 12, ...);  
//recv data  
recv(sock, buffer, 1024);
```

Some background: The first implementations of the socket interface where in 4.2 BSD Unix (1983). Today there are alternatives: WinSock, BSD socket, POSIX socket and more. The first reference of the socket was in the [RFC #147](https://www.rfc-editor.org/rfc/rfc147) in 1971. It follows the UNIX philosophy: **everything is a file** (sockets are file descriptors).

> [!info]- Recap: Socket API
> 1. `int fd = socket(AF_INET, SOCK_STREAM, 0);`
> 	1. Returns a file descriptor as an integer
> 	2. File descriptor, a process-local integer to identify any open file or socket
> 	3. Unique within a process
> 2. `int ret = connect(fd, (struct sockaddr*)&server_addr, sizeof(server_addr))`
> 	1. Proceed to setup a connection with a server_addr and attach to "fd"
> 	2. For TCP, here runs the 3-way handshake protocol
> 3. `ret = send(fd, (void*) tx_buffer, (int) size, (int) flags);`
> 	1. Does data transmission operation
> 	2. Return values:
> 		1. `<0`: There was error, check errno
> 		2. `>0` but smaller than size: Only some part of data was accepted for TX
> 		3. equal to size: All of it was accepted for TX
> 4. Similarly for `recv()`
> 5. `close(fd)`: close the connection

## Socket Interface
- Setting up and managing connections
	- `socket()`, `bind()`, `listen()`, `connect()`, `accept()`, `close()`
- Network operations
	- `send()`, `recv()`, `sendto()` and `recvfrom()` (or `write()` and `read()` may also be used)
- Address/hostname management
	- `gethostbyname()` and `gethostbyaddr()` to resolve IPv4 host names and addresses
- Select activity and readiness of a socket for I/O
	- `select()`, `poll()`
- Setting up extra options
	- `getsockopt()` and `setsockopt()`

The socket is a very successful abstraction since applications have not to worry about anything but "what to send" and "where to receive" with `send()` and `recv()`. It has worked **extremely well** all these years supporting different classes of applications (web servers, video streaming, messaging,...).

## Packet's Journey
**Sending Path**:
1. Queue data when application calls `send()`
2. Perform data packet building
	- TCP Header
	- IP Header
	- Which device
3. Tell the device driver to transmit the packet
4. Packet is transmitted on the network (through Network Interface Controller)

**Receiving Path**:
1. A new packet arrives from the network
2. Notify the operating system. All network cards have device drivers
3. Perform network processing figure out which application queue data into the queue
4. Consume data when application calls `recv()`


---
References:
