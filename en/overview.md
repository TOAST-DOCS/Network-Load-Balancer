## Network > Load Balancer > Overview
TOAST provides Load Balancer, by which:

- Throughput can increase by dispersing loads into many instances;
- Availability may increase by automatically excluding faulty or under-maintenance instances from service.

### Types of Load Balancing

Load Balancer supports a total of three types of load balancing:

* Round Robin (consecutive selection): It is the basic and most popular type of load balancing, and can be applied when all member instances respond the same for same requests.  

* Least Connections (least connection preferred): This type selects instances with the least number of TCP connections. That is, identify the load status of instances by the number of TCP connections and send requests to an instance with the least load for equal processing. When applied for extreme change of loads due to requests, this type can prevent unbalanced load concentration.      

* Source IP (selection by original IP): This method hashes source IP of the client and selects the instance to process. Requests from the same IP get to the same instance at all times. It is useful to deal with a user’s request at a same instance, every time it arises.   

### Supported Protocols

Load balancer currently supports the following protocols: 

* TCP
* HTTP
* HTTPS
* TERMINATED_HTTPS

Among those, TERMINATED_HTTPS gets HTTPS traffic and delivers the traffic in HTTP to member instances. With this protocol, the end user and load balancer communicate with HTTPS so as to secure higher security, while the server receives traffic in HTTP and reduces the CPU loads required for decryption.  

> [Note] To use the TERMINATED_HTTPS protocol, a certificate and private key should be registered at a load balancer: the private key must be removed of password so as to be properly operated. 


### Create Load Balancers

Load balancer is created in [Subnet](/Network/VPC/en/overview/#_2) under [VPC](/Network/VPC/en/overview/#_2). When a load balancer is created, an IP is assigned from a designated subnet and serves as its own IP. And, by registering instances that belong to a same VPC as specified subnet, inflow traffic can be distributed. Instances of other VPC cannot be added.

Inbound traffic to be processed at load balancer is defined at the listener. By defining traffic-receiving ports and protocols for each listener, one load balancer can be configured to process various traffic types. In general, the web server has 80 port listener for HTTP traffic and 443 port listener for HTTPS traffic. Many listeners can be registered to a load balancer. 

> [Caution] Listeners of a same receiving port cannot be redundantly created at a load balancer. 

Load Balancer operates in a proxy mode. The client, to send a request, connects to a load balancer, while the load balancer connects to an instance server. From the member instance server’s perspective, session’s source IP is viewed as load balancer IP. To check client IP from the server, make a reference of X-Forwarded-For header information (HTTP/TERMINATED_HTTPS protocol) or use **Proxy Protocol** (TCP/HTTPS protocol).

> [Note] Proxy Mode <br>
> As one of the operating methods of L4 load balancer, proxy mode means server's response delivered to a client through load balancer. That is, a load balancer is connected with client via TCP, and then connected with server via TCP.
>
> With the proxy mode, the load balancer may be serviced differently at the client port from server port. Also a load balancer that belongs to a different subnet may be added.    
>
> Note, however, that resources are wasted because the load balancer is connected redundantly with the client and the server. In other words, it takes two ports of the load balancer for a single connection. Furthermore, the client IP is not easily recognized at the server. Using an HTTP header like X-Forwarded-For or a proxy protocol provided by load balancer can be a solution.   

<br>

> [Note] X-Forwarded-For Header <br>
> As non-standardized HTTP header, it is used by the server to check client's IP. 
HTTP requests through load balancer include the **X-Forwarded-For** key. And the value is the client IP. 
>
> The X-Forwarded-For header can be activated only when the load balancer protocol is set with HTTP or TERMINATED_HTTPS.  

<br>

> [Note] Proxy Protocol <br>
> It is the protocol to deliver IP information of the client when the load balancer uses TCP. Represented with a line of text in the US-ASCII format, the proxy protocol delivers at the initial connection of TCP, while other data delivery is postponed until the receiver gets all data. 
>
> The proxy protocol has 6 subdivisions, and each is divided by whitespace characters. 
> The last character must end with Carriage Return (\r) + Line Feed (\n).
>  ```
>  PROXY INET_PROTCOL CLIENT_IP PROXY_IP CLIENT_PORT PROXY_PORT\r\n
>  ```
>
> | Acronym | ASCII | HEX | Description |
> |--|--|--|--|
> | PROXY | "PROXY" | 0x50 0x52 0x4F 0x58 0x59 | Indicator for a proxy protocol |
> | INET_PROTOCL | "TCP4" or "TCP6" | 0x54 0x43 0x50 0x34 or 0x54 0x43 0x50 0x36 | INET protocol type currently in use |
> | CLIENT_IP | e.g) "192.168.100.101" <br>or, "fe80::a159:b1f3:c346:5975" | 0xC0 0xA8 0x64 0x65 | Departure IP address |
> | PROXY_IP | e.g) "192.168.100.102" <br> or, "fe80::a159:b1f3:c346:5976" | 0xC0 0xA8 0x64 0x66 | Destination IP address |
> | CLIENT_PORT | e.g) "43179" | 0xA8 0xAB | Departure port |
> | PROXY_PORT | e.g) "80" | 0x80 | Destination port |
>
> Examples of proxy protocol are as follows: 
>
> - "PROXY TCP4 255.255.255.255 255.255.255.255 65535 65535\r\n": TCP/IPv4
> - "PROXY TCP6 ffff:f...f:ffff ffff:f...f:ffff 65535 65535\r\n": TCP/IPv6
> - "PROXY UNKNOWN\r\n": Unknown connection

### Restricted Connection

To ensure QoS, load balancer restricts the number of maintainable connection per listener. If requests exceed a restricted value, they are queued and can be processed after previous requests are completed. Or, requests may be forced to stop, as they may not even be queued due to loads or timed out from server/client. This is when the client experiences unexpected response delay. Therefore, take a cautious approach to select your restriction value.  

> [Note] The restriction value currently supported by load balancer is ranged between 2000 and 60000, and you are free to decide within the range. To readjust restriction, contact administrator. 

### Session Persistence

When there is a need to maintain user information or a client’s request must be delivered to a particular server only, load balancer session persistence can be enabled. This function enables a server which processed a client’s request to continue to deal with the client’s further requests. When Source IP is selected for load balancing, the server can be determined upon client’s IP, so as to provide session persistence. If Round Robin or Least Connection is opted for load balancing, following session persistence becomes available.

Following are the types of session persistence supported by load balancer: 

* No Session Persistence (no session persistence): Session persistence is not applied. 
* Source IP (session management by original IP): Session is persisted by source IP of the client. To this end, the mapping table between an instance, selected by load balancer at the initial request, and the original IP must be saved internally. Then, for subsequent requests with the same original IP, check the mapping table and deliver to the instance that responded to the initial request. Load balancer can save up to 10000 mappings for an original IP. This method is recommended to configure session persistence at the TCP protocol listener.  
* APP Cookie (session management by application): Session can persist by configuring a specific cookie inserted by the server. For an initial request, the server must deliver through the **Set-Cookie** header of HTTP to set its own cookie value.  Here, the load balancer checks if there is any specific cookie while responding to the server, and if there is one, mapping between the cookie and the server ID can persist internally. Afterwards, when the client inserts a cookie indicating a specific server to the **Cookie** header, the load balancer delivers the request to a server that can respond to the cookie. If the cookie-server ID mapping is not used for three hours, it is automatically deleted. 
* HTTP Cookie (session management by load balancer): Similar to APP Cookie, but this type of persistence is made available via cookies that are automatically set by load balancer. Load balancer adds a cookie called **SRV** to respond to the server: the **SRV** value refers to original ID of each server.  When the client inserts **SRV** to the cookie, request is delivered to the server which responded first. 

[Note] TCP session maintenance time can be set for load balancer. By setting **Keepalive timeout** value, session persistence can be adjusted between client and load balancer, and load balancer and server. 

### Check Instance Status

TOAST Load Balancer conducts status checks periodically to see if member instances operate properly. Status check is conducted by confirming if responses come as specified protocol. If an instance does not send a normal response within specified number or time, it is considered abnormal and excluded from load dispersion. As such, service can be provided without suspension even with unexpected faults or maintenance.  

Status check protocols provided by load balancer are TCP, HTTP, and HTTPS. To check status more precisely, various methods can be applied for each protocol use. 

### Pricing

The load balancer has two pricing policies: 

 * Usage Price: Charged as the usage time of load balancer. Does not charge if the status is not **ACTIVE**.
 * Traffic Price: Charged as much as the load balancer traffic volume, adding to the whole project traffic.
