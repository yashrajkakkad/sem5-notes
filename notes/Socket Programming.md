---
attachments: [Clipboard_2020-10-02-10-35-07.png, Clipboard_2020-10-02-10-58-14.png, Clipboard_2020-10-02-11-07-11.png]
tags: [cn-lab]
title: Socket Programming
created: '2020-10-02T04:36:44.046Z'
modified: '2020-10-16T03:33:12.528Z'
---

# Socket Programming
A socket is one endpoint of a two way communication link between two programs running on the network. The socket mechanism provides a means of inter-process communication (IPC) by establishing named contact points between which the communication take place.

A socket is an interface between application process and transport layer.

The application process can send/receive messages to/from another application process (local or remote) via a socket.

Types of Sockets: Internet Sockets, UNIX Sockets, X.25 Sockets etc.
Internet sockets characterized by IP address (4 bytes), port number (2 bytes).
- 65536 available ports
  - 0 - 1023 - well known ports. IANA has reserved these
    - Telnet - 23
    - HTTP - 80
    - FTP - 21 (Data transmit on 20)
      - ftp://ftp.xxx.com (optional different port)
      - Changeable, but should not ideally
      - If you change, it must be communicated
  - To avoid clashes, use ports > 1023 for your server.

Windows sockets library: Winsock

- Sockets lets you talk to the transport layer.

- Transport layer - end-to-end
- Link layer - hop-to-hop
- Network layer - host-to-host

- House example.

Things I need for socket communication, the things which constitute a socket:
  - IP Address
  - Port Number

## netstat command
Displays network-related information such as open connections, open socket ports, etc.

### Local Address
- Process ID

### Foreign Address
- IP can also be obtained from the address.
- Standardized ports, public IP addresses for reachability.

### Active UNIX domain sockets (w/o servers)
Interprocess communication:
- Shared memory location
  - Synchronous
  - Asynchronous
- Sending messages in synchronous mode 

- fd - File descriptor
  - an integer assigned by the kernal to access your file.
- inode entry - Data structure where the file information (not data) is stored 
  - index node.
  - fd is a part of inode

- `stat filename` - gives you information about the files.

- Analogous to file descriptor, we have a socket descriptor.

![](@attachment/Clipboard_2020-10-02-10-35-07.png)

## Demultiplexing

- Transport layer demultiplexes messages to send them to applications.

## Role of Transport layer
- Process-to-process channel.
- Segment
- Connectionless vs Connection-oriented
  - Connection-oriented
    - TCP
      - Three way handshaking
      - Large overhead in header as acknowledgement is expected.
      - Stream oriented data
  - Connectionless
    - UDP
      - Datagram
- SCTP - Stream Control Protocol
  - Set protocol to 0 makes the socket choose TCP or UDP based on type.
- There are several other protocols at the transport layer
- **Homework:** Real time content delivery protocols in streaming services like Netflix - DASH, SCTP (combination of TCP and UDP)

## Types of Internet Sockets
- Stream Sockets (SOCK_STREAM)
  - Connection oriented
  - TCP
  ![](@attachment/Clipboard_2020-10-02-10-58-14.png)
  - `bind()` - Binds the newly created socket to the specified address.
  - `listen()` - Wait for a client. Be active for accepting new connections.
  - `connect()` - Connect to server. Need not specify client port as OS is going to assign it.    
  - `accept()` - 
  - In practice, servers are multi-threaded. `pthread` library.
    - Processes are heavy, `lht` - light headed threads are sometimes ued as an alternative.
- Datagram Sockets (SOCK_DGRAM)
  - Connectionless
  - Rely on UDP
  ![](@attachment/Clipboard_2020-10-02-11-07-11.png)
  - No `listen()` or `accept()`

- `close()` - recommended for freeing up space.

  
## socket() -- Get the file descriptor

```c
int socket(int domain, int type, int protocol);
```

- Domain should be set to `PF_INET` (or `AF_INET`)
- Type can be `SOCK_STREAM` or `SOCK_DGRAM`(total there are 4 types)
- Set protocol to 0 to have socket choose the correct protocol based on type.
- socket() returns a socket descriptor for use in later system calls or -1 on error.

Transport layers and below are all in the kernel space. User has to interact with the transport layer.

## UDP
- Connectionless
- Applications:
  - Streaming
  - DNS
  - TFTP
    - Diskless booting -  using a remote system or systems to store the kernel and the filesystem that will be used on other computers.
    - Allows a client to get a file from or put a file on a remote host.
