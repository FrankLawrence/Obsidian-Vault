---
Tags: 
Created: 2024-09-10 00:44:07
---
(Links:: [[Advanced Network Programming]])
# Advanced Network Programming (ANP) Skeleton 

ANP skeleton is the basic skeleton code used in the ANP course for developing your 
own networking stack. 

## Code 
Corresponding to the figure 3 in the accompanying assignment, here is a brief 
overview of the various files in this project 

  * anp_netdev.[ch] : implements the application privide network interface with 10.0.0.4 IP address. 
  * anpwrapper.[ch] : shared library wrapper code that provides hooks for socket, connect, send, recv, and close calls. 
  * arp.[ch] : ARP protocol implementation. 
  * config.h : various configurations and variables used in the project.
  * debug.h : some basic debugging facility. Feel free to enhance it as you see fit. 
  * ethernet.h : Ethernet header definition. 
  * icmp.[ch] : ICMP implementation (your milestone 2). 
  * init.[ch] : various initialization routines. 
  * ip.h : IP header definition. 
  * ip rx and rx : IP tranmission and reception paths. 
  * linklist.h : basic data structure implementation that you can use to keep track of various networking states (e.g., unacknowledged packets, open connections).
  * route.[ch] : a basic route cache implementation that can be used to find MAC address for a given IP (linked with the ARP implementation).
  * subuffer.[ch] : Linux kernel uses Socket Kernel Buffer (SKB) data strucutre to keep track of packets and data inside the kernel (http://vger.kernel.org/~davem/skb.html). This is our implementation of Socket Userspace Buffer (subuff). It is mostly used to build inplace packet headers.
  * systems_headers.h : a common include file with many commonly used headers. 
  * tap_netdev.[ch] : implementation for sending/receiving packets on the TAP device. It is pretty standard code. 
  * timer.[ch] : A very basic timer facility implementation that can be used to register a callback to do asynchronous processing. Mostly useful in timeout conditions. 
  * utilities.[ch] : various helper routines, printing, debugging, and checksum calculation.


***You are given quite a bit of skeleton code as a starting point. 
Feel free not to use or use any portion of it when developing your stack. 
You have complete freedom*** 
  
## Prerequisites

```bash
sudo apt-get install git build-essential cmake libcap-dev arping net-tools tcpdump 
``` 

## How to build 

```bash
cmake . 
make 
sudo make install  
```

This will build and install the shared library. 

## Scripts 

* sh-make-tun-dev.sh : make a new TUN/TAP device 
* sh-disable-ipv6.sh : disable IPv6 support 
* sh-setup-fwd.sh : setup packet forwarding rules from the TAP device to the outgoing interface. This script takes the NIC name which has connectivity to the outside world. The NIC name can be found by running the `nmcli` command (the one displayed in green e.g. 'enp1s0')
* sh-setup-arpserver.sh : compiles a dummy main program that can be used to run the shared library to run the ARP functionality 
* sh-hack-anp.sh : a wrapper script to preload the libanpnetstack library and take over networking calls. 
* sh-debug-anp.sh : a wrapper script that runs the supplied program in gdb for debugging and preloads the libanpnetstack library. Before starting to debug, configure the CMake project with `cmake . -DCMAKE_BUILD_TYPE=Debug` (and rebuild) to enable debug symbols in the shared library.

# Setup 
After a clean reboot, run following scripts in the order 
 1. Make a TAP/TUN device 
 2. Disable IPv6 
 3. Setup packet forwarding rules

 
## To run ARP setup 
Make and install the libanpnetstack library. Then run `./sh-setup-arpserver.sh` and follow instructions from there. Example 

```bash
atr@atr:~/anp-netskeleton/bin$ ./sh-setup-arpserver.sh 
++ gcc ./arpdummy.c -o arpdummy
++ set +x
-------------------------------------------
The ANP netstack is ready, now try this:
 1. [terminal 1] ./sh-hack-anp.sh ./arpdummy
 2. [terminal 2] try running arping -I tap0 10.0.0.4

atr@atr:~/anp-netskeleton/bin$ sudo ./sh-hack-anp.sh ./arpdummy
[sudo] password for atr: 
+ prog=./arpdummy
+ shift
+ LD_PRELOAD=/usr/local/lib/libanpnetstack.so ./arpdummy
Hello there, I am ANP networking stack!
tap device OK, tap0 
Executing : ip link set dev tap0 up 
OK: device should be up now, 10.0.0.0/24 
Executing : ip route add dev tap0 10.0.0.0/24 
OK: setting the device route, 10.0.0.0/24 
Executing : ip address add dev tap0 local 10.0.0.5 
OK: setting the device address 10.0.0.5 
GXXX  0.0.0.0
GXXX  0.0.0.0
GXXX  10.0.0.5
[ARP] A new entry for 10.0.0.5
ARP an entry updated 
ARP an entry updated 
^C
atr@atr:~/anp-netskeleton/bin$
```
From a second terminal 
  
```bash
atr@atr:~/home/atr$ arping -I tap0 10.0.0.4 
ARPING 10.0.0.4 from 10.0.0.5 tap0
Unicast reply from 10.0.0.4 [??:??:??:??:??]  0.728ms
Unicast reply from 10.0.0.4 [??:??:??:??:??]  0.922ms
Unicast reply from 10.0.0.4 [??:??:??:??:??]  1.120ms
^CSent 3 probes (1 broadcast(s))
Received 3 response(s) 
```
In place of [??:??:??:??:??] you should see an actual mac address for the TAP device. 

## A simple TCP-Server client example 
In the repo, you are also given a simple C socket server-client example. It is the accompanying code for developing the ANP
networking stack for Advanced Network Programming. When you compile and build, this code is also compiled. 

After compiling you will have server and client executables in the `./build/` folder

* `./build/anp_server`
* `./build/anp_client`

You should be able to run these programs unmodified with your ANP netstack stack.

### Parameters

Both, the server and client, have very primitive parameter parsing built in (patches welcome). Essentially

```bash
./build/anp_server -a [ip_address] -p [port] 
./build/anp_server -a [ip_address] -p [port]
```

On the server side the IP will be the IP on which the server binds and port which it listens.
You must pass both, or only IP parameter. The same logic goes for the client side.

When in doubt, read the source code. In the future revision, I will update to have more
sophisticated parameter parsing. But, for now it will do.

You can ask the client to wait before starting the test with the `-w` option.
This can be useful if you want to capture packets on the tap0 interface with tcpdump
as the interface is only created when running the program together with `bin/sh-hack-anp.sh`
(see [this section](#getting-started-with-hijacking-socket-call-and-integration-of-libanpstack) for details on how to do that).

**Note:** The client waits for 5 second before calling close to the server side. This is to make sure that
all TCP buffer related calls are done.

### Example runs

#### Default loopback, no parameters
```bash 
$./build/anp_server 
Socket successfully created, fd = 3 
default IP: 0.0.0.0 and port 43211 (none) 
OK: going to bind at 0.0.0.0 
Socket successfully binded
Server listening.
new incoming connection from 127.0.0.1 
         [receive loop] 4096 bytes, looping again, so_far 4096 target 4096 
OK: buffer received ok, pattern match :  < OK, matched >   
         [send loop] 4096 bytes, looping again, so_far 4096 target 4096 
OK: buffer tx backed 
ret from the recv is 0 errno 0 
OK: server and client sockets closed
----

$./build/anp_client 
usage: ./anp_client ip [default: 127.0.0.1] port [default: 43211]
OK: socket created, fd is 3 
default IP: 127.0.0.1 and port 43211 
OK: connected to the server at 127.0.0.1 
         [send loop] 4096 bytes, looping again, so_far 4096 target 4096 
OK: buffer sent successfully 
OK: waiting to receive data 
         [receive loop] 4096 bytes, looping again, so_far 4096 target 4096 
Results of pattern matching:  < OK, matched >  
A 5 sec wait before calling close 
OK: shutdown was fine. Good bye!
``` 

### with loopback ip and port
```bash
$./build/anp_server 192.168.56.1 4096 
Socket successfully created, fd = 3 
setting up the IP: 127.0.0.2 and port 4096 (both) 
OK: going to bind at 127.0.0.2 
Socket successfully binded
Server listening.
new incoming connection from 127.0.0.1 
         [receive loop] 4096 bytes, looping again, so_far 4096 target 4096 
OK: buffer received ok, pattern match :  < OK, matched >   
         [send loop] 4096 bytes, looping again, so_far 4096 target 4096 
OK: buffer tx backed 
ret from the recv is 0 errno 0 
OK: server and client sockets closed

--- 
$./build/anp_client 192.168.56.1 4096
 usage: ./anp_client ip [default: 127.0.0.1] port [default: 43211]
 OK: socket created, fd is 3 
 setting up the IP: 127.0.0.2 and port 4096 
 OK: connected to the server at 127.0.0.2 
          [send loop] 4096 bytes, looping again, so_far 4096 target 4096 
 OK: buffer sent successfully 
 OK: waiting to receive data 
          [receive loop] 4096 bytes, looping again, so_far 4096 target 4096 
 Results of pattern matching:  < OK, matched >  
 A 5 sec wait before calling close 
 OK: shutdown was fine. Good bye!

```

## Getting started with hijacking socket call and integration of libanpstack 

* Step 1: run the TCP server in one terminal with a specific IP and port number 
* Step 2: run the TCP client in another terminal, and first check if they connect, run, and pass the buffer matching test.
 
Then, you can run your client with the `./bin/sh-hack-anp.sh` script as 
```bash
sudo [path_to_anp]/bin/sh-hack-anp.sh [path]/build/anp_client -a IP -p port 
``` 
Or, run your client in GDB with the `./bin/sh-debug-anp.sh` script to debug
```bash
sudo [path_to_anp]/bin/sh-debug-anp.sh [path]/build/anp_client -a IP -p port 
```

In case you have some non-standard installation path, please 
update the path `/usr/local/lib/libanpnetstack.so` in the `sh-hack-anp.sh` script.

## Authors 
Animesh Trivedi, Lin Wang and the ANP teaching team 
VU Amsterdam 

---
References: