---
title: 'Network Security #1 The Basic of Networking'
date: 2024-04-16 08:42:21
---

To attack network protocols, you need to understand the basic of computer networking. The more you understand how commin networks are built and function, the easier it will be to apply that knowledge to capturing, analyzing, and exploiting new protocols.

## Network Architecture and Protocols

Let's start by reviewing some basic networking terminology and asking the fundamental question: "what is a network?"

A network is a set of two or more computers connected together to share information. It's common to refer to each connected device as a node on the network to make the description applicalble to a wider range of devices.

![A simple network of three nodes](https://i.imgur.com/PEwP15r.png)

The figure shows three nodes connected with a common network. Each node might have a different operating system or hardware. But as long as each node follows a set of rules, or network protocol, it can communicate with the other nodes one the network. To communicate correctly, all nodes on a network must understand the the same network protocol.

A network protocol serves many functions, including one or more of the following:

- **Maintaining session state** Protocols typically implement mechanisms to create new connections and terminate existing connections.
- **Identifying nodes through addressing** Data must be transmitted to the correct node on a network. Some protocols implement an addressing mechanism to identify specific nodes or groups of nodes.
- **Controlling flow** The amount of data transferred across a network is limited. Protocols can implement ways of managing data flow to increase throughput and
reduce latency.
- **Guaranteeing the order of transmitted data** Many networks do not guarantee that the order in which the data is sent will match the order in which it’s received. A protocol can reorder the data to ensure it’s delivered in the correct order.
- **Detecting and correcting errors** Many networks are not 100 percent reliable; data can become corrupted. It’s important to detect corruption and, ideally, correct it.
- **Formatting and encoding data** Data isn’t always in a format suitable for transmitting on the network. A protocol can specify ways of encoding data, such as encoding English text into binary values.