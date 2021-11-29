# HTTP, TCP, and UDP
## TCP and UDP

TCP stands for Transmission Control Protocol. UDP stands for User Datagram Protocol. Both protocols allow network applications to exchange data between nodes. The main difference between both is that TCP is a connection-oriented protocol while UDP is a connectionless protocol.

When the TCP protocol is used, a special connection is opened up between two network devices, and the channel remains open to transmit data until it is closed. On the other hand, a UDP transmission does not make a proper connection and merely broadcasts its data to the specified network address without any verification of receipt.

## HTTP

HTTP stands for Hypertext Transfer Protocol, it's often called the protocol of the Internet. HTTP received this designation because most Internet traffic is based on HTTP. When a user requests a Web resource, it is requested using HTTP. The following is a Web request:

http://www.example.com

When a client enters this address into a Web browser, DNS is called to resolve the Fully Qualified Domain Name (FQDN) to an IP address. When the address is resolved, an HTTP get request is sent to the Web server. The Web server responds with an HTTP send response. Such communication is done several times throughout a single session to a Web site. HTTP uses TCP for communication between clients and servers. HTTP operates on port 80.