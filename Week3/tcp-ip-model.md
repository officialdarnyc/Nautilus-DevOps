# TCP/IP Model
## What is TCP/IP?

TCP/IP stands for Transmission Control Protocol/Internet Protocol and is a suite of communication protocols used to interconnect network devices on the internet. TCP/IP is also used as a communications protocol in a private computer network (an intranet or extranet).

Common TCP/IP protocols include the following:

- Hypertext Transfer Protocol (HTTP) handles the communication between a web server and a web browser.
- HTTP Secure handles secure communication between a web server and a web browser.
- File Transfer Protocol handles transmission of files between computers.

## How does TCP/IP work?

TCP/IP uses the client-server model of communication in which a user or machine (a client) is provided a service, like sending a webpage, by another computer (a server) in the network.

Collectively, the TCP/IP suite of protocols is classified as stateless, which means each client request is considered new because it is unrelated to previous requests. Being stateless frees up network paths so they can be used continuously.

The transport layer itself, however, is stateful. It transmits a single message, and its connection remains in place until all the packets in a message have been received and reassembled at the destination.

The TCP/IP model differs slightly from the seven-layer Open Systems Interconnection (OSI) networking model designed after it. The OSI reference model defines how applications can communicate over a network.

## The 4 layers of the TCP/IP Model

TCP/IP functionality is divided into four layers, each of which includes specific protocols:

1. The application layer provides applications with standardized data exchange. Its protocols include HTTP, FTP, Post Office Protocol 3 (POP3), Simple Mail Transfer Protocol (SMTP) and Simple Network Management Protocol (SNMP). At the application layer, the payload is the actual application data.

2. The transport layer is responsible for maintaining end-to-end communications across the network. TCP handles communications between hosts and provides flow control, multiplexing and reliability. The transport protocols include TCP and User Datagram Protocol, which is sometimes used instead of TCP for special purposes.

3. The network layer, also called the internet layer, deals with packets and connects independent networks to transport the packets across network boundaries. The network layer protocols are IP and Internet Control Message Protocol, which is used for error reporting.

4. The physical layer, also known as the network interface layer or data link layer, consists of protocols that operate only on a link -- the network component that interconnects nodes or hosts in the network. The protocols in this lowest layer include Ethernet for local area networks and Address Resolution Protocol.