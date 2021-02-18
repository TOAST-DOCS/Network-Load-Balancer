## Network > Load Balancer > Overview
NHN Cloud provides Load Balancer, by which:

- Throughput can increase by dispersing loads into many instances;
- Availability may increase by automatically excluding faulty or under-maintenance instances from service.

## Types of Load Balancing

Load Balancer supports a total of three types of load balancing:

* Round Robin (consecutive selection): It is the basic and most popular type of load balancing, and can be applied when all member instances respond the same for same requests.  

* Least Connections (least connection preferred): This type selects instances with the least number of TCP connections. That is, identify the load status of instances by the number of TCP connections and send requests to an instance with the least load for equal processing. When applied for extreme change of loads due to requests, this type can prevent unbalanced load concentration.      

* Source IP (selection by original IP): This method hashes source IP of the client and selects the instance to process. Requests from the same IP get to the same instance at all times. It is useful to deal with a user’s request at a same instance, every time it arises.   

## Supported Protocols

Load balancer currently supports the following protocols: 

* TCP
* HTTP
* HTTPS
* TERMINATED_HTTPS

Among those, TERMINATED_HTTPS gets HTTPS traffic and delivers the traffic in HTTP to member instances. With this protocol, the end user and load balancer communicate with HTTPS so as to secure higher security, while the server receives traffic in HTTP and reduces the CPU loads required for decryption.  

> [Note] To use the TERMINATED_HTTPS protocol, a certificate and private key should be registered at a load balancer: the private key must be removed of password so as to be properly operated. 

## SSL/TLS Version for Load Balancer
* When creating a load balancer using the TERMINATED_HTTPS protocol, you may select a Secure Socket Layer (SSL) / Transport Layer Security (TLS) version for communication between client and load balancer.
*  A lower SSL/TLS protocol version may be less secure and the algorithms comprising the cipher suite may be more vulnerable, too; hence, it is recommended to choose the highest possible version of SSL/TLS.
* NHN Cloud load balancer is about to support TLS v1.3.

### TLS Version
Select a TLS version to create load balancer. The load balancer communicates with clients by using a selected version at its highest version only.

| SSL/TLS Version Setting | TLS Version for Load Balancer |
| -- | -- |
| SSLv3 | SSLv3, TLSv1.0, TLSv1.1, TLSv1.2 |
| TLSv1.0 | TLSv1.0, TLSv1.1, TLSv1.2 |
| TLSv1.0_2016 | TLSv1.0, TLSv1.1, TLSv1.2 |
| TLSv1.1 | TLSv1.1, TLSv1.2 |
| TLSv1.2 | TLSv1.2 |

### Cipher Suite for Each SSL/TLS Version
* A cipher suite is a set of cryptographic algorithms used for HTTPS communications, including key exchanges between client and load balancer,   certificate authentication, message encryption, and message integrity checks.
* Each SSL/TLS version applies the following cipher suite.
* For a higher version of TLS, cipher suites that adopt less secure algorithms are not used.

| SSL/TLS Version Setting | Cipher Suites that are Used | Remarks |
| -- | -- | -- |
| SSLv3 | ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>ECDHE-RSA-AES256-SHA<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256<br>AES256-SHA<br>AES128-SHA<br>DES-CBC3-SHA<br>RC4-MD5 | |
| TLSv1.0 | ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>ECDHE-RSA-AES256-SHA<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256<br>AES256-SHA<br>AES128-SHA<br>DES-CBC3-SHA | RC4-MD5 is excluded |
| TLSv1.0_2016 | ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>ECDHE-RSA-AES256-SHA<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256<br>AES256-SHA<br>AES128-SHA | DES-CBC3-SHA is excluded |
| TLSv1.1 | ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>ECDHE-RSA-AES256-SHA<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256<br>AES256-SHA<br>AES128-SHA | Same as above |
| TLSv1.2 | ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256 | ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-SHA<br>AES256-SHA<br>AES128-SHA is excluded |

## Create Load Balancers

Load balancer is created in [Subnet](/Network/VPC/en/overview/#_2) under [VPC](/Network/VPC/en/overview/#_2). When a load balancer is created, an IP is assigned from a designated subnet and serves as its own IP. And, by registering instances that belong to a same VPC as specified subnet, inflow traffic can be distributed. Instances of other VPC cannot be added.

Inbound traffic to be processed at load balancer is defined at the listener. By defining traffic-receiving ports and protocols for each listener, one load balancer can be configured to process various traffic types. In general, the web server has 80 port listener for HTTP traffic and 443 port listener for HTTPS traffic. Many listeners can be registered to a load balancer. 

> [Caution] Listeners of a same receiving port cannot be redundantly created at a load balancer. 

## Load Balancer Proxy Mode
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

## Restricted Connection

To ensure QoS, load balancer restricts the number of maintainable connection per listener. If requests exceed a restricted value, they are queued and can be processed after previous requests are completed. Or, requests may be forced to stop, as they may not even be queued due to loads or timed out from server/client. This is when the client experiences unexpected response delay. Therefore, take a cautious approach to select your restriction value.  

> [Note] The restriction value currently supported by load balancer is ranged between 2000 and 60000, and you are free to decide within the range. To readjust restriction, contact administrator. 

## Session Persistence

When there is a need to maintain user information or a client’s request must be delivered to a particular server only, load balancer session persistence can be enabled. This function enables a server which processed a client’s request to continue to deal with the client’s further requests. When Source IP is selected for load balancing, the server can be determined upon client’s IP, so as to provide session persistence. If Round Robin or Least Connection is opted for load balancing, following session persistence becomes available.

Following are the types of session persistence supported by load balancer: 

* No Session Persistence (no session persistence): Session persistence is not applied. 
* Source IP (session management by original IP): Session is persisted by source IP of the client. To this end, the mapping table between an instance, selected by load balancer at the initial request, and the original IP must be saved internally. Then, for subsequent requests with the same original IP, check the mapping table and deliver to the instance that responded to the initial request. Load balancer can save up to 10000 mappings for an original IP. This method is recommended to configure session persistence at the TCP protocol listener.  
* APP Cookie (session management by application): Session can persist by configuring a specific cookie inserted by the server. For an initial request, the server must deliver through the **Set-Cookie** header of HTTP to set its own cookie value.  Here, the load balancer checks if there is any specific cookie while responding to the server, and if there is one, mapping between the cookie and the server ID can persist internally. Afterwards, when the client inserts a cookie indicating a specific server to the **Cookie** header, the load balancer delivers the request to a server that can respond to the cookie. If the cookie-server ID mapping is not used for three hours, it is automatically deleted. 
* HTTP Cookie (session management by load balancer): Similar to APP Cookie, but this type of persistence is made available via cookies that are automatically set by load balancer. Load balancer adds a cookie called **SRV** to respond to the server: the **SRV** value refers to original ID of each server.  When the client inserts **SRV** to the cookie, request is delivered to the server which responded first. 

[Note] TCP session maintenance time can be set for load balancer. By setting **Keepalive timeout** value, session persistence can be adjusted between client and load balancer, and load balancer and server. 

## Check Instance Status

NHN Cloud Load Balancer conducts status checks periodically to see if member instances operate properly. Status check is conducted by confirming if responses come as specified protocol. If an instance does not send a normal response within specified number or time, it is considered abnormal and excluded from load dispersion. As such, service can be provided without suspension even with unexpected faults or maintenance.  

Status check protocols provided by load balancer are TCP, HTTP, and HTTPS. To check status more precisely, various methods can be applied for each protocol use. 

## Statistical Functions of Load Balancer 

You can find many statistical indicators relevant to network flows processed by load balancer on charts. Features of statistics of NHN Cloud Load Balancer are as follows:   

* Provide charts of statistics by load balancer, or listener 
* Classify periods by the hour, 24 hour, 1 week, 1 month or others as specified.  
* Provide statistical volume of load balancer by client or instance on different charts.  
* Provide instance statistics by member instance or collection results only. (View by Instance: On/OFF)

Following charts are provided: 

| Statistics Indicator Name <br>(Chart Name) | Type | Unit | Description |
|--|--|--|--|
| Number of Client Sessions | Client | ea | Number of Sessions where Load Balancer is Attached to Client |
| Client Session CPS | Client | cps<br>(connections per second) | Number of Sessions Newly Connected with Client for a Second |
| Session CPS | Instance | cps<br>(connections per second) | Number of Sessions Newly Connected with Instance for a Second |
| Traffic In | Instance | bps<br>(bits per second) | Volume of Load Balancer Traffic Sent to Instance |
| Traffic Out | Instance | bps<br>(bits per second) | Volume of Instance Traffic Sent to Load Balancer |
| Number of Load Balancer Exceptions | Instance | ea | Number of Exceptions from Load Balancer due to Failed Health Checks |

> [Note] Restraints and References 
>
> * Statistical charts are provided only for load balancer, listener, or member, which are currently enabled. When the load balancer resource is removed, its past statistics shall not be provided.   
> * A measure may mean differently depending on the period set in charts in which the unit is ea. To find out the meaning, put your mouse on a question mark at top of each chart.    
> * Measures displayed on charts for indicators related to network usage, such as Traffic In or Traffic Out, refer to payload transmission size, divided by unit time, with L2, L3, and L4 headers excluded.   
> * Statistical data are provided for up to 1 year. 

## Load Balancer IP Access Control 

To control packets into load balancer, you can apply features of IP access control.
Such features are different from [Security Group](/Network/VPC/en/console-guide/#_6), as follows: 

> [Reference] Comparison between Security Group and Load Balancer IP Access Control 
>
> | Category | Security Group | Load Balancer IP Access Control | Remarks |
> |--|--|--|--|
> | Control Targets | Instance | Load Balancer | |
> | Configuration Targets | Configure IP and Port | Configure IP Only | Block traffic other than ports configured at a load balancer |
> | Control Traffic | Select Inbound/Outbound <br> Traffic | Control inbound traffic only |
> | Access Control Type | Configure Allow Policy Only | Select Allow or Deny policy |

Configuration of security group and load balancer IP access control are not mutually influential. Therefore, apply security group to control inbound or outbound traffic in and out of an instance, while using IP access control to control inbound traffic to load balancer.   

To apply IP access control, configure as follows: 

### IP Access Control Groups
* Up to 10 groups can be created for a project. 
* A group has attributes, such as name, memo, and access control type. 
* Access control type can have either Allow or Deny. 
* More IP access control targets can be added for an access control group.
* When an IP access control group is deleted, all its IP access control targets are deleted; control is not performed over such IPs in all load balancers where the group has been applied to. 

### IP Access Control Type
* 'Allow': Access of IPs of a group is <b>Allowed</b>, while that of all the other IPs are <b>Denied</b>.
* 'Deny': Access of IPs of a group is <b>Denied</b>, while that of all the other IPs are <b>Allowed</b>.
> [Caution]
> To apply ‘Allow’-type access control group to a load balancer, member instance IP of the load balancer must be added as access control target.


### IP Access Control Targets
* Up to 1,000 access control targets can be created for a project. 
* An access control target has attributes, such as memo and IP address. 
* One access control target can have IP address range in the IP address or CIDR format. Enter IP address range in the CIDR format, and all bandwidth of the network are to be included as access control target. 

> [Reference]
> Check threatening remote IP from [NHN Cloud Security Monitoring](/Security/Security%20Monitoring/en/Overview/). 
>
> Create an IP access control group in the ‘Deny’ type of IP access control type, and add detected threatening IP as IP access control targets; then, security of the system can be enforced.
>

### Apply IP Access Control Groups
* An access control group can be applied to many load balancers. 
* Many access control groups can be applied to a load balancer. However, such binding groups must have same access control type. 
* Load balancers not applied by IP access control group allow access of all IPs. 

> [Reference]
>* Operations when load balancer and IP access control are changed
>     * When load balancer is deleted, access control binding is deleted; but, access control group is not deleted.  
>     * When access control group is deleted, it is reflected on all load balancers which are bound with the group. 
>     * When access control targets are added or deleted within an access control group, it is reflected on all load balancers which are bound with the group. 


### Pricing

The load balancer has two pricing policies: 

 * Usage Price: Charged as the usage time of load balancer. Does not charge if the status is not **ACTIVE**.
 * Traffic Price: Charged as much as the load balancer traffic volume, adding to the whole project traffic.
