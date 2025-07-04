
![GitHub release (latest by date)](https://img.shields.io/github/v/release/h3xduck/Raw_TCP_Lib)
![Maintainability](https://img.shields.io/static/v1?label=maintainability&message=B&color=green)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/h3xduck/Raw_TCP)


# Xing_Lib

Xing is a library to build custom TCP/IP packets from the ground and send/receive them using only raw sockets and the C language.

## Features
* Create custom TCP/IP packets with control over all their fields (including network flags).
* Automatically build packets' checksums, or request a recalculation manually.
* Send built packets over raw sockets.
* **NEW:** Sniff incoming packets to your device, and operate with them later.

## Use cases

* Send spoofed network packets (fake source IP/port).
* Build specific packets for network attacks (e.g SYN flooding).
* Monitor your device network traffic.
* Fast integration in your network application.
  
  
Note: Xing uses the AF_INET address family so it supports both customization of the Network Layer (IP) and the Transport Layer (TCP) headers. Support for the Link Layer will come in future updates.



## Installation
Xing is built as an static library to facilitate integration with your project. 
1.  Clone Xing and put it in your project directory.
2.  Integrate it in your project
    *  METHOD 1: With gcc compiler
    ```sh
    gcc <your-files> libXing_Lib.a
    ```

    * METHOD 2 With cmake
    ```cmake
    target_link_libraries(<your-target> ${CMAKE_CURRENT_SOURCE_DIR}/libXing_Lib.a)
    ```
    Note that you will need to specify the path to the library in both cases if you stored it under a project folder.
3. Import "Xing.h" from a source file whenever you need Xing.

## Examples
### Create a TCP/IP packet
```c
//Packet indicates source=192.168.1.10:8000, dest=192.168.1.20:8080
packet_t packet = build_standard_packet(8000, 8080, "192.168.1.10", "192.168.1.20", 4096, "MyMessage");
```

### Set TCP flag active
Xing uses [hexadecimal notation](https://synfinner.blog/tcp-flags-hex-values) for the TCP flags. Multiple flags can be set at the same time by adding their values.
```c
//Setting both SYN and FIN flags
set_TCP_flags(packet, 0x03);
```
Xing manages the recomputation of the checksums automatically.

### Sending packet to destination
```c
//Destination port and destination IP are those specified when creating the packet.
rawsocket_send(packet);
```
IMPORTANT: Raw sockets need root priviliges, so you will need them before you can run this specific function.
### Continously sending packets
```c
//Sequence number & checksum automatically updated by OS
packet_t packet = build_standard_packet(...)
while(1){
    rawsocket_send(packet);
}
```

### Sniff incoming TCP packets
```c
//The packet can be used as if it was generated with build_standard_packet()
packet_t packet = rawsocket_sniff();
```

## Issues?
Whether it is a bug, question or suggestion, please [open a ticket]and I will have a look at it as fast as I possibly can.
## LICENSE
This software is licensed under the MIT license. See [LICENSE]




