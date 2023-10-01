
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
  - Functions for conversion
    1. htons() -> Host TO Network Short
    2. ntohs()
    3. htonl()
    4. ntohl() -> Network TO Host Long
    * (short , long) = (2 bytes , 4 bytes)

Extra: RFC 1918 , Gopher Protocol, Kermit protocol, ELIZA , Vint Cerf
---

