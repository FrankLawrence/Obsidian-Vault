---
Tags: lecture
Created: 2023-05-31 17:21:36
---
(Links:: [[Computer Networks]])
# HTTP In the World Wide Web
- different HTTP Request Methods to access data (GET, POST, PUT, HEAD,...)
- **Persistent connections** allow browsers to issue multiple requests over the same TCP connection
	- Increase performance by reducing connection setup overhead

> [!danger] Head of line Blocking
> Each request needs to wait for the previous one to complete
> -> Pipelined requests

- Pipelined requests: Increase performance by pipelining requests (hiding latency)

> [!danger] Performance problem
> Responses need to arrive in same order as requests

## HTTP/2
- Binary instead of plaintext -> easier for machines
- Multiplexed streams over a single TCP connection
	- Supports out-of-order responses
- Server push allows the server to send resources before the client asks for it explicitly
## HTTP/3
- Uses **QUIC** protocol 
	- performs multiplexing, using UDP
- UDP does not enforce in-order delivery
- QUIC orders data per stream
	- data is delivered in order within each stream, but not across streams
## WebSocket
- Provides TCP like data stream for complex applications
- Advantages over using WebSockets instead of TCP directly: Uses services from HTTP that aren't in TCP
	- Provides set of methods -> more simple
	- Provides security
	- Provides naming
# Multimedia applications
- Without compression, only possible over wired fiber-optic channels
- Compression reduced bandwidth requirement by an order of magnitude
## Digital audio compression
- **Lossy compression** achieves higher compression rate than **lossless compression** but **loses data**
- Humans cannot detect slight differences in image and audio quality
- Eyes are less sensitive to chrominance than to luminance -> compression rate $\times 2$
- Digital video: MPEG compresses over a sequence of frames only sending what **parts** that have changed from the previous frames
## Run-Length Encoding
- lossless compression technique

> [!example]
> `aaaaaaaabbbbbbcc` $\to$ `a8 b6 c2`

## Huffman Encoding
- create a queue with all symbols and their occurrences 
- combine the smallest two nodes into a new node, until only the root is left
- more common symbols are represented with smaller code-words 


---
References: