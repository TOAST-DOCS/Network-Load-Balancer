## Network > Load Balancer > Console Guide
## Load Balancer Management
### Create Load Balancers 
A load balancer can be easily created only by entering values in the TOAST Load Balancer console. 

#### Load Balancer Information 
Enter basic information required for a load balancer as follows: 

* Name: Enter name of the load balancer.
* Description: Describe the load balancer.
* Network (Subnet): Specify the subnet to which load balancer is to be connected.
* Load Balancer Type : You can choose between General and Private.

#### Register Listeners 
Define attributes of the traffic to be processed by a load balancer. TOAST Load Balancer, by default, has one listener, which may be added or deleted later in detail pages. 

* Load Balancing Type: Determines how a load balancer disperses traffic. Choose one of ROUND_ROBIN, LEAST_CONNECTIONS, or SOURCE_IP. 
* Protocol: Specify the protocol of traffic to be processed by load balancer. Choose one of TCP, HTTP, HTTPS, or TERMINATED_HTTPS.
* Load Balancer Port: Specify the port for default listener to receive traffic.
* Instance Port: Specify the port for load balancer members. Inbound traffic to the load balancer port shall be delivered to the instance port of a member instance.  

> [Note] Each load balancer port and instance port has a value between 1 and 65535.  

> [Caution] Load balancer port, instance port, and protocol cannot be changed after listener is created. 

* SSL Certificate: Register a certificate to be used when TERMINATED_HTTPS is selected as protocol: enabled only when the protocol is TERMINATED_HTTPS. 

> [Note] Registering TERMINATED_HTTPS Certificates 
>
> When TERMINATED_HTTPS is specified as listener protocol for load balancer, a register button for SSL certificate is activated. 
>
> Files to register are ‘Certificate’ and ‘Private Key’. ‘Private Key’ refers to a private key which is paired with a public key embedded in the server certificate. 
>
> The ‘Certificate’ must follow the x.509 PEM format as below:  
>
>     -----BEGIN CERTIFICATE-----
>     (omitted)
>     -----END CERTIFICATE-----
>
>
> To register a server certificate, as well as chain certificate and intermediate certificate, altogether, they must be created and registered on a single file.  
>
> To create a single file for certificates, describe server certificate on top of the file, followed by chain certificate. The chain certificate can be described, regardless of the order. 
>
> A server certificate and two chain certificates can be created on a single certificate file, in the following format. 
>
>
>      -----BEGIN CERTIFICATE-----
>      (Server Certificate, omitted)
>      -----END CERTIFICATE-----
>      -----BEGIN CERTIFICATE-----
>      (Chain Certificate #1, omitted)
>      -----END CERTIFICATE-----
>      -----BEGIN CERTIFICATE-----
>      (Chain Certificate #2, omitted)
>      -----END CERTIFICATE-----
>
>
>
> ‘Private Key’ is a counterpart to a public key which is included in a server certificate. 
>
> You may register files in the format of PKCS#1 or PKCS#8 PEM. 
>
>
>
>      -----BEGIN RSA PRIVATE KEY-----
>      (Private Key, omitted)
>      -----END RSA PRIVATE KEY-----
>
>, or
>
>      -----BEGIN PRIVATE KEY-----
>      (Private Key, omitted)
>      -----END PRIVATE KEY-----

##### Health Check
Setting for status check is also determined when listener is created. TOAST Load Balancer can define status check operations per listener. Following items are required: 

* Health Check Protocol: Determine the protocol to check status. Choose one of TCP, HTTP, or HTTPS. 
* Health Check Port: Determine the port for member instance to try status check.
* HTTP Method: Select the HTTP method to check status: enabled only when HTTP or HTTPS is selected. Currently supports GET only. 
* HTTP Status Code: Enter the HTTP status code to be considered normal for a status check: enabled only when HTTP or HTTPS is selected. Currently supports GET only.  
* URL: Specify the route of member instance to try status check: enabled only when HTTP or HTTPS is selected. 
* Health Check Cycle: Enter cycle of status check. The unit is the second and status check is tried at every specified cycle. 
* Maximum Wait Time for Responce: Specify the maximum time to wait for normal response after status check. The unit is the second and exceeding a specified latency is considered a failure. 
* Maximum Number of Retries: Specify the maximum number of retrials of status check. If the number is more than 2, not getting normal response on status check is not readily considered a failure. In case of repeated failure as much as it is retried, the instance shall be excluded from load dispersion.  

##### Connect
* Session Persistence: Responses to requests are set to be made at specific instances only, so as to maintain session. Choose one of No Session Persistence, APP_COOKIE, HTTP_COOKIE, or SOURCE_IP. 
* Association Limited: Specify the number of TCP connections to which default listener is to be maintained at the same time: if left blank, the default value of 2000 shall be set. 
* Keepalive Timeout: Session persistence between client and server is specified by the second. Load balancer maintains session during this time as long as the other side maintains the session. It is recommended that the keepalive timeout set in the server apply the same. Default is 300 seconds. 
* Proxy Protocol: Load balancer can be made to support proxy protocol. This value must be enabled only when proxy protocol is made available at the server to know client IP: can be applied only when TCP and HTTPS protocols are used.  

#### Register Members 
Specify the instance to be registered as member when load balancer is created. It is fine to register members after load balancer is created: only those instances that belong to VPC to which load balancer is associated can be registered as members.  

#### IP Access Control Groups 
Specify the IP access control group to apply when load balancer is created. You can select many groups in the same type of access control. After load balancer is created, you may change IP access control group to apply. 

### View Load Balancers
After load balancer is created, the list of load balancers is returned. On this page, you can find basic information of created load balancers, and following items are displayed on the page:   

* Name: Name of the load balancer specified when it is created. 
* Type: Load balancer type
* IP Address: Refers to a private IP assigned by VPC associated to load balancer. The IP provides an access to load balancer from within VPC. From outside of VPC, floating IP needs to be associated to access load balancer.  
* Load Balancer Port/Instance Port: A pair of listeners' port and instance port that belong to a load balancer.   
* Protocol: Protocol of listeners that belong to a load balancer. 
* IP Access Control Type: Shows the type of IP access control group which is specified for a load balancer. If not specified, nothing is shown; if specified, the type is either ‘Allow’ or ‘Deny’. 
* Network: Name of VPC and subnet CIDR associated to load balancer.
* Status: Status of load balancer creation. ACTIVE means it has been normally created. 

> [Note] Provisioning status of load balancer is to be determined as one of the following:

> | Status | Description |
> |--|--|
> | ACTIVE | Completed with Load Balancer Creation; Under Normal Operations |
> | PENDING_CREATE | Load balancer is being created |
> | PENDING_UPDATE | Modifying Load Balancer Configuration <br> Unless the status is changed to ACTIVE within an hour after configuration is modified, contact administrator. |
> | PENDING_DELETE | Deleting Load Balancer<br> Unless it is removed from the list within an hour after deleted, contact administrator. |
> | ERROR | Failed to Create Load Balancer<br> Contact administrator. |

### Modify Load Balancer and its Details 
Select a load balancer from the list, and a page of details shows, composed of the three tabs as follows: 

* Details of Load Balancer: Shows detailed information of a load balancer. Name and description of a selected listener can be changed. 
* Listener: Check detailed setting of listeners created under a selected load balancer. Add or delete listeners. 
* Instance: View the list of instances registered as members to a selected load balancer. Register new instances as members or exclude existing ones.  
* Statistics: Statistical information of a selected load balancer is available. 

> [Note] Cannot change VPC and IP address to which load balancer is associated. 

#### Add Listeners 
To add a listener, select Listener on the detail page of load balancer, and click Add. Items required to add listeners are identical to those required for default listener when a load balancer is created. The load balancer port which existing listeners used cannot be applied to add listeners.  

#### Modify Listeners 
To modify the setting of a listener, click Modify. 

> [Note] Cannot change the listener protocol, load balancer port, and instance port. 

#### Delete Listeners 
To delete a listener, click Delete: cannot delete, though, if the load balancer has only one listener. 

> [Caution] Add/Modify/Delete Listeners causes reboot of a load balancer: its abrupt suspension may cause unexpected malfunctioning. Change of listeners, therefore, is recommended when its influence on services is limited.  

#### Add Members
Register a new instance as member of load balancer in the instance tab. Only those instances that belong to VPC to which load balancer is associated can be added.

#### Disable Members 
Member instances may be disabled to be excluded from service: select `True` from instance item to exclude, and it is changed to `False` and disabled.  

#### Delete Members 
Instances that are no longer used may be deleted. Click Detach Instance of the instance to exclude, and it is deleted from the member of load balancer. Deletion from load balancer member does not mean its instance is also deleted.    

> [Caution] Add/Disable/Delete Members causes reboot of a load balancer: its abrupt suspension may cause unexpected malfunctioning. Change of members, therefore, is recommended when its influence on services is limited.  

### Delete Load Balancers
To delete a load balancer, select a load balancer from the list and press Delete.

## IP Access Control Groups
For more details on the features of IP access control, see [IP Access Control](/Network/Load%20Balancer/ko/overview/#_9).

#### Create IP Access Control Groups
To create an IP access control group, click Create Access Control Group and enter the following values: 

* Name: Enter name of an access control group. 
* Description: Describe the access control group. 
* IP Access Control Type: Select either Deny or Allow. 
* Add Target of IP Access Control: Describe the IP control target. Click "+" on the right to add many targets of access control > at once. To make it easy, you may select “Mass Input”. In this case, enter “IP address or CIDR” and “Description” in one line. Up to 100 targets of access control can be entered at a time. 


Press Check, and the groups and targets of access control are created. 

> [Reference] Number of groups and targets of IP access control
>
> Up to 10 access control groups can be created for each project. 
> Up to 1,000 access control targets can be created for each project. 


#### Change IP Access Control Groups
Attributes of IP access control group can be changed. Name and description are available to change. The “IP access control type” attribute cannot be changed. 

#### Delete IP Access Control Groups
You can delete selected IP access control groups. When a group is deleted, its access control targets are deleted altogether. 
With an IP access control group deleted, its load balancer is not allowed to use the policy any more.

#### Add IP Access Control Targets
Select an access control group, and the menu shows its target of access control. 
If a target is added to the access control group, policy on the added IP or CIDR for all load balancers that use this group shall be applied. 

#### Change IP Access Control Targets 
You may change attributes for access control targets: only description can be changed.

#### Delete IP Access Control Targets
Select an access control group, and the menu shows its target of access control. 
If a target of a group is deleted, policy on the IP or CIDR for all load balancers that use this group shall be deleted. 

#### Apply IP Access Control Groups
Select a load balancer to apply IP access control group. Select a group for the load balancer and press Confirm.
Many groups in the same "access control type" can be applied to load balancer.  

