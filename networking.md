# Networking

Explain the first steps for creating a VPN / Ipsec

## What is a stateless and statefull firewall?

Stateless firewalls watch network traffic, and restrict or block packets based on source and destination addresses or other static values. They are not 'aware' of traffic patterns or data flows. A stateless firewall uses simple rule-sets that do not account for the possibility that a packet might be received by the firewall 'pretending' to be something you asked for.

Stateful firewalls can watch traffic streams from end to end. They are are aware of communication paths and can implement various IP Security (IPsec) functions such as tunnels and encryption. In technical terms, this means that stateful firewalls can tell what stage a TCP connection is in (open, open sent, synchronized, synchronization acknowledge or established), it can tell if the MTU has changed, whether packets have fragmented etc.

## CIDR

CIDR (Classless Inter-Domain Routing, sometimes called supernetting) is a way to allow more flexible allocation of Internet Protocol (IP) addresses than was possible with the original system of IP address classes. As a result, the number of available Internet addresses was greatly increased, which along with widespread use of network address translation (NAT), has significantly extended the useful life of IPv4.

Originally, IP addresses were assigned in four major address classes, A through D. Each of these classes allocates one portion of the 32-bit IP address format to identify a network gateway -- the first 8 bits for class A, the first 16 for class B, and the first 24 for class C. The remainder identify hosts on that network -- more than 16 million in class A, 65,535 in class B and 254 in class C. (Class D addresses identify multicast domains.)

## Handshake TCP

Host A **sends** a TCP**SYN**chronize packet to Host B

Host B receives A's**SYN**

Host B **sends** a **S YN**chronize-ACKnowledgement

Host A receives B's **SYN-ACK**

Host A **sends ACK**nowledge

Host B receives **ACK**.

_**TCP socket connection is ESTABLISHED.**_

## Dump traffic

Tcpdump

## Differences between SCP and SFTP

SFTP is works on interactive mode (session) and SCP works on non-interactive. Using SFTP we can access remote file system i.e. creating, deleting, and listing files.

## REST vs RPC

The RPC API thinks in terms of "verbs", exposing the restaurant functionality as function calls that accept parameters, and invokes these functions via the HTTP verb that seems most appropriate - a 'get' for a query, and so on, but the name of the verb is purely incidental and has no real bearing on the actual functionality, since you're calling a different URL each time. Return codes are hand-coded, and part of the service contract.

The REST API, in contrast, models the various entities within the problem domain as resources, and uses HTTP verbs to represent transactions against these resources - POST to create, PUT to update, and GET to read. All of these verbs, invoked on the same URL, provide different functionality. Common HTTP return codes are used to convey status of the requests.

## Unicast Vs Multicast Vs Anycast

Unicast is used when two network nodes need to talk to each other. This is pretty straight forward, so I'm not going to spend much time on it. TCP by definition is a Unicast protocol, except when there is Anycast involved (more on that below).

When you need to have more than two nodes see the traffic, you have options.

If all of the nodes are on the same subnet, then**broadcast**becomes a viable solution. All nodes on the subnet will see all traffic. There is no TCP-like connection state maintained. Broadcast is a layer 2 feature in the Ethernet protocol, and also a layer 3 feature in IPv4.

**Multicast**is like a broadcast that can cross subnets, but unlike broadcast does not touch all nodes. Nodes have to subscribe to a multicast group to receive information. Multicast protocols are usually UDP protocols, since by definition no connection-state can be maintained. Nodes transmitting data to a multicast group do not know what nodes are receiving. By default, Internet routers do not pass Multicast traffic. For internal use, though, it is perfectly allowed; thus, "Defined horizon" in the above chart. Multicast is a layer 3 feature of IPv4 & IPv6.

To use**anycast**you advertise the same network in multiple spots of the Internet, and rely on shortest-path calculations to funnel clients to your multiple locations. As far the network nodes themselves are concerned, they're using a**unicast**connection to talk to your anycasted nodes. For more on Anycast, try:[What is "anycast" and how is it helpful?](http://serverfault.com/questions/14985/what-is-anycast-and-how-is-it-helpful). Anycast is also a layer 3 feature, but is a function of how route-coalescing happens.

Some examples of how the non-Unicast methods are used in the real Internet.

**Broadcast**\
ARP is a broadcast protocol, and is used by TCP/IP stacks to determine how to send traffic to other nodes on the network. If the destination is on the same subnet, ARP is used to figure out the MAC address that goes to the stated IP address. This is a Level 2 (Ethernet) broadcast, to the reserved FF:FF:FF:FF:FF:FF MAC address.

Also, Microsoft's machine browsing protocol is famously broadcast based. Work-arounds like WINS were created to allow cross-subnet browsing. This involves a Level 3 (IP) broadcast, which is an IP packet with the Destination address listed as the broadcast address of the subnet (in 192.168.101.0/24, the broadcast address would be 192.168.101.255).

The NTP protocol allows a broadcast method for announcing time sources.

**Multicast**\
Inside a corporate network, Multicast can deliver live video to multiple nodes without having to have massive bandwidth on the part of the server delivering the video feed. This way you can have a video server feeding a 720p stream on only a 100Mb connection, and yet still serve that feed to 3000 clients.

When Novell moved away from IPX and to IP, they had to pick a service-advertising protocol to replace the SAP protocol in IPX. In IPX, the Service Advertising Protocol, did a\_network-wide announcement\_every time it announced a service was available. As TCP/IP lacked such a global announcement protocol, Novell chose to use a Multicast based protocol instead: the Service Location Protocol. New servers announce their services on the SLP multicast group. Clients looking for specific types of services announce their need to the multicast group and listen for unicasted replies.

HP printers announce their presence on a multicast group by default. With the right tools, it makes it real easy to learn what printers are available on your network.

The NTP protocol\_also\_allows a multicast method (IP 224.0.1.1) for announcing time sources to areas beyond just the one subnet.

**Anycast**\
Anycast is a bit special since Unicast layers on top of it. Anycast is announcing the same network in different\_parts\_of the network, in order to decrease the network hops needed to get to that network.

The 6to4 IPv6 transition protocol uses Anycast. 6to4 gateways announce their presence on a specific IP, 192.88.99.1. Clients looking to use a 6to4 gateway send traffic to 192.88.99.1 and trust the network to deliver the connection request to a 6to4 router.

NTP services for especially popular NTP hosts may very well be anycasted, but I don't have proof of this. There is nothing in the protocol to prevent it.

Other services use Anycast to improve data locality to end users. Google does Anycast with its search pages in some places (and geo-IP in others). The Root DNS servers use Anycast for similar reasons. ServerFault itself just might go there, they do have datacenters in New York and Oregon, but hasn't gone there yet.

## What happens when you type www.google.com

[https://github.com/alex/what-happens-when](https://github.com/alex/what-happens-when)

Short answer:

The browser extracts the domain name from the URL.

The browser queries DNS for the IP address of the URL. Generally, the browser will have cached domains previously visited, and the operating system will have cached queries from any number of applications. If neither the browser nor the OS have a cached copy of the IP address, then a request is sent off to the system's configured DNS server. The client machine knows the IP address for the DNS server, so no lookup is necessary.

The request sent to the DNS server is almost always smaller than the maximum packet size, and is thus sent off as a single packet. In addition to the content of the request, the packet includes the IP address it is destined for in its header. Except in the simplest of cases (network hubs), as the packet reaches each piece of network equipment between the client and server, that equipment uses a routing table to figure out what node it is connected to that is most likely to be part of the fastest route to the destination. The process of determining which path is the best choice differs between equipment and can be very complicated.

The is either lost (in which case the request fails or is reiterated), or makes it to its destination, the DNS server.

If that DNS server has the address for that domain, it will return it. Otherwise, it will forward the query along to DNS server it is configured to defer to. This happens recursively until the request is fulfilled or it reaches an authoritative name server and can go no further. (If the authoritative name server doesn't recognize the domain, the response indicates failure and the browser generally gives an error like "Can't find the server at www.lkliejafadh.com".) The response makes its way back to the client machine much like the request traveled to the DNS server.

Assuming the DNS request is successful, the client machine now has an IP address that uniquely identifies a machine on the Internet. The web browser then assembles an HTTP request, which consists of a header and optional content. The header includes things like the specific path being requested from the web server, the HTTP version, any relevant browser cookies, etc. In the case in question (hitting Enter in the address bar), the content will be empty. In other cases, it may include form data like a username and password (or the content of an image file being uploaded, etc.)

This HTTP request is sent off to the web server host as some number of packets, each of which is routed in the same was as the earlier DNS query. (The packets have sequence numbers that allow them to be reassembled in order even if they take different paths.) Once the request arrives at the webserver, it generates a response (this may be a static page, served as-is, or a more dynamic response, generated in any number of ways.) The web server software sends the generated page back to the client.

Assuming the response HTML and not an image or data file, then the browser parses the HTML to render the page. Part of this parsing and rendering process may be the discovery that the web page includes images or other embedded content that is not part of the HTML document. The browser will then send off further requests (either to the original web server or different ones, as appropriate) to fetch the embedded content, which will then be rendered into the document as well.

DNS resource records

Zone DNS database is a collection of resource records and each of the records provides information about a specific object. A list of most common records is provided below:

*   [Address Mapping records](http://tools.ietf.org/html/rfc1035#page-12)

    (A) The record A specifies IP address (IPv4) for given host. A records are used for conversion of domain names to corresponding IP addresses.
*   [IP Version 6 Address records](http://tools.ietf.org/html/rfc3596#page-3)

    (AAAA) The record AAAA (also quad-A record) specifies IPv6 address for given host. So it works the same way as the A record and the difference is the type of IP address.
*   [Canonical Name records](http://tools.ietf.org/html/rfc1035#page-14)

    (CNAME) The CNAME record specifies a domain name that has to be queried in order to resolve the original DNS query. Therefore CNAME records are used for creating aliases of domain names. CNAME records are truly useful when we want to alias our domain to an external domain. In other cases we can remove CNAME records and replace them with A records and even decrease performance overhead.
*   [Host Information records](http://tools.ietf.org/html/rfc1035#page-14) (HINFO)\
    HINFO records are used to acquire general information about a host. The record specifies type of CPU and OS. The HINFO record data provides the possibility to use operating system specific protocols when two hosts want to communicate. For security reasons the HINFO records are not typically used on public servers.

    **Note:**Standard values in[RFC 1010](http://tools.ietf.org/html/rfc1010)
* [Integrated Services Digital Network records](http://tools.ietf.org/html/rfc1183#section-3.2)\
  (ISDN)\
  The ISDN resource record specifies ISDN address for a host. An ISDN address is a telephone number that consists of a country code, a national destination code, a ISDN Subscriber number and, optionally, a ISDN subaddress. The function of the record is only variation of the A resource record function.
* [Mail exchanger record](http://tools.ietf.org/html/rfc1035#section-3.3.9)\
  (MX)\
  The MX resource record specifies a mail exchange server for a DNS domain name. The information is used by Simple Mail Transfer Protocol (SMTP) to route emails to proper hosts. Typically, there are more than one mail exchange server for a DNS domain and each of them have set priority.

### TCP Slow Start

Tcp slow start is a congestion control algorithm that starts by increasing the TCP congestion window each time an ACK is received, until an ACK is not received.

### How HTTPS works

HTTPS takes the well-known and understood HTTP protocol, and simply layers a SSL/TLS (hereafter referred to simply as “SSL”) encryption layer on top of it. Servers and clients still speak exactly the same HTTP to each other, but over a secure SSL connection that encrypts and decrypts their requests and responses. The SSL layer has 2 main purposes:

* Verifying that you are talking directly to the server that you think you are talking to
* Ensuring that only the server can read what you send it and only you can read what it sends back

The really, really clever part is that anyone can intercept every single one of the messages you exchange with a server, including the ones where you are agreeing on the key and encryption strategy to use, and still not be able to read any of the actual data you send.

## How an SSL connection is established

An SSL connection between a client and server is set up by a handshake, the goals of which are:

* To satisfy the client that it is talking to the right server (and optionally visa versa)
* For the parties to have agreed on a “cipher suite”, which includes which encryption algorithm they will use to exchange data
* For the parties to have agreed on any necessary keys for this algorithm

Once the connection is established, both parties can use the agreed algorithm and keys to securely send messages to each other. We will break the handshake up into 3 main phases - Hello, Certificate Exchange and Key Exchange.

1. _Hello_- The handshake begins with the client sending a ClientHello message. This contains all the information the server needs in order to connect to the client via SSL, including the various cipher suites and maximum SSL version that it supports. The server responds with a ServerHello, which contains similar information required by the client, including a decision based on the client’s preferences about which cipher suite and version of SSL will be used.
2. _Certificate Exchange_- Now that contact has been established, the server has to prove its identity to the client. This is achieved using its SSL certificate, which is a very tiny bit like its passport. An SSL certificate contains various pieces of data, including the name of the owner, the property (eg. domain) it is attached to, the certificate’s public key, the digital signature and information about the certificate’s validity dates. The client checks that it either implicitly trusts the certificate, or that it is verified and trusted by one of several Certificate Authorities (CAs) that it also implicitly trusts. Much more about this shortly. Note that the server is also allowed to require a certificate to prove the client’s identity, but this typically only happens in very sensitive applications.
3. _Key Exchange_- The encryption of the actual message data exchanged by the client and server will be done using a symmetric algorithm, the exact nature of which was already agreed during the Hello phase. A symmetric algorithm uses a single key for both encryption and decryption, in contrast to asymmetric algorithms that require a public/private key pair. Both parties need to agree on this single, symmetric key, a process that is accomplished securely using asymmetric encryption and the server’s public/private keys.

The client generates a random key to be used for the main, symmetric algorithm. It encrypts it using an algorithm also agreed upon during the Hello phase, and the server’s public key (found on its SSL certificate). It sends this encrypted key to the server, where it is decrypted using the server’s private key, and the interesting parts of the handshake are complete. The parties are sufficiently happy that they are talking to the right person, and have secretly agreed on a key to symmetrically encrypt the data that they are about to send each other. HTTP requests and responses can now be sent by forming a plaintext message and then encrypting and sending it. The other party is the only one who knows how to decrypt this message, and so Man In The Middle Attackers are unable to read or modify any requests that they may intercept.

### Common HTTP response codes

200 OK The request has succeeded

500 Internal Server Error (Server Error)

403 Forbidden

301 Permanent Redirect

302 Temporary Redirect

[http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

### DHCP protocol states

DHCPDISCOVER client->server : broadcast to locate server

DHCPOFFER server->client : offer to client with offer of configuration parameters

DHCPREQUEST client->server : requesting a dhcp config from server

DHCPACK server->client : actual configuration paramters

DHCPNAK server->client : indicating client’s notion of network address is incorrect

DHCPDECLINE client->server : address is already in use

DHCPRELEASE client->server : giving up of ip address

DHCPINFORM client->server : asking for local config parameters

Reference: [https://www.ietf.org/rfc/rfc2131.txt](https://www.ietf.org/rfc/rfc2131.txt)

## Sockets

### Select

One traditional way to write network servers is to have the main server block on accept\\(\\), waiting for a connection. Once a connection comes in, the server fork\\(\\)s, the child process handles the connection and the main server is able to service new incoming requests.

With select\\(\\), instead of having a process for each request, there is usually only one process that "multi-plexes" all requests, servicing each request as much as it can.

So one main advantage of using select\\(\\) is that your server will only require a single process to handle all requests. Thus, your server will not need shared memory or synchronization primitives for different 'tasks' to communicate.

One major disadvantage of using select\\(\\), is that your server cannot act like there's only one client, like with a fork\\(\\)'ing solution. For example, with a fork\\(\\)'ing solution, after the server fork\\(\\)s, the child process works with the client as if there was only one client in the universe -- the child does not have to worry about new incoming connections or the existence of other sockets. With select\\(\\), the programming isn't as transparent.

### IO Multiplexing

When the TCP client is handling two inputs at the same time: standard input and a TCP socket, we encountered a problem when the client was blocked in a call to fgets \\(on standard input\\) and the server process was killed. The server TCP correctly sent a FIN to the client TCP, but since the client process was blocked reading from standard input, it never saw the EOF until it read from the socket \\(possibly much later\\).

We want to be notified if one or more I/O conditions are ready \\(i.e., input is ready to be read, or the descriptor is capable of taking more output\\). This capability is called I/O multiplexing and is provided by the select and poll functions, as well as a newer POSIX variation of the former, called pselect.

I/O multiplexing is typically used in networking applications in the following scenarios:

When a client is handling multiple descriptors \\(normally interactive input and a network socket\\)

When a client to handle multiple sockets at the same time \\(this is possible, but rare\\)

If a TCP server handles both a listening socket and its connected sockets

If a server handles both TCP and UDP

If a server handles multiple services and perhaps multiple protocols

I/O multiplexing is not limited to network programming. Many nontrivial applications find a need for these techniques.

[https://notes.shichao.io/unp/ch6/](https://notes.shichao.io/unp/ch6/)

Socket Server/Client example

## Socket Server Example

```
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <sys/types.h>
#include <time.h>

int main(int argc, char *argv[]) {
    int listenfd = 0, connfd = 0;
    struct sockaddr_in serv_addr; 

    char sendBuff[1025];
    time_t ticks; 

    listenfd = socket(AF_INET, SOCK_STREAM, 0);
    memset(
&
serv_addr, '0', sizeof(serv_addr));
    memset(sendBuff, '0', sizeof(sendBuff)); 

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    serv_addr.sin_port = htons(5000); 

    bind(listenfd, (struct sockaddr*)
&
serv_addr, sizeof(serv_addr)); 

    listen(listenfd, 10); 

    while(1)
    {
        connfd = accept(listenfd, (struct sockaddr*)NULL, NULL); 

        ticks = time(NULL);
        snprintf(sendBuff, sizeof(sendBuff), "%.24s\r\n", ctime(
&
ticks));
        write(connfd, sendBuff, strlen(sendBuff)); 

        close(connfd);
        sleep(1);
     }
}
```

In the above program, we have created a server. In the code :

* The call to the function ‘socket()’ creates an UN-named socket inside the kernel and returns an integer known as socket descriptor.
* This function takes domain/family as its first argument. For Internet family of IPv4 addresses we use AF\_INET.
* The second argument ‘SOCK\_STREAM’ specifies that the transport layer protocol that we want should be reliable ie it should have acknowledgement techniques. For example : TCP
* The third argument is generally left zero to let the kernel decide the default protocol to use for this connection. For connection oriented reliable connections, the default protocol used is TCP.
* The call to the function ‘bind()’ assigns the details specified in the structure ‘serv\_addr’ to the socket created in the step above. The details include, the family/domain, the interface to listen on(in case the system has multiple interfaces to network) and the port on which the server will wait for the client requests to come.
* The call to the function ‘listen()’ with second argument as ’10’ specifies maximum number of client connections that server will queue for this listening socket.
* After the call to listen(), this socket becomes a fully functional listening socket.
* In the call to accept(), the server is put to sleep and when for an incoming client request, the three way TCP handshake\* is complete, the function accept () wakes up and returns the socket descriptor representing the client socket.
* The call to accept() is run in an infinite loop so that the server is always running and the delay or sleep of 1 sec ensures that this server does not eat up all of your CPU processing.
* As soon as server gets a request from client, it prepares the date and time and writes on the client socket through the descriptor returned by accept().

Three way handshake is the procedure that is followed to establish a TCP connection between two remote hosts. We might soon be posting an article on the theoretical aspect of the TCP protocol.

Socket Client Example

```
#include <sys/socket.h>
#include <sys/types.h>
#include <netinet/in.h>
#include <netdb.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <arpa/inet.h>

int main(int argc, char *argv[]) {
    int sockfd = 0, n = 0;
    char recvBuff[1024];
    struct sockaddr_in serv_addr; 

    if(argc != 2)
    {
        printf("\n Usage: %s 
<
ip of server
>
 \n",argv[0]);
        return 1;
    } 

    memset(recvBuff, '0',sizeof(recvBuff));
    if((sockfd = socket(AF_INET, SOCK_STREAM, 0)) 
<
 0)
    {
        printf("\n Error : Could not create socket \n");
        return 1;
    } 

    memset(
&
serv_addr, '0', sizeof(serv_addr)); 

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(5000); 

    if(inet_pton(AF_INET, argv[1], 
&
serv_addr.sin_addr)
<
=0)
    {
        printf("\n inet_pton error occured\n");
        return 1;
    } 

    if( connect(sockfd, (struct sockaddr *)
&
serv_addr, sizeof(serv_addr)) 
<
 0)
    {
       printf("\n Error : Connect Failed \n");
       return 1;
    } 

    while ( (n = read(sockfd, recvBuff, sizeof(recvBuff)-1)) 
>
 0)
    {
        recvBuff[n] = 0;
        if(fputs(recvBuff, stdout) == EOF)
        {
            printf("\n Error : Fputs error\n");
        }
    } 

    if(n 
<
 0)
    {
        printf("\n Read error \n");
    } 

    return 0;
}
```

In the above program, we create a client which will connect to the server and receive date and time from it. In the above piece of code :

* We see that here also, a socket is created through call to socket() function.
* Information like IP address of the remote host and its port is bundled up in a structure and a call to function connect() is made which tries to connect this socket with the socket (IP address and port) of the remote host.
* Note that here we have not bind our client socket on a particular port as client generally use port assigned by kernel as client can have its socket associated with any port but In case of server it has to be a well known socket, so known servers bind to a specific port like HTTP server runs on port 80 etc while there is no such restrictions on clients.
* Once the sockets are connected, the server sends the data (date+time) on clients socket through clients socket descriptor and client can read it through normal read call on the its socket descriptor.

### Flow Control and Congestion Control

\_Flow control \_and \_congestion control\_are two different mechanisms for different purposes.

* \_Flow Control \_adapts sending rate of source to the receiving buffer and processing speed of receiver
* \_Congestion Control \_adapts rate of source to state of network

In another word,\_flow control\_is used to coordinate sending and receiving rate. If the sender is sending too fast, while the receiving application is processing slowly, the receiving buffer may not have enough space to hold the incoming data. Thus, there should be a mechanism to tell the sender to slow down.

\_Congestion control \_is a mechanism to avoid congestion collapse in the network. In the real world, if there’s heavy traffic, and every driver wants to rush, the congestion will be more severe. So there is need for a mechanism to regulate traffic in case of congestion.

[http://fengy.me/prog/2015/01/14/flow-control-and-congestion-control.html](http://fengy.me/prog/2015/01/14/flow-control-and-congestion-control.html)

### Error Control in TCP

TCP provides reliability using error control. Error control includes mechanisms for detecting corrupted segments, lost segments, out-of-order segments, and duplicated segments. Error control also includes a mechanism for correcting errors after they are detected. Error detection and correction in TCP is achieved through the use of three simple tools: checksum, acknowledgment, and time-out.

_**1. Checksum:**_

Each segment includes a checksum field which is used to check for a corrupted segment. If the segment is corrupted, it is discarded by the destination TCP and is considered as lost. TCP uses a 16-bit checksum that is mandatory in every segment.

_**2. Acknowledgment:**_

TCP uses acknowledgments to confirm the receipt of data segments. Control segments that carry no data but consume a sequence number are also acknowledged. ACK segments are never acknowledged.

_**3. Retransmission:**_

The heart of the error control mechanism is the retransmission of segments. When a segment is corrupted, lost, or delayed, it is retransmitted. A segment is retransmitted on two occasions: when a retransmission timer expires or when the sender receives three duplicate ACKs.

[http://www.myreadingroom.co.in/notes-and-studymaterial/68-dcn/853-error-control-in-tcp.html](http://www.myreadingroom.co.in/notes-and-studymaterial/68-dcn/853-error-control-in-tcp.html)

## HTTP

### Status codes

[https://en.wikipedia.org/wiki/List\_of\_HTTP\_status\_codes](https://en.wikipedia.org/wiki/List\_of\_HTTP\_status\_codes)

## Requests methods

* [OPTIONS](https://www.gitbook.com/book/asancheza/devops/edit)
* [GET](https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol#Request\_methods)
* [HEAD](https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol#Request\_methods)
* [POST](https://en.wikipedia.org/wiki/POST\_\(HTTP\))
* [PUT](https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol#Request\_methods)
* [DELETE](https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol#Request\_methods)
* [TRACE](https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol#Request\_methods)
* [CONNECT](https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol#Request\_methods)
* [PATCH](https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol#Request\_methods)

## Header fields

* Cookie&#x20;
* ETag&#x20;
* Location&#x20;
* HTTP referer&#x20;
* DNT&#x20;
* X-Forwarded-For

| Accept                                                                                            | Content-Types that are acceptable for the response. See[Content negotiation](https://en.wikipedia.org/wiki/Content\_negotiation).                                                                                                                                                                                                                                                                                                                                                                           | `Accept: text/plain`                                                                                     | Permanent                                                                                                 |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| Accept-Charset                                                                                    | Character sets that are acceptable.                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | `Accept-Charset: utf-8`                                                                                  | Permanent                                                                                                 |
| Accept-Encoding                                                                                   | List of acceptable encodings. See[HTTP compression](https://en.wikipedia.org/wiki/HTTP\_compression).                                                                                                                                                                                                                                                                                                                                                                                                       | `Accept-Encoding: gzip, deflate`                                                                         | Permanent                                                                                                 |
| Accept-Language                                                                                   | List of acceptable human languages for response. See[Content negotiation](https://en.wikipedia.org/wiki/Content\_negotiation).                                                                                                                                                                                                                                                                                                                                                                              | `Accept-Language: en-US`                                                                                 | Permanent                                                                                                 |
| Accept-Datetime                                                                                   | Acceptable version in time.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | `Accept-Datetime: Thu, 31 May 2007 20:35:00 GMT`                                                         | Provisional                                                                                               |
| Authorization                                                                                     | Authentication credentials for HTTP authentication.                                                                                                                                                                                                                                                                                                                                                                                                                                                         | `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`                                                      | Permanent                                                                                                 |
| [Cache-Control](https://en.wikipedia.org/wiki/Cache-Control)                                      | Used to specify directives thatmustbe obeyed by all caching mechanisms along the request-response chain.                                                                                                                                                                                                                                                                                                                                                                                                    | `Cache-Control: no-cache`                                                                                | Permanent                                                                                                 |
| Connection                                                                                        | Control options for the current connection and list of hop-by-hop request fields.[\[7\]](https://en.wikipedia.org/wiki/List\_of\_HTTP\_header\_fields#cite\_note-rfc7230\_connection-7)                                                                                                                                                                                                                                                                                                                     | `Connection: keep-alive`[`Connection: Upgrade`](https://en.wikipedia.org/wiki/HTTP/1.1\_Upgrade\_header) | Permanent                                                                                                 |
| Cookie                                                                                            | An[HTTP cookie](https://en.wikipedia.org/wiki/HTTP\_cookie)previously sent by the server with[Set-Cookie](https://en.wikipedia.org/wiki/List\_of\_HTTP\_header\_fields#innerlink\_set-cookie)(below).                                                                                                                                                                                                                                                                                                       | `Cookie: $Version=1; Skin=new;`                                                                          | Permanent: standard                                                                                       |
| Content-Length                                                                                    | The length of the request body in[octets](https://en.wikipedia.org/wiki/Octet\_\(computing\))(8-bit bytes).                                                                                                                                                                                                                                                                                                                                                                                                 | `Content-Length: 348`                                                                                    | Permanent                                                                                                 |
| Content-MD5                                                                                       | A[Base64](https://en.wikipedia.org/wiki/Base64)-encoded binary[MD5](https://en.wikipedia.org/wiki/MD5)sum of the content of the request body.                                                                                                                                                                                                                                                                                                                                                               | `Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==`                                                                  | Obsolete[\[8\]](https://en.wikipedia.org/wiki/List\_of\_HTTP\_header\_fields#cite\_note-tools.ietf.org-8) |
| Content-Type                                                                                      | The[MIME type](https://en.wikipedia.org/wiki/MIME\_type)of the body of the request (used with POST and PUT requests).                                                                                                                                                                                                                                                                                                                                                                                       | `Content-Type: application/x-www-form-urlencoded`                                                        | Permanent                                                                                                 |
| Date                                                                                              | The date and time that the message was originated (in "HTTP-date" format as defined by[RFC 7231 Date/Time Formats](http://tools.ietf.org/html/rfc7231#section-7.1.1.1)).                                                                                                                                                                                                                                                                                                                                    | `Date: Tue, 15 Nov 1994 08:12:31 GMT`                                                                    | Permanent                                                                                                 |
| Expect                                                                                            | Indicates that particular server behaviors are required by the client.                                                                                                                                                                                                                                                                                                                                                                                                                                      | `Expect: 100-continue`                                                                                   | Permanent                                                                                                 |
| Forwarded                                                                                         | Disclose original information of a client connecting to a web server through an HTTP proxy.[\[9\]](https://en.wikipedia.org/wiki/List\_of\_HTTP\_header\_fields#cite\_note-9)                                                                                                                                                                                                                                                                                                                               | `Forwarded: for=192.0.2.60;proto=http;by=203.0.113.43Forwarded: for=192.0.2.43, for=198.51.100.17`       | Permanent                                                                                                 |
| From                                                                                              | The email address of the user making the request.                                                                                                                                                                                                                                                                                                                                                                                                                                                           | `From: user@example.com`                                                                                 | Permanent                                                                                                 |
| Host                                                                                              | The domain name of the server (for[virtual hosting](https://en.wikipedia.org/wiki/Virtual\_hosting)), and the[TCP port](https://en.wikipedia.org/wiki/List\_of\_TCP\_and\_UDP\_port\_numbers)number on which the server is listening. The[port](https://en.wikipedia.org/wiki/Port\_\(computer\_networking\))number may be omitted if the port is the standard port for the service requested.[\[10\]](https://en.wikipedia.org/wiki/List\_of\_HTTP\_header\_fields#cite\_note-10)Mandatory since HTTP/1.1. | `Host: en.wikipedia.org:8080Host: en.wikipedia.org`                                                      | Permanent                                                                                                 |
| If-Match                                                                                          | Only perform the action if the client supplied entity matches the same entity on the server. This is mainly for methods like PUT to only update a resource if it has not been modified since the user last updated it.                                                                                                                                                                                                                                                                                      | `If-Match: "737060cd8c284d8af7ad3082f209582d"`                                                           | Permanent                                                                                                 |
| If-Modified-Since                                                                                 | Allows a304 Not Modifiedto be returned if content is unchanged.                                                                                                                                                                                                                                                                                                                                                                                                                                             | `If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT`                                                       | Permanent                                                                                                 |
| If-None-Match                                                                                     | Allows a304 Not Modifiedto be returned if content is unchanged, see[HTTP ETag](https://en.wikipedia.org/wiki/HTTP\_ETag).                                                                                                                                                                                                                                                                                                                                                                                   | `If-None-Match: "737060cd8c284d8af7ad3082f209582d"`                                                      | Permanent                                                                                                 |
| If-Range                                                                                          | If the entity is unchanged, send me the part(s) that I am missing; otherwise, send me the entire new entity.                                                                                                                                                                                                                                                                                                                                                                                                | `If-Range: "737060cd8c284d8af7ad3082f209582d"`                                                           | Permanent                                                                                                 |
| If-Unmodified-Since                                                                               | Only send the response if the entity has not been modified since a specific time.                                                                                                                                                                                                                                                                                                                                                                                                                           | `If-Unmodified-Since: Sat, 29 Oct 1994 19:43:31 GMT`                                                     | Permanent                                                                                                 |
| Max-Forwards                                                                                      | Limit the number of times the message can be forwarded through proxies or gateways.                                                                                                                                                                                                                                                                                                                                                                                                                         | `Max-Forwards: 10`                                                                                       | Permanent                                                                                                 |
| Origin                                                                                            | Initiates a request for[cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin\_resource\_sharing)(asks server for an 'Access-Control-Allow-Origin' response field).                                                                                                                                                                                                                                                                                                                     | `Origin: http://www.example-social-network.com`                                                          | Permanent: standard                                                                                       |
| Pragma                                                                                            | Implementation-specific fields that may have various effects anywhere along the request-response chain.                                                                                                                                                                                                                                                                                                                                                                                                     | [`Pragma: no-cache`](https://en.wikipedia.org/wiki/List\_of\_HTTP\_header\_fields#Avoiding\_caching)     | Permanent                                                                                                 |
| Proxy-Authorization                                                                               | Authorization credentials for connecting to a proxy.                                                                                                                                                                                                                                                                                                                                                                                                                                                        | `Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`                                                | Permanent                                                                                                 |
| Range                                                                                             | Request only part of an entity. Bytes are numbered from 0. See[Byte serving](https://en.wikipedia.org/wiki/Byte\_serving).                                                                                                                                                                                                                                                                                                                                                                                  | `Range: bytes=500-999`                                                                                   | Permanent                                                                                                 |
| [Referer](https://en.wikipedia.org/wiki/HTTP\_referer)\[[sic](https://en.wikipedia.org/wiki/Sic)] | This is the address of the previous web page from which a link to the currently requested page was followed. (The word “referrer” has been misspelled in the RFC as well as in most implementations to the point that it has become standard usage and is considered correct terminology)                                                                                                                                                                                                                   | `Referer: http://en.wikipedia.org/wiki/Main_Page`                                                        | Permanent                                                                                                 |
| TE                                                                                                | The transfer encodings the user agent is willing to accept: the same values as for the response header field Transfer-Encoding can be used, plus the "trailers" value (related to the "[chunked](https://en.wikipedia.org/wiki/Chunked\_transfer\_encoding)" transfer method) to notify the server it expects to receive additional fields in the trailer after the last, zero-sized, chunk.                                                                                                                | `TE: trailers,`[`deflate`](https://en.wikipedia.org/wiki/Deflate)                                        | Permanent                                                                                                 |
| User-Agent                                                                                        | The[user agent string](https://en.wikipedia.org/wiki/User\_agent\_string)of the user agent.                                                                                                                                                                                                                                                                                                                                                                                                                 | `User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:12.0) Gecko/20100101 Firefox/21.0`                       | Permanent                                                                                                 |
| [Upgrade](https://en.wikipedia.org/wiki/Upgrade\_header)                                          | Ask the server to upgrade to another protocol.                                                                                                                                                                                                                                                                                                                                                                                                                                                              | `Upgrade: HTTP/2.0, HTTPS/1.3, IRC/6.9, RTA/x11, websocket`                                              | Permanent                                                                                                 |
| Via                                                                                               | Informs the server of proxies through which the request was sent.                                                                                                                                                                                                                                                                                                                                                                                                                                           | `Via: 1.0 fred, 1.1 example.com (Apache/1.1)`                                                            | Permanent                                                                                                 |
| Warning                                                                                           | A general warning about possible problems with the entity body.                                                                                                                                                                                                                                                                                                                                                                                                                                             | `Warning: 199 Miscellaneous warning`                                                                     | Permanent                                                                                                 |
