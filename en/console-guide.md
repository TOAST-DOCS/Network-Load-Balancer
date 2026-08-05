<!-- pre-align:aligned sig=7b3576dbe008 -->

<a id='network-load-balancer-console-guide'></a>
## Network > Load Balancer > Console Guide { #network-load-balancer-console-guide }

<a id='manage-loadbalancers'></a>
## Manage Load Balancers { #manage-loadbalancers }

<a id='create-loadbalancers'></a>
### Create Load Balancers { #create-loadbalancers }
You can easily create a load balancer by entering the setting values in the NHN Cloud Load Balancer console. Depending on your purpose, you can select either L4 routing or L7 routing mode to create it. <br>
The mode refers to the template, not the actual type of load balancer. You can create a load balancer with L4 routing mode and add L7 rules.

* L4 Routing: A load balancer that performs load balancing based on IP and port. You can change it to a Layer7 load balancer by adding L7 rules after creation.
* L7 Routing: A load balancer that performs load balancing based on L7 data.

<a id='create-loadbalancers-set-up-load-balancers'></a>
#### Set up Load balancers
Set up basic information about the load balancer. The following items are required

* Name: Enter the name of the load balancer.
* Description: Enter the description of the load balancer.
* Type: You can choose General or Dedicated.
* Network (Subnet): Specify the subnet of the VPC with which the load balancer is to be associated.
* Subnet static routes: Select whether to apply the static route settings of the subnet where the load balancer will be located to the load balancer. If you select **Auto Assign**, the load balancer is assigned a private IP that is available within the subnet range. You can select **Specify** to give the load balancer a private IP of your choice. 


> [Note] For more information about load balancer types, See [Load Balancer Types](https://www.toast.com/service/network/load-balancer).

<a id='create-loadbalancers-set-up-listeners'></a>
#### Set up Listeners

Defines the properties of the traffic that the load balancer will process. A load balancer in NHN Cloud can have one or more listeners.

* Name: Enter a name for the listener.
* Description: Describe the listener.
* Protocol: Specifies the protocol of the traffic that the load balancer will handle. Select one of the following: TCP/HTTP/HTTPS/TERMINATED_HTTPS.
* Load balancer port: Specifies the port on which the default listener will listen for traffic.
* Default member group: Specifies the member group that will be distributed by default when traffic is received. You can specify the default member group for the listener as **Not use**. If there is no L7 rule, or even if there is, when you set to **Not use** because the rule does not meet the conditions, the request will return a 503.
* Connection limit: Specifies the number of TCP sessions that the default listener will maintain simultaneously. You can set a maximum of 60,000 for a general load balancer and a maximum of 480,000 for a dedicated load balancer.
* Keep-Alive timeout: Specifies the amount of time, in seconds, to keep a session alive with the client and server. The load balancer will keep the session alive for this amount of time as long as the other side keeps the session alive. We recommend that you set the Keep-Alive timeout value you set on your server here. The default value is set to 300 seconds.
* Proxy protocols: Allows you to enable the load balancer to support proxy protocols. You should enable this value only if you have enabled proxy protocols for the server to know the client's IP. This is only available if you are using the TCP and HTTPS protocols.
* Block invalid requests: When **Not use** is selected, blocks HTTP request headers if they contain invalid characters. Available only when using HTTP and the TERMINATED_HTTPS protocol.

* SSL Certificate: Register a certificate to be used when TERMINATED_HTTPS is selected as the protocol.
* SSL policy: If TERMINATED_HTTPS is selected as the protocol, you can select a custom SSL policy to connect to the listener.
  * If **Disabled** is selected, the default cipher suites provided based on the listener's TLS version setting are applied.
  * If an SSL policy is selected, the cipher suites defined in that policy are applied, and the listener's TLS version must be set to match the minimum TLS version of that policy.
  * If no SSL policy is available, you must first create one in the **SSL policy management** menu.

!!! danger "Caution"
    - Load balancer port, instance port, and protocol cannot be changed after a listener is created.
    - You can set the listener's default member group to **Disabled**. If there are no L7 rules, or if existing L7 rules do not match the conditions and load balancing defaults to **Disabled**, the request will return a 503.


!!! tip "Note"
    - The load balancer port accepts values between 1 and 65535.
    - Health checks are performed only when a member group is assigned as a listener's default member group or designated as the action target of an L7 rule. Otherwise, health checks are not performed for that member group.

!!! tip "Note"
    How to register TERMINATED_HTTPS certificates

    When the listener protocol of a load balancer is set to TERMINATED_HTTPS, the button to register an SSL certificate is enabled.

    The files to register are the "Certificate" and the "Private Key." The "Private Key" refers to the private key paired with the public key embedded in the server certificate.

    The "Certificate" follows the x.509 PEM format as shown below:

        -----BEGIN CERTIFICATE-----
        (Content omitted)
        -----END CERTIFICATE-----

    When registering a server certificate along with a chain certificate (Chain Certificate, Intermediate Certificate), you must combine the server certificate and chain certificate into a single file for registration.

    When creating a single certificate file, the server certificate must be placed at the top of the file, followed by the chain certificates. Chain certificates can be listed in any order.

    When combining one server certificate and two chain certificates into a single certificate file, the format is as follows:

        -----BEGIN CERTIFICATE-----
        (Server certificate content omitted)
        -----END CERTIFICATE-----
        -----BEGIN CERTIFICATE-----
        (Chain certificate #1 content omitted)
        -----END CERTIFICATE-----
        -----BEGIN CERTIFICATE-----
        (Chain certificate #2 content omitted)
        -----END CERTIFICATE-----

    The "Private Key" is the key file corresponding to the public key included in the server certificate. The registered "Private Key" must have its password removed to function properly.

    Files in PKCS#1 or PKCS#8 PEM format can be registered.

        -----BEGIN RSA PRIVATE KEY-----
        (Private key content omitted)
        -----END RSA PRIVATE KEY-----

    or

        -----BEGIN PRIVATE KEY-----
        (Private key content omitted)
        -----END PRIVATE KEY-----


##### Using Certificate Manager
When the listener uses TERMINATED_HTTPS, you can register a certificate in one of the following two methods: using a certificate registered in Certificate Manager or directly registering a certificate.

* By registering a certificate in Certificate Manager and connecting it with the listener, you can receive an email alarm on certificate expiration date.
* No expiration alarms will be sent if the certificate has been directly registered in the listener. Still, you can find the expiration date on the listener page of the console.
!!! danger "Caution"
    When a certificate is updated in the Certificate Manager, certificates of any other affected listener must be updated as well.
    To apply the certificate which is registered in the Certificate Manager to the listener, the password of the 'Private Key' must be removed, and the format must be PKCS#1 or PKCS#8 PEM.

##### Set up L7 Rules
The load balancer can perform load balancing based on L7 data. When you select an L7 routing template to create a load balancer, you can create a load balancer that includes L7 policies. L7 policies work well only when the protocol of the listener is HTTP/TERMINATED_HTTPS. Even if you create a load balancer with an L4 template, you can add L7 rules later.

* Name: Enter a name for the L7 rule.
* Description: Describe the L7 rule.
* Action type: Specify the action to take when matching L7 rules. 
  * Forward to member group: Send to a set member group when matched to an L7 rule. You can route packets to specific member groups based on L7 data.
  * Forward to URL: This feature redirects to a set URL when an L7 rule is matched. It uses the Location in the HTTP header to perform the redirect.
  * Block: Block if matched by an L7 rule. Returns a response as Forbidden (403).
      * For **Forward to URL**, you can set the Redirect URL in fine detail. For each of the protocol, port, host, route, and query, you can keep the values or change them directly to redirect. See [Notes] below for more information.
    * Status code: The HTTP response code that the load balancer will respond with when redirecting. 301 and 302 are supported.
* Task target: Set a target based on the task type. The input varies depending on the task type.
* Task priority: Set the L7 rule priority. The value you enter determines the priority within the task type, and if you enter a duplicate value, the new rule takes precedence.
  * The order of rule application is **Block**, **Pass by URLL**, and  **Pass to Member Group**. Within the same action type, apply the priority entered by the user.
  * The **Priority Order** column in the L7 Rules table makes it easy to understand the actual order in which rules are applied.
  * Whenever you add or change an L7 rule, the task priorities you enter are reordered internally to re-prioritize them.
  * The task priorities you see in **View Details** represent the relative values of the internally reordered priorities, not the absolute values you entered.
  * If you want to add or change L7 rules in the future, you'll need to set priorities based on the relative values of the internally reordered priorities.
* Condition: Describe the conditions to apply to the L7 rule. You can create up to 10 conditions per L7 rule.
  * Condition type: Condition types support paths, headers, file types, cookies, and hostnames.
    * Path: Examines the value of the URL path.
    * Header: Examines the fields contained in the HTTP header. You must provide additional header field names.
    * File type: Examines the end value of the URL path. This can be useful for matching extensions.
    * Cookie: Examines the Cookie field in the HTTP request header. You must additionally enter the key of the cookie.
    * Hostname: Examines the Host field in the HTTP request header.
  * Comparison method: The comparison method can be selected from CONTAINS/EQUAL_TO/STARTS_WITH/ENDS_WITH/REGEX, depending on the condition type.
    * CONTAINS: True if the string of the condition type contains the value you entered.
    * EQUAL_TO: True if the string of the condition type matches the value you entered.
    * STARTS_WITH: True if the string of the condition type starts with the value you entered.
    * ENDS_WITH: True if the string of the condition type ends with the value you entered.
    * REGEX: True if the string of the condition type conforms to the syntax of the regular expression you entered.
  * Value: Enter the string you want to match. If the condition type is header or cookie, you must additionally enter key.

!!! danger "Caution"
    - Among the condition types, host name matching is case-insensitive.
    - If a redirect URL is entered in an incorrect format, the redirect URL may be converted to a value different from the actual input.

!!! tip "Note"
    - If traffic does not match any configured L7 rule, it is forwarded to the listener's default member group.
    - Health checks are performed only when a member group is assigned as a listener's default member group or designated as the action target of an L7 rule. Otherwise, health checks are not performed for that member group.
    - If a member group is deleted, L7 rules that had that member group as their action target will have their action type changed to Block.
    - When **Forward to URL** is configured in an L7 rule, you can retain or manually specify individual components of the redirect URL. To retain the value of a specific field, enter it in the corresponding URI component field in the format `#{protocol}`, `#{port}`, `#{host}`, `#{path}`, or `#{query}`. For example, to change only the protocol and port to HTTPS and port 443 for incoming HTTP requests, enter `HTTPS` for the protocol, `443` for the port, `#{host}` for the host, `#{path}` for the path, and `#{query}` for the query.


<a id='create-loadbalancers-set-up-member-groups'></a>
#### Set up Member Groups
Set the target member groups to forward load balancing traffic to. You can create additional member groups even after the load balancer creation is complete.

* Name: Enter a name for the member group.
* Description: Describe the member group.
* Protocol: Specify the protocol of the traffic that the member group will handle. Select one of the following: HTTP/HTTPS/TCP/HTTP_REENCRYPT.
* Member port: Specify the ports on which the member group listens for traffic.
* Load Balancing Method: Determines how the load balancer distributes traffic. Select one of the following. ROUND_ROBIN/LEAST_CONNECTIONS/SOURCE_IP.
* Session Persistence: A setting that forces responses to requests to be made only on a specific instance to preserve the session. You can select one of the following: No session persistence/APP_COOKIE/HTTP_COOKIE/SOURCE_IP.


!!! danger "Caution"
    Member ports and protocols cannot be changed after a member group is created.

!!! tip "Note"
    Member ports have values between 1 and 65535.


##### Health Check

The settings for health check are also determined when creating the listener. NHN Cloud's load balancer can define health check behavior per listener. The items required are as follows:

* Health Check Protocol: Determine the protocol to use for health checks. Choose one of TCP, HTTP, or HTTPS.
* Health Check Port: Determine the port of member instance to try health checks.
* Health Check Port: Set the member's port to attempt health checks on. Select a member port to perform health checks on the port numbers specified for each member. If you select Custom, health checks are performed on a custom port number in bulk, independent of the port number for each member.
* HTTP Method: Select the HTTP method to use for health checks. This setting is enabled only when HTTP or HTTPS is selected. Currently supports GET only.
* HTTP Status Code: Enter the HTTP status code to consider as normal for a health check. This setting is enabled only when HTTP or HTTPS is selected. Currently supports GET only.
* URL: Specify the path of the member instance to try health checks. This setting is enabled only when HTTP or HTTPS is selected.
* Health Check Cycle: Enter the cycle of health checks. The unit is seconds and health checks are tried at every specified cycle.
* Maximum Wait Time for Response: Specify the maximum time to wait for a normal response after health checks. The unit is seconds and exceeding the specified wait time is considered a failure.
* Maximum Number of Retries: Specify the maximum number of retry attempts for health checks. If the maximum number of retries is 2 or higher, it is not immediately considered a failure when a normal response to the health check is not received. If it fails repeatedly for the maximum number of retries, the instance is excluded from load balancing.
* Host Header: Enter the field value to use in the host header for health checks. This setting is enabled only when HTTP or HTTPS is selected.

!!! danger "Caution"
    If you have multiple members in a member group with different port numbers, be careful about setting the health check port. For example, if you have two members, such as port 80 on 192.168.0.10 and port 8080 on 192.168.0.10, selecting Health check port as member port will perform health checks on port 80 and port 8080 respectively. If you select Custom as the health check port and type 80, it will check port 80 even if the member port is port 8080. If the 80 port on 192.168.0.10 is active, then the member on the 8080 port for 192.168.0.10 is also considered ACTIVE because it is checking the status of the 80 port for 192.168.0.10.

!!! tip "Note"
    Health checks are performed only when a member group is assigned as a listener's default member group or designated as the action target of an L7 rule. Otherwise, health checks are not performed for that member group.


##### Set up Members
Specify instances or IPs to register as members when the load balancer is created. You can register members even after the load balancer is created. Members can be registered in two ways

* Instance: You can add instances that belong to the VPC to which the load balancer is attached and to VPCs that are peered with that VPC as members. However, if you want to add an instance with a different subnet than the load balancer as a member, you must register both subnets in the routing table.
* IP address: You can register members by entering an IP directly. In this case, the communication path between the load balancer and that IP must be set up appropriately.

<a id='create-loadbalancers-delete-proteection'></a>
#### Delete Proteection
Enabling delete protection protects a load balancer from accidental deletion. You cannot delete that load balancer until you disable delete protection. A load balancer with delete protection enabled cannot delete listeners, member groups, and L7 rules, and also cannot delete and change health monitors.

<a id='create-loadbalancers-ip-access-control-groups'></a>
#### IP Access Control Groups
Specify the IP access control group to apply when the load balancer is created. You can select multiple groups with the same access control type among the IP access control groups. You can change the IP access control group to be applied even after the load balancer is created.

<a id='view-loadbalancers'></a>
### View Load Balancers { #view-loadbalancers }
After a load balancer is created, you will be returned to the load balancer list page. In the load balancer list page, you can check the basic information of the created load balancers. The items displayed on the list page are as follows:

* Name: Name of the load balancer specified when it is created.
* Type: Load balancer type
* IP Address: A private IP assigned by the VPC associated with the load balancer. From inside the VPC, you can access the load balancer through this IP. To access the load balancer from outside the VPC, you need to associate a floating IP.
* Network: Name of the VPC associated with the load balancer and the subnet CIDR.
* Status: Status of load balancer creation. ACTIVE means it has been created normally.
* Load balancer details: View details of the listeners and member groups connected to the load balancer.

!!! tip "Note"
    Provisioning status of a load balancer is determined as one of the following:

    | Status | Description |
    |--|--|
    | ACTIVE | A load balancer has been created and is operating normally |
    | PENDING_CREATE | Creating a load balancer <br> If the status does not change to ACTIVE within an hour after creation, contact the administrator. |
    | PENDING_UPDATE | Modifying load balancer configuration <br> If the status does not change to ACTIVE within an hour after modifying the configuration, contact the administrator. |
    | PENDING_DELETE | Deleting a load balancer<br> If the load balancer does not disappear from the list within an hour after deletion, contact the administrator. |
    | ERROR | Failed to create a load balancer<br> Contact the administrator. |
    | ERROR_MIGRATE | Failed to migrate a load balancer<br> Contact the administrator. |

<a id='modify-loadbalancers'></a>
### Modify Load Balancers and Details { #modify-loadbalancers }
Select a load balancer from the list, and a page of details shows up at the bottom, which is composed of the following tabs:

* Details of Load Balancer: Shows detailed information of a load balancer. Name and description of the selected load balancer, and whether to apply subnet static routing can be changed.
* Listener: Check detailed setting of listeners created under a selected load balancer. Add or delete listeners.
* Instance: View the list of instances registered as members to a selected load balancer. Register new instances as members or exclude existing ones.
* Statistics: Statistical information of a selected load balancer is available.

!!! tip "Note"
    The VPC and IP address connected to the load balancer cannot be changed.
<a id='change-listener'></a>
### Listener Changes and Details { #change-listener }
On the main screen of the load balancer, select the desired load balancer detail view to see the listeners and member groups connected to the load balancer. From there, you can select the **Listeners** tab to create, change, or delete listeners.

<a id='change-listener-add-listeners'></a>
#### Add Listeners
Listeners can be added by clicking the Add Listener button on the Listener tab in the detail screen of the load balancer. Items required to add listeners are the same as those required by the default listener during creation of the load balancer. When a listener is added, the load balancer port used by previous listeners can no longer be used.

<a id='change-listener-modify-listeners'></a>
#### Modify Listeners
To modify the setting of a listener, click Modify.

!!! danger "Caution"
    You cannot change the listener protocol, load balancer port, and instance port.

<a id='change-listener-manage-certificate'></a>
#### Manage Certificate
For listeners using the TERMINATED_HTTPS protocol, you can manage multiple certificates in the **Certificates** tab of the listener details screen.

##### View Certificates
1. Click the **Listeners** tab in the load balancer details screen.
2. Select the TERMINATED_HTTPS listener for which you want to manage certificates.
3. Click the **Certificates** tab in the listener details screen.
4. You can view the following information in the list of registered certificates:
- **Name**: certificate name
- **Expiration Date**: certificate expiration date and number of days remaining until expiration

##### Add Certificates
1. Click **+ Add Certificate** button in the **Certificates** tab of the listener details screen.
2. Select whether to use the Certificate Manager.
- **Enable**: select from the list of certificates registered in the Certificate Manager.
- **Disable**: register by directly uploading the certificate and private key files. 3. After reviewing the warning message, select the checkbox and click **OK**.

!!! danger "Caution"
    Adding a certificate will restart the load balancer. Existing sessions will be maintained during the restart process, but new sessions will not be processed (about less than 1 second). Therefore, we recommend changing during a time that will not impact the service.

##### Change the Default Certificate
1. In the **Certificate** tab of the listener details screen, click the **Change Default Certificate** button.
2. Select the certificate to use as the default certificate.
3. After reviewing the warning message, select the checkbox and click **OK**.

!!! danger "Caution"
    Changing the default certificate will restart the load balancer. During the restart process, existing sessions will be maintained, but new sessions will not be processed (about less than 1 second). Therefore, we recommend changing during a time that will not impact the service.

##### Delete a Certificate
1. In the **Certificate** tab of the listener details screen, select the certificate to be deleted.
2. Click **Delete Certificate** button. 3. After reviewing the warning message, select the checkbox and click **OK**.

!!! danger "Caution"
    - The default certificate cannot be deleted. To delete the default certificate, you must first change another certificate to the default and then delete it.
    - Deleting a certificate will restart the load balancer. During the restart process, existing sessions will be maintained, but new sessions cannot be processed (about 1 second). Therefore, we recommend performing the process during a time that will not impact the service.

<a id='custom-response-guide'></a>
### Custom Response Guide { #custom-response-guide }

You can configure custom responses in the load balancer listener. Using custom responses, you can directly deliver custom messages or HTML content to users when a specific HTTP error code occurs.

<a id='custom-response-guide-view-and-configure-custom-responses'></a>
#### View and Configure Custom Responses

1. Click the **Listeners** tab on the load balancer details screen.
2. Select the listener for which you want to configure a custom response.
3. On the listener details screen, click **View/Change Custom Response Settings**.
4. You can enter and confirm the following items:
   - **Response Code**: select the HTTP status code to which the custom response will be applied. (Supported codes: 400, 403, 408, 500, 502, 503, 504)
   - **Content Type**: select the Content-Type of the response to be delivered to the user. (Choose from `text/html`, `text/plain`, `application/json`, `application/javascript`, and `text/css`)
   - **Response Body**: enter the body of the response to be displayed to the user. (up to 1,024 characters. The content can be HTML, text, or any other format, depending on the content type.)
5. After entering each item, click **Confirm** to generate the response. The generated response can be viewed in the list.

<a id='custom-response-guide-delete-a-custom-response'></a>
#### Delete a Custom Response

- You can delete a created custom response by selecting it from the list and clicking **Delete** button.
- If an error code corresponding to the deleted response occurs, the default system response will be displayed.

!!! tip "Note"
    Each error code can only be registered as a custom response once within the same listener.

!!! danger "Caution"
    When adding, modifying, or deleting a custom response, the listener may briefly restart (less than 1 second). Therefore, we recommend changing during a time when service impact is minimal.

<a id='x-forwarded-header-guide'></a>
### Guide to X-Forwarded Header Configuration { #x-forwarded-header-guide }

You can view and change the X-Forwarded header settings on a load balancer listener. The X-Forwarded header is used to forward the client's source information (protocol, port, IP address) to the backend server.

<a id='x-forwarded-header-guide-view-and-configure-the-x-forwarded-header'></a>
#### View and Configure the X-Forwarded Header

1. Click the **Listener** tab on the load balancer details screen.
2. Select the listener for which you want to configure the X-Forwarded header.
3. On the **Basic Information** tab on the listener details screen, click **View/Change Settings** button under the **X-Forwarded Header** section.
4. You can configure the following:
   - **X-Forwarded-Proto**: set whether to forward the protocol (http or https) used by the client to the backend server. Select either **Enable** or **Disable**.
   - **X-Forwarded-Port**: set whether to forward the port number the client connected to to the backend server. Select either **Enable** or **Disable**.
   - **X-Forwarded-for**: set whether to forward the client's original IP address to the backend server. Select either **Enable** or **Disable**.
5. After configuring each item, check the box in the warning message and click **OK** to apply the settings.

!!! danger "Caution"
    Changing the X-Forwarded header settings will cause the load balancer to restart. Existing sessions will be maintained during the restart process, but new sessions cannot be processed (about less than 1 second). Therefore, we recommend changing during a time when the service will not be affected.

<a id='x-forwarded-header-guide-delete-listeners'></a>
#### Delete Listeners
To delete a listener, click Delete: cannot delete, though, if the load balancer has only one listener.

!!! danger "Caution"
    Add/Modify/Delete listeners causes reboot of a load balancer. During the reboot, existing connected sessions are maintained, but new sessions cannot be processed (less than 1 second). Therefore, it is recommended to proceed at a time that does not affect the service.

<a id='change-member-group'></a>
### Member Group Changes and Details { #change-member-group }
On the Load Balancers screen, select the desired load balancer's **View Details** to see the listeners and member groups connected to the load balancer. From there, you can select the **Member Groups** tab to create, change, or delete member groups.

<a id='change-member-group-create-member-groups'></a>
#### Create Member Groups
Click **Create Member Group** to create additional member groups. The items required to create a member group are the same as those required for a member group when creating a load balancer.

<a id='change-member-group-change-member-groups'></a>
#### Change Member Groups
Click **Change Member Group** to change settings related to the member group.

!!! danger "Caution"
    Member ports and protocols cannot be changed after a member group is created.

<a id='change-member-group-delete-member-groups'></a>
#### Delete Member Groups
Select the member group you want to delete and click **Delete Member Group** to delete that member group.

!!! danger "Caution"
    Creating/editing/deleting a member group restarts the load balancer. During the restart, existing connected sessions are preserved, but new sessions cannot be processed (for less than a second). Therefore, we recommend doing this at a time that does not impact service.

!!! tip "Note"
    When a member group is deleted, any L7 rules that had that member group as an action target will have their action type changed to Block.

<a id='change-member'></a>
### Member changes and details { #change-member }
On the Load Balancer **View Details** screen, select the **Member Group** tab, and then select the desired member group to view the details of the member group and the status of the members in the member group.

<a id='change-member-add-a-member'></a>
#### Add a member
After you select a member group, you'll see the **Basic Info**, **Members**, and **Check Status** tabs at the bottom of the screen. Select the **Members** tab to enroll the desired instances or IP addresses as members. You can only add instances that belong to the VPC to which the load balancer is attached and to VPCs that are peered to that VPC. You can specify your own destination port number for each member, and load balancing will be done with that destination port number.

!!! danger "Caution"
    If you have multiple members in a member group with different port numbers, be careful about setting the health check port. For example, if you have two members, such as port 80 on 192.168.0.10 and port 8080 on 192.168.0.10, selecting Health check port as member port will perform health checks on port 80 and port 8080 respectively. If you select Custom as the health check port and type 80, it will check port 80 even if the member port is port 8080. If the 80 port on 192.168.0.10 is active, then the member on the 8080 port for 192.168.0.10 is also considered ACTIVE because it is checking the status of the 80 port for 192.168.0.10.

<a id='change-member-deactivate-a-member'></a>
#### Deactivate a member
You can temporarily exclude specific members from the service. Select the members you want to exclude, click the **Deactivate members** button, and then click **OK**.
The excluded members' permissions will change to **X** and their member status will change to **ONLINE**.

!!! tip "Note"
    The status of a member is determined by one of the following

    | Status | Meaning |
    |--|--|
    | ACTIVE | Member connection complete, working fine |
    | INACTIVE | A member's health check is not being performed |
    | ONLINE | Member is disabled|
    | OFFLINE | Member connection failure <br> Contact your administrator.|

<a id='change-member-delete-members'></a>
#### Delete Members
Instances that are no longer used may be deleted. Click Detach Instance of the instance to exclude, and it is deleted from the member of load balancer. Deletion from load balancer member does not mean its instance is also deleted.

!!! danger "Caution"
    Add/Disable/Delete Members causes reboot of a load balancer. During the reboot, existing connected sessions are maintained, but new sessions cannot be processed (less than 1 second). Therefore, it is recommended to proceed at a time that does not affect the service.

<a id='delete-loadbalancers'></a>
### Delete Load Balancers { #delete-loadbalancers }
Select the load balancer you want to delete from the load balancer list screen and click **Delete** button to delete the load balancer.


<a id='ip-acl-groups'></a>
## IP Access Control Groups { #ip-acl-groups }
For more details on the features of IP access control, see [IP Access Control](/Network/Load%20Balancer/en/overview/#load-balancer-ip-access-control).

<a id='create-ip-acl-groups'></a>
#### Create IP Access Control Groups
To create an IP access control group, click [Create Access Control Group] and enter the following values:

* Name: Enter the name of the access control group.
* Description: Enter the description of the access control group.
* IP Access Control Type: Select either Block or Allow.
* Add IP Access Control Target: Enter the access control target IP and its description. You can add multiple access control targets at once by clicking the **+** button on the right. To make bulk input easier, you can select **Enter in Mass**. In this case, enter "IP address or CIDR" and "description" on one line. You can enter up to 100 access control targets at one time.


Click **Confirm** and the groups and targets of access control are created.

!!! tip "Note"
    Number of groups and targets of IP access control

    Up to 10 access control groups can be created for each project.
    Up to 1,000 access control targets can be created for each project.

<a id='change-ip-acl-groups'></a>
#### Change IP Access Control Groups
You can change the properties of an IP access control group. The properties you can change are name and description. The "IP Access Control Type" property cannot be changed.

<a id='delete-ip-acl-groups'></a>
#### Delete IP Access Control Groups
You can delete the selected IP access control groups. When you delete a group, all access control targets belonging to the group are also deleted.
When you delete an IP access control group, load balancers using the group will no longer use that policy.

<a id='add-ip-acl-targets'></a>
#### Add IP Access Control Targets
If you select an access control group, the access control target menu appears at the bottom.
When a target is added to an access control group, the policy of the added IP or CIDR is reflected in all load balancers using this access control group.

<a id='change-ip-acl-targets'></a>
#### Change IP Access Control Targets
You can change the properties of the access control target. You can only change the description.

<a id='delete-ip-acl-targets'></a>
#### Delete IP Access Control Targets
If you select an access control group, the access control target menu appears at the bottom.
If you delete a target belonging to an access control group, the policy of the corresponding IP or CIDR is deleted from all load balancers using this access control group.

<a id='apply-ip-acl-groups'></a>
#### Apply IP Access Control Groups
Select the load balancer to apply the IP access control group to. Select the group you want to configure for that load balancer and click **Confirm**.
Multiple groups with the same "access control type" can be applied to the load balancer.

<a id='ssl-policies'></a>
## SSL Policy Management { #ssl-policies }
An SSL policy is a custom security policy that defines the minimum TLS version and cipher suite combination to use for a listener. For the concept of SSL policies and the list of available cipher suites, see [Custom SSL policy](/Network/Load%20Balancer/en/overview/#ssl).

<a id='create-ssl-policies'></a>
#### Create SSL Policy
To create an SSL policy, click the **Create SSL policy** button and enter the following values:

* Name: Enter the name of the SSL policy.
* Minimum SSL/TLS version: Select the minimum SSL/TLS version allowed by the SSL policy. Select one of `SSLv3`, `TLSv1.0`, `TLSv1.0_2016`, `TLSv1.1`, `TLSv1.2`, or `TLSv1.3`.
* Cipher suite list: The cipher suites available for the selected minimum SSL/TLS version are displayed. Select the suites to use.
* Description: Enter a description of the SSL policy.

!!! danger "Caution"
    - The minimum SSL/TLS version cannot be changed after creation. If a change is needed, you must create a new SSL policy and apply it to the listener.
    - At least one cipher suite must be selected.

!!! tip "Note"
    Up to 10 SSL policies can be created per project.

<a id='change-ssl-policies'></a>
#### Modify SSL Policy
You can modify the name, description, and cipher suites of an SSL policy. The minimum SSL/TLS version cannot be changed.

When the cipher suites of an SSL policy are modified, the settings of all listeners to which that policy is applied are automatically updated.

!!! danger "Caution"
    When an SSL policy is modified, the load balancer of the listener connected to that policy is restarted. During the restart, existing sessions are maintained, but new sessions may not be processed for up to 1 second. It is recommended to perform this operation during a time that does not affect the service. The change will only proceed after selecting the confirmation checkbox on the modification screen.

<a id='delete-ssl-policies'></a>
#### Delete SSL Policy
You can delete an SSL policy. However, an SSL policy connected to one or more listeners cannot be deleted. You must first change the SSL policy of all connected listeners to **Disabled** to disconnect them before deleting the policy.

<a id='apply-ssl-policies'></a>
#### Apply SSL Policy
An SSL policy is connected to a listener on the listener creation screen or the [Change listener and details](#change-listener) screen.

* An SSL policy can only be connected to a listener whose protocol is TERMINATED_HTTPS.
* When connecting an SSL policy to a listener, the listener's TLS version must match the minimum TLS version of the selected policy.
* To disconnect, select **Disabled** for the SSL policy on the listener modification screen.

<a id='restarting-guide-for-maintenance'></a>
## Guide to Restarting Load Balancers for Maintenance { #restarting-guide-for-maintenance }

NHN Cloud updates software of the load balancer equipment on a regular basis to enhance security and stability of the basic infrastructure services. For maintenance of the load balancer, the load balancer running in the maintenance target equipment must be restarted to be migrated to the load balancer equipment where maintenance has been completed.

Load balancers that require a restart have **! Restart** button displayed next to their names. You can use this button to restart the load balancers.

Go to the project with the load balancer specified as the maintenance target and perform a restart with the following procedure.

1. Check the load balancers for maintenance. A load balancer with the **! Restart** button next to its name is the maintenance target.
    ![image-001](http://static.toastoven.net/prod_load_balancer/lb_p_migration_en_1.png)
    Hover the mouse pointer over the  **! Restart** button to find the detailed maintenance schedule. It is recommended to perform maintenance at a time that does not affect service.
    ![image-002](http://static.toastoven.net/prod_load_balancer/lb_p_migration_en_2.png)
2. Select a load balancer for maintenance and click the **! Restart** button next to its name.
3. When the window asking if you want to restart the load balancer appears, click **Confirm**.
    ![image-003](http://static.toastoven.net/prod_load_balancer/lb_p_migration_en_3.png)
4. Wait until the status indicator turns green and the **! Restart** button disappears.
    If the status indicator does not change or the **! Restart** button does not disappear, try 'Refresh'.
    ![image-004](http://static.toastoven.net/prod_load_balancer/lb_p_migration_en_4.png)

The load balancer becomes inoperable while restarting is underway.

If the load balancer restart is not completed normally, it is automatically reported to the administrator, and NHN Cloud will contact you separately.
