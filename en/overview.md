## Network > Load Balancer > Overview

NHN Cloud provides a load balancer, which enables you to achieve the following:

- Increase the throughput by distributing loads that are difficult to handle with one instance to multiple instances.
- Increase availability by automatically excluding the instances that have failed or are under maintenance from service.


## Load Balancing Methods

Load Balancer supports a total of three load balancing methods:

* Round Robin (select sequentially): This is the most basic and popular load balancing method that sequentially selects instances to forward traffic to. This method can be used when all member instances make the same response to the same request.

* Least Connections (select the least connections first): This method selects the instance with the smallest number of current TCP connections. That is, it identifies the load status of instances based on the number of TCP connections and sends requests to the instance with the least load among the members so that requests are processed as evenly as possible. If you apply the method when the processing load caused by the requests fluctuates greatly, you can avoid a situation in which the load is concentrated on a specific instance.

* Source IP (select by the source IP): This method selects the instance to process requests by hashing the source IP of the requester. When this method is used, requests coming from the same IP are always forwarded to the same instance. This is useful when you want the same instance to handle requests from a specific user every time.


## Supported Protocols

Load Balancer supports the following protocols:

* TCP
* HTTP
* HTTPS
* TERMINATED_HTTPS

Among the above protocols, the TERMINATED_HTTPS protocol receives HTTPS traffic and forwards it to member instances as HTTP traffic. When the TERMINATED_HTTPS protocol is used, you can ensure high security by communicating over HTTPS between the end user and the load balancer, and reduce the CPU load for decryption by passing HTTP traffic to the server.

> [Note] To use the TERMINATED_HTTPS protocol, a certificate and private key must be registered with the load balancer. The private key that is registered works correctly only when the password is removed.

## SSL/TLS Version for Load Balancer
* When you create a load balancer that uses the TERMINATED_HTTPS protocol, you can select the version of Secure Socket Layer/Transport Layer Security (SSL/TLS) used for communication between clients and the load balancer.
* Because a lower SSL/TLS protocol version may have security flaws and the cryptographic algorithms that make up the cipher suite are also less secure, it is recommended to select the highest SSL/TLS version supported by the client.

### SSL/TLS Version
Select one of the SSL/TLS versions to create a load balancer. The created load balancer communicates with clients using only the selected version and versions higher than the selected version, as shown below.

| SSL/TLS Version Setting | SSL/TLS Version Used by Load Balancer |
| -- | -- |
| SSLv3 | SSLv3, TLSv1.0, TLSv1.1, TLSv1.2, TLSv1.3 |
| TLSv1.0 | TLSv1.0, TLSv1.1, TLSv1.2, TLSv1.3 |
| TLSv1.0_2016 | TLSv1.0, TLSv1.1, TLSv1.2, TLSv1.3 |
| TLSv1.1 | TLSv1.1, TLSv1.2, TLSv1.3 |
| TLSv1.2 | TLSv1.2, TLSv1.3 |
| TLSv1.3 | TLSv1.3 |


### Cipher Suites by SSL/TLS Version
* A cipher suite refers to a set of cryptographic algorithms used for HTTPS communications, including key exchange between clients and the load balancer, certificate validation, message encryption, and message integrity checking.
* The cipher suites used depending on the SSL/TLS version are shown below.
* If you choose a higher TLS version, cipher suites that use less secure algorithms are not used.

| SSL/TLS Version Setting | Cipher Suites Used | Note |
| -- | -- | -- |
| SSLv3 | TLS-AES-128-GCM-SHA256<br>TLS-AES-256-GCM-SHA384<br>TLS-CHACHA20-POLY1305-SHA256<br>ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>ECDHE-RSA-AES256-SHA<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256<br>AES256-SHA<br>AES128-SHA<br>DES-CBC3-SHA<br>RC4-MD5 | |
| TLSv1.0 | TLS-AES-128-GCM-SHA256<br>TLS-AES-256-GCM-SHA384<br>TLS-CHACHA20-POLY1305-SHA256<br>ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>ECDHE-RSA-AES256-SHA<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256<br>AES256-SHA<br>AES128-SHA<br>DES-CBC3-SHA | RC4-MD5 is excluded |
| TLSv1.0_2016 | TLS-AES-128-GCM-SHA256<br>TLS-AES-256-GCM-SHA384<br>TLS-CHACHA20-POLY1305-SHA256<br>ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>ECDHE-RSA-AES256-SHA<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256<br>AES256-SHA<br>AES128-SHA | DES-CBC3-SHA is excluded |
| TLSv1.1 | TLS-AES-128-GCM-SHA256<br>TLS-AES-256-GCM-SHA384<br>TLS-CHACHA20-POLY1305-SHA256<br>ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>ECDHE-RSA-AES256-SHA<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256<br>AES256-SHA<br>AES128-SHA | Same as above |
| TLSv1.2 | TLS-AES-128-GCM-SHA256<br>TLS-AES-256-GCM-SHA384<br>TLS-CHACHA20-POLY1305-SHA256<br>ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256 | ECDHE-RSA-AES128-SHA<br>ECDHE-RSA-AES256-SHA<br>AES256-SHA<br>AES128-SHA is excluded |
| TLSv1.3 | TLS-AES-128-GCM-SHA256<br>TLS-AES-256-GCM-SHA384<br>TLS-CHACHA20-POLY1305-SHA256 | ECDHE-RSA-AES128-GCM-SHA256<br>ECDHE-RSA-AES128-SHA256<br>ECDHE-RSA-AES256-GCM-SHA384<br>ECDHE-RSA-AES256-SHA384<br>AES128-GCM-SHA256<br>AES256-GCM-SHA384<br>AES128-SHA256 is excluded |

## Create Load Balancers

Load balancers can be created with an IP automatically assigned within the [VPC's](/Network/VPC/en/overview/#_2) [subnet](/Network/VPC/en/overview/#_2), or you can specify an IP.

* Automatically assigned IP: Uses one of the available IPs on the subnet as the IP for the load balancer.
* Specify an IP: Uses the specified IP as the IP for the load balancer. The IP must be within the CIDR range of the subnet.

The load balancer registers instances as members to distribute incoming traffic. Members can be registered in two ways

* Instance: You can add instances that belong to this VPC and VPCs that are peered with the VPC as members.
* IP address: You can register members by entering their IP directly. In this case, the communication path between the load balancer and the instance must be set appropriately.

The traffic that flows into the load balancer is defined by listeners. By defining the ports and protocols on which to receive traffic per listener, you can set up a single load balancer to handle a wide variety of traffic. Generally, you would set up a port 80 listener on your web server to listen for HTTP traffic and a port 443 listener to listen for HTTPS traffic. You can register multiple listeners on one load balancer.

> [Caution] You cannot create duplicate listeners with the same listening port on a load balancer.

## L7 rules

The load balancer can perform load balancing based on L7 data. When you select an L7 routing template to create a load balancer, you can create a load balancer with L7 policies.
The available actions are as follows.

* Forward to target group: Sends to a set target group when matched to an L7 rule. You can route packets to specific target groups based on L7 data.
* Forward to URL: Redirects to a set URL when matched to an L7 rule. Redirection is performed by using Location of the HTTP header.
* Block: Blocks when matched to an L7 rule. Returns a response as Forbidden (403).

## Load Balancer Proxy Mode

Load Balancer operates in a `proxy mode`. The client connects to a load balancer to send a request, while the load balancer connects to an instance server. From the member instance server’s perspective, session’s source IP is viewed as the load balancer IP. To check client IP from the server, refer to `X-Forwarded-For` header information (HTTP or TERMINATED_HTTPS protocol) or use `Proxy Protocol` (TCP or HTTPS protocol).

> [Note] Proxy Mode <br>
> When the load balancer operates in proxy mode, the load balancer may serve differently for the port requested by the client and the port served by the server side. In addition, a function to reduce the server load such as TERMINATED_HTTPS can be provided, and the amount of traffic sent to the client can be provided in the form of statistics. (Statistics function to be added)
>

<br>

> [Note] X-Forwarded-For Header <br>
> This is a non-standard HTTP header, which is used by the server to check the client's IP.
HTTP requests coming in through the load balancer include the **X-Forwarded-For** key. Its value is the IP address of the client.
>
> The X-Forwarded-For header is enabled only when the load balancer protocol is set to HTTP or TERMINATED_HTTPS.

<br>

> [Note] Proxy Protocol <br>
> This is a protocol for transmitting IP information of the client side when the load balancer uses TCP. It is expressed as a single line of text in US-ASCII format for human readability. When a TCP connection is established, it is transmitted once for the first time, and other data transmission is delayed until the receiving end receives all data.
>
> The proxy protocol is divided into 6 entries. Each entry is separated by a space character.
> The last character must end with Carriage Return (\r) + Line Feed (\n).
>  ```
>  PROXY INET_PROTCOL CLIENT_IP PROXY_IP CLIENT_PORT PROXY_PORT\r\n
>  ```
>
> | Acronym | ASCII | HEX | Description |
> |--|--|--|--|
> | PROXY | "PROXY" | 0x50 0x52 0x4F 0x58 0x59 | Indicator for a proxy protocol |
> | INET_PROTOCL | "TCP4" or "TCP6" | 0x54 0x43 0x50 0x34 or 0x54 0x43 0x50 0x36 | INET protocol type currently in use |
> | CLIENT_IP | Example: "192.168.100.101" <br>or, "fe80::a159:b1f3:c346:5975" | 0xC0 0xA8 0x64 0x65 | Source IP address |
> | PROXY_IP | Example: "192.168.100.102" <br> or, "fe80::a159:b1f3:c346:5976" | 0xC0 0xA8 0x64 0x66 | Destination IP address |
> | CLIENT_PORT | Example: "43179" | 0xA8 0xAB | Source port |
> | PROXY_PORT | Example: "80" | 0x80 | Destination port |
>
> Examples of the proxy protocol are as follows:
>
> - "PROXY TCP4 255.255.255.255 255.255.255.255 65535 65535\r\n": TCP/IPv4
> - "PROXY TCP6 ffff:f...f:ffff ffff:f...f:ffff 65535 65535\r\n": TCP/IPv6
> - "PROXY UNKNOWN\r\n": Unknown connection
>
> If you are using the TCP or HTTPS protocol, you can set up a proxy protocol on the load balancer to check the client IP address. In this case, the server must also have the capability to recognize the proxy protocol like the ones shown above.
>


## Session Connection Limits

To ensure QoS, the load balancer limits the number of concurrent connections per listener. If the number of incoming requests exceeds the specified connection limit value, the requests are queued in a queue inside the load balancer and processed after previous requests are completed. In addition, requests can be terminated forcibly if the queue is full or a server/client times out. In this case, the client side may experience unexpected response delays.

> [Note] The maximum number of session connection limits are as follows: 60,000 for a general load balancer, 480,000 for a dedicated load balancer, 1,000,000 for a physical Basic load balancer, and 3,000,000 for a physical Premium load balancer.

## Session Persistence

You can take advantage of the load balancer's session persistence feature when there is a need to maintain user information or a client’s request must be forwarded to a specific server only. This feature enables a server that processed a client’s request to continue processing the client’s further requests. If you select Source IP as the load balancing method, session persistence is provided because it determines the server based on the IP of the client. If you use Round Robin or Least Connection as the load balancing method, you can use the following session persistence features.

* No Session Persistence (not maintaining sessions): A method that does not maintain a session.

* Source IP (session management by source IP): A method of maintaining a session based on the source IP of the requester. For this purpose, the mapping table between the source IP and the instance selected by the load balancing method at the time of the initial request is kept internally. Afterwards, when a request with the same source IP comes in, it checks the mapping table and forwards it to the instance that responded to the first request. The load balancer can store mappings for up to 10000 source IPs. If you want to set up a TCP protocol listener to maintain a session, you must use this method.

* APP Cookie (session management by application): A method of maintaining a session through explicit cookie setting from the server side. For the initial request, the server must forward a message to set the cookie value set for itself through the **Set-Cookie** header of HTTP. At this time, the load balancer checks whether there is a specified cookie among the server response, and if there is a cookie, internally maintains the mapping between the cookie and the server ID. After that, when the client puts a cookie pointing to a specific server in the **Cookie** header and sends it, the load balancer forwards the request to the server corresponding to the cookie. The load balancer automatically deletes the cookie-server ID mapping after 3 hours of inactivity.

* HTTP Cookie (session management by load balancer): This is similar to the APP Cookie method, but maintains the session through a cookie that is automatically set by the load balancer. The load balancer adds a cookie called **SRV** to the server's response and sends it. Here, the value of the **SRV** cookie is a unique ID for each server. When a client sends an **SRV** in a cookie, the request is forwarded to the server that responded at first.

[Note] You can set the TCP session keep-alive time on the load balancer. By setting the keepalive timeout value, you can adjust the session maintenance time between the client and the load balancer and between the load balancer and the server.


## Invalid Request Blocking

This feature blocks HTTP request headers if they contain invalid characters. HTTP request headers with invalid characters may be sent by a hacker trying to exploit the server's vulnerability or via a browser affected by bugs. When this feature is enabled, the load balancer blocks HTTP requests with invalid characters to prevent them from being transferred to an instance and sends 400 response code (bad request) to the client.


## Instance Health Check

NHN Cloud Load Balancer periodically tries checking the status of the instances registered as members to ensure that they operate normally. The health check is done by checking whether a expected response comes according to the specified protocol. If a normal response does not come within the specified number of times or duration, the instance is regarded as abnormal and excluded from the target of load balancing. This function enables uninterrupted service to be provided even in case of unexpected failure or maintenance.

The load balancer supports TCP, HTTP, and HTTPS as health check protocols. For precise health check, various health check methods can be set when using each protocol.


## Statistics Function of Load Balancer

You can find many statistical indicators relevant to network flows processed by load balancer on charts. Features of statistics of NHN Cloud Load Balancer are as follows:

* Provide charts of statistics by load balancer, or listener
* Classify periods by the hour, 24 hour, 1 week, 1 month or other specified period.
* Provide statistical volume of load balancer by client or instance on different charts.
* Provide instance statistics view by member instance or aggregated results only. (View by Instance: On/OFF)

The following charts are provided:

| Statistics Indicator Name <br>(Chart Name) | Type | Unit | Description |
|--|--|--|--|
| Client Session Count | Client | ea | Number of sessions where Load Balancer is connected with clients |
| Client Session CPS | Client | cps<br>(connections per second) | Number of sessions newly connected with clients for a second |
| Session CPS | Instance | cps<br>(connections per second) | Number of sessions newly connected with instances for a second |
| Traffic In | Instance | bps<br>(bits per second) | Volume of traffic sent from Load Balancer to instances |
| Traffic Out | Instance | bps<br>(bits per second) | Volume of traffic sent from instances to Load Balancer |
| Load Balancing Exclusion Count | Instance | ea | Number of exclusion from load balancing targets due to a health check failure |

> [Note] Restraints and Notes
>
> * Statistics charts are provided only for the currently used load balancers, listeners, or members. When the load balancer resource is removed, its past statistics data is not provided.
> * In the charts with the ea unit, the meaning of the figure may vary depending on the set period. You can find out the meaning of the figures by hovering the mouse over the question marks at the top of the individual charts.
> * In indicators related to network usage such as Traffic In and Traffic Out, the figures expressed in the chart are data obtained by dividing the payload transmission size excluding the sizes of L2, L3, and L4 headers by the unit time. Therefore, the figures displayed in the chart are irrelevant to the billing data.
> * Statistics data are provided for up to 1 year.


## Load Balancer IP Access Control

To control packets flowing into the load balancer, you can use the IP access control feature.
This feature is different from [Security Group](/Network/VPC/en/console-guide/#_6), and the differences are as follows:

> [Note] Comparison between Security Group and Load Balancer IP Access Control
>
> | Category | Security Group | Load Balancer IP Access Control | Note |
> |--|--|--|--|
> | Control Target | Instance | Load Balancer | |
> | Configuration Target | Configure IP and port | Configure IP Only | 	Traffic from ports other than the ports set on the load balancer is blocked by default |
> | Control Traffic | Incoming/outgoing traffic<br>selectable | Only incoming traffic can be controlled |
> | Access Control Type | Set Allow policy only | Allow or Deny policy selectable |

Security group settings and load balancer IP access control settings do not affect each other. Therefore, you need to use security groups to control incoming/outgoing traffic to/from your instances, and use IP access control to control incoming traffic to your load balancer.

To use the IP access control, you must set the following.

### IP Access Control Groups
* Up to 10 groups can be created for a project.
* A group has attributes, such as name, memo, and access control type.
* Access control type can have either Allow or Deny.
* More IP access control targets can be added for an access control group.
* When an IP access control group is deleted, all of its IP access control targets are deleted. Access control for the IP addresses is not performed in all load balancers to which the group has been applied.

### IP Access Control Type
* 'Allow': <b>Allow</b> access from IPs belonging to the group, and <b>deny</b> access from all other IPs.
* 'Deny': <b>Deny</b> access from IPs belonging to the group, and <b>allow</b> access from all other IPs.
> [Caution]
> To apply 'Allow' type access control group to a load balancer, member instance IP of the load balancer must be added as access control target.


### IP Access Control Targets
* Up to 1,000 access control targets can be created for a project.
* An access control target has attributes, such as memo and IP address.
* One access control target can have an IP address or an IP address range in the CIDR format. If you enter an IP address range in the CIDR format, all range in the network is included in the access control target.

> [Note]
> You can use the [NHN Cloud Security Monitoring](/Security/Security%20Monitoring/en/Overview/) to find out the thread remote IP addresses.
>
> You can enhance the system security by creating an IP access control group with the 'Deny' IP access control type, and adding detected threat remote IP addresses to access control targets.
>

### Applying IP Access Control Groups
* One access control group can be applied to multiple load balancers.
* Multiple access control groups can be applied to a load balancer. However, the groups bound together must have the same access control type.
* A load balancer to which no IP access control group is applied allows access from all IPs.

> [Note]
>
> * Behavior when changing a load balancer or IP access control
>     * If you delete a load balancer, access control binding is deleted, but access control groups are not deleted.
>     * If you delete an access control group, the change is reflected in all load balancers bound to the group.
>     * If you add or delete an access control target within an access control group, the change is reflected in all load balancers bound to the group.


## Pricing

Load Balancer has two pricing policies:

* Load Balancer Usage Price: You will be charged by the usage period of a load balancer in the **ACTIVE** state. For details on the usage fee, refer to [Prices by load balancer type](https://www.toast.com/service/network/load-balancer).
* Load Balancer Traffic Price: The volume of outgoing traffic from the load balancer is added to the overall project traffic and they are billed together.
