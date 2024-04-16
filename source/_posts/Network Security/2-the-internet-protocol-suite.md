---
title: 'Network Security #2 The Internet Protocol Suite'
date: 2024-04-16 12:21:13
---

TCP/IP is the de facto protocol that modern networks use. Although you can think of TCP/IP as a single protocol, it’s actually a combination of two protocols: the Transmission Control Protocol (TCP) and the Internet Protocol (IP). These two protocols form part of the Internet Protocol Suite (IPS), a conceptual model of how network protocols send network traffic over the internet that breaks down network communication into four layers.

![Internet Protocol Suite Layers](https://i.imgur.com/CoasvgC.png)

These four layers form a protocol stack. The following list explains each layer of the IPS:

- **Link layer (layer 1)** This layer is the lowest level and describes the physical mechanisms used to transfer information between nodes on a local network. Well-known examples include Ethernet (both wired and wireless) and Point-to-Point Protocol (PPP)
- **Internet layer (layer 2)** This layer provides the mechanisms for addressing network nodes. Unlike in layer 1, the nodes don't have to be located on the local network. This level contains the IP; on modern networks, the actual protocol used could be either version 4 (IPv4) or version 6 (IPv6).
- **Transport layer (layer 3)** This layer is responsible for connections between clients and servers, sometimes ensuring the correct order of packets and providing service multiplexing. Service multiplexing allows a single node to support multiple different services by assigning a different number for each service; this number is called a port. TCP and the User Datagram Protocol (UDP) operate on this layer.
- **Application layer (layer 4)** This layer contains network protocols, such as the HyperText Transport Protocol (HTTP), which transfers web page contents; the Simple Mail Transport Protocol (SMTP), which transfers email; and the Domain Name System (DNS) protocol, which converts a name to a node on the network.

Each layer interacts only with the layer above and below it, but there must be some external interactions with the stack.

The link layer interacts with a physical network connection, transmitting data in a physical medium, such as pulses of electricity or light. The application layer interacts with the user application: an application is a collection of related functionality that provides a service to a user.

![Example Mail Application](https://i.imgur.com/kmFinAk.png)

Figure above shows an example of an application that processes email. The service provided by the mail application is the sending and receiving of messages over a network.

Typically, applications contain the following components:

- **Network communication** This component communicates over the network and processes incoming and outgoing data. For a mail application, the network communication is most likely a standard protocol, such as SMTP or [POP3](https://www.techtarget.com/whatis/definition/POP3-Post-Office-Protocol-3)
- **Content parsers**  Data transferred over a network usually contains content that must be extracted and processed. Content might include textual data, such as the body of an email, or it might be pictures or video.
- **User interface (UI)** The UI allows the user to view received emails and to create new emails for transmission. In a mail application, the UI might display emails using HTML in a web browser.

Note the user interacting with the UI doesn't have to be a human being. It could be another application that automates the sending and receiving of emails through a command line tool.