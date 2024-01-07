
# Chapter 2
- Sockets -> communication using file descriptors
  - File descriptors -> pointers to a file
  - send() & recv()

- Three types of sockets 
  1. Raw -> Read more
  2. Stream -> 
    * Uses TCP -> Transmission Control Protocol
    * Sequential Data
    * Reliability -> Uses continuous connection
  3. Datagram ->
    * Uses UDP -> User Datagram Protocol
    * Non-sequential
    * Fast -> Prioritises speed over reliability
    * Sends ack packet after recieving

- Different Layers
  1. Application Layer -> Telnet & FTP -> Debate
  2. Host to Host transport layer -> TCP & UDP -> Marathi
  3. Internet Layer -> IP & Routing -> Mumbai
  4. Network Access layer -> Ethernet & Wifi -> Flight

  * All these layers are used to transport data -> Cat

Extra : Have a look throught the RFC -> 793 (TCP) , 791 (IP) , 768 (UDP)

---

# Chapter 3
- IPv4 -> 4 bytes -> Too small -> IPv6
- IPv6 -> Hexadecimal -> 16 bytes
  1. Eg: 2001:0db8:c9d2:0012:0000:0000:0000:0051
  2. Shortened -> Removing leading zeros
    - Eg
      a. 2001:0db8:c9d2:0012:0000:0000:0000:0051 === 2001:db8:c9d2:12::51
      b. 2001:0db8:ab00:0000:0000:0000:0000:0000 === 2001:db8:ab00::
      c. 0000:0000:0000:0000:0000:0000:0000:0001 === ::1
  3. loopback -> localhost -> ::1
  4. IPv4 compatability mode -> ::ffff:192.0.2.33

- Subnets
  - IP Address -> 2 portions
    1. Network 
    2. Host
  - Old -> Three classes -> (let test_ip = 192.0.2.1)
    1. Class A
      * Network = 192
      * Host = 0.2.1
    2. Class B
      * Network = 192.0
      * Host = 2.1
    3. Class C
      * Network = 192.0.2
      * Host = 1
  - Netmask -> extracts network number from ip
    * network_number = (ip => (ip & netmask))
    * number of bits in network (or) number of bits in mask -> <IP>/<X>
      - Ex
        a. 192.0.2.12/30
        b. 2001:db8::/32

- Ports -> Internet Layer -> More specificity -> Mumbai Hotel, Room No.420
  * Yak shaving 
    * /etc/services -> ports mapped
    * Big IANA port list

- Byte Order
  - Order of data stored -> 2 types
    1. Big Endian -> Network Byte Order -> b34f -> b3 + 4f
    2. Little Endian -> b34f -> 4f + b3
    * Host Byte Order -> Depends on system
      * x86 -> Little endian -> run `lscpu | grep 'Byte Order'`
      * `sysctl hw` for mac
  - Functions for conversion
    1. htons() -> Host TO Network Short
    2. ntohs()
    3. htonl()
    4. ntohl() -> Network TO Host Long
    * (short , long) = (2 bytes , 4 bytes)

Extra: RFC 1918 , Gopher Protocol, Kermit protocol, ELIZA , Vint Cerf

---

### All code is in Rust.

## 3.3

- [Socket Address V6 Rabbit hole](https://doc.rust-lang.org/nightly/core/net/struct.SocketAddrV6.html)
    - rabbit hole -> look at parse in first example
- [Socket Address struct](https://doc.rust-lang.org/nightly/core/net/enum.SocketAddr.html)

---

## 3.4
- inet_pton() -> string to sock_addr -> presentation to network
- inet_ntop() -> sock_addr to string -> network to presentation
- These are C conversion functions. Rust implements these as traits on the IPAddrV4 / V6 types.

### 3.4.1
- NAT -> Network Address Translation -> Internal IP address to External IP address and vice versa
    - implemented by firewalls -> especially on home routers
    - Standard Internal
        * 10.x.x.x
        * 192.168.x.x
        * 172.y.x.x
            * y = [16..31]
* Extra : great firewall of china

---

# Chapter 4
IPv4 -> IPv6 -> Things to keep generic + change

---

# Chapter 5

## Until 5.2
1. getaddrinfo() -> Rust -> Can directly translate strings to socket addresses
    * [C source code for getaddrinfo](https://github.com/openbsd/src/blob/7bda13b189a4f018fdaecef65dbcb2de30679627/lib/libc/asr/getaddrinfo.c#L28) 
    * [Rust Source Code](https://doc.rust-lang.org/std/net/trait.ToSocketAddrs.html) 
2. socket() -> Rust -> SockAddr ? 

---

* Fix -> ToSocketAddrs -> have to give "google.com:<port_number>" for it to work
* Extra -> Iterables | Iterators | Generators
    * Yield vs return
    * Symbolic Links -> Hard vs Soft Links
        * https://chat.openai.com/share/ea22ce75-9af8-46c1-818b-d6d64255f0e4

---

Extra : DNS Lookup crate in rust 
	* Motivation: ``` For resolving ip addresses for web servers you will probably want to avoid to_socket_addrs(). The request crate for example uses hyper::client::connect::dns::GaiResolver (which uses the same libc interface as to_socket_addrs()) or https://github.com/hickory-dns/hickory-dns which has a pure rust dns implementation.```
	
---

## 5.3
1. Sockets <-> Ports -> Call bind
   * Rust me sock_addr already takes care of this.
2. Old way mentioned by the author in the book
3. Ports < 1024 -> Reserved for superusers
4. If you don't give a shit about which port -> call connect() and it should assign

## 5.4

* connect -> connects to a remote host given IP address && port
* Returns -1 on error and sets the variable : errno

## 5.5
* listen -> waits for a request for a connection
  * backlog -> maximum number of connections in request queue

Extra : Backlog parameter in rust hardcoded to 128 ?
	* [Github issue](https://github.com/rust-lang/rust/issues/55614)

## 5.6
* Accept -> Accepts the connection & returns a socket file descriptor
* Steps till now
  * create the structs
  * create the socket
  * bind socket to port
  * listen on port
  * accept connection

### 5.7

* send() -> send data
  * returns number of bytes
  * you have to compare and see if length of data sent is equal to data length, or you have to fire another send() call
* recv() -> recieve data
  * returns bytes recieved in buffer
  * returns 0 if connection is closed

Extra
* Project -> Making git
* Telnet -> how to use
* FTP server
* [How much data can you send through sockets](https://stackoverflow.com/questions/27120736/maximum-data-size-that-can-be-sent-and-received-using-sockets-at-oncetcp-sock)
  * [From the post above, about sockets](https://blog.erratasec.com/2014/04/fun-with-ids-funtime-3-heartbleed.html)
  * [Bittorrent extended to use UDP](http://www.bittorrent.org/beps/bep_0029.html)
  
* Fun stuff : Hiding zips in images
1. Zip up files - `zip <output> <files>
2. Concat to image - `cat mars.png 1.zip > mars_hidden.png`
3. unzip image - `unzip <image>`

---
