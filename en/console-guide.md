## Network > Load Balancer > Console Guide

## Manage Load Balancers
### Create Load Balancers
You can easily create a load balancer by entering the setting values in the NHN Cloud Load Balancer console. Depending on your purpose, you can select either L4 routing or L7 routing mode to create it. <br>
The mode refers to the template, not the actual type of load balancer. You can create a load balancer with L4 routing mode and add L7 rules.

* L4 Routing: A load balancer that performs load balancing based on IP and port. You can change it to a Layer7 load balancer by adding L7 rules after creation.
* L7 Routing: A load balancer that performs load balancing based on L7 data.

#### Set up Load balancers
Set up basic information about the load balancer. The following items are required

* Name: Enter the name of the load balancer.
* Description: Enter the description of the load balancer.
* Type: You can choose General or Dedicated.
* Network (Subnet): Specify the subnet of the VPC with which the load balancer is to be associated.
* Subnet static routes: Select whether to apply the static route settings of the subnet where the load balancer will be located to the load balancer. If you select **Auto Assign**, the load balancer is assigned a private IP that is available within the subnet range. You can select **Specify** to give the load balancer a private IP of your choice. 

> [Note] For more information about load balancer types, See [Load Balancer Types](https://www.toast.com/service/network/load-balancer).

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

> [Caution] Load balancer port, instance port, and protocol cannot be changed after a listener is created.

> [Note] Load balancer port and instance port have a value between 1 and 65535. If there is no L7 rule, or even if there is, when you set to **Not use** because the rule does not meet the conditions, the request will return a 503.

> [Note] Health checks are performed only if the member group is either the default member group for the listener or is specified as the action target of an L7 rule; otherwise, health checks are not performed with that member group.

> [Note] How to register TERMINATED_HTTPS certificates
>
> When TERMINATED_HTTPS is specified as listener protocol for load balancer, a button to register an SSL certificate is activated.
>
> Files to register are ‘Certificate’ and ‘Private Key’. ‘Private Key’ refers to a private key which is paired with a public key embedded in the server certificate.
>
> The ‘Certificate’ must follow the x.509 PEM format as follows:
>
>     -----BEGIN CERTIFICATE-----
>     (omitted)
>     -----END CERTIFICATE-----
>
>
> When you need to register a server certificate and a chain certificate (intermediate certificate) together, you must create and register the server certificate and chain certificate in one file.
>
> To create a single file for certificates, the server certificate must be described at the top of the file and the chain certificates must be described at the bottom of the file. Chain certificates can be described in any order.
>
> If you create one server certificate and two chain certificates into one certificate file, it will have the following format:
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
> ‘Private Key’ a key file corresponding to the public key contained in the server certificate. The registered 'private key' works properly only when the password is removed.
>
> You can register files with the PKCS#1 or PKCS#8 PEM format.
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


##### Using Certificate Manager
When the listener uses TERMINATED_HTTPS, you can register a certificate in one of the following two methods: using a certificate registered in Certificate Manager or directly registering a certificate.

* By registering a certificate in Certificate Manager and connecting it with the listener, you can receive an email alarm on certificate expiration date.
* No expiration alarms will be sent if the certificate has been directly registered in the listener. Still, you can find the expiration date on the listener page of the console.
> [Caution]
> When a certificate is updated in the Certificate Manager, certificates of any other affected listener must be updated as well.
> To apply the certificate which is registered in the Certificate Manager to the listener, the password of the 'Private Key' must be removed, and the format must be PKCS#1 or PKCS#8 PEM.

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

> [Caution] Among condition types, hostnames are not case sensitive.

> [Note] If there is no match to the L7 rules you set up, traffic is forwarded to the listener's default member group.

> [Note] Health checks are performed only if the member group is either the default member group for the listener or is specified as the action target of an L7 rule; otherwise, health checks are not performed with that member group.

> [Note] When a member group is deleted, any L7 rules that had that member group as an action target will have their action type changed to Block.

> [Note] When setting **Forward to URL** in L7 rules, you can keep or customize some of the values in the URL that you want to redirect. If you want to keep the values of specific fields, enter them in each URI partial settings field in the following format: `#{protocol}`, `#{port}, #{host}`, `#{path}`, `#{query}`. For example, if you want to change only the protocol and port number to HTTPS and port 443 for requests that come in over HTTP, enter `HTTPS as`the protocol, port as `443`, host as `#{host}`, path ` as #{path}`, and query ` as #{query}`.


#### Set up Member Groups
Set the target member groups to forward load balancing traffic to. You can create additional member groups even after the load balancer creation is complete.

* Name: Enter a name for the member group.
* Description: Describe the member group.
* Protocol: Specify the protocol of the traffic that the member group will handle. Select one of the following: HTTP/HTTPS/TCP.
* Member port: Specify the ports on which the member group listens for traffic.
* Load Balancing Method: Determines how the load balancer distributes traffic. Select one of the following. ROUND_ROBIN/LEAST_CONNECTIONS/SOURCE_IP.
* Session Persistence: A setting that forces responses to requests to be made only on a specific instance to preserve the session. You can select one of the following: No session persistence/APP_COOKIE/HTTP_COOKIE/SOURCE_IP.


> [Caution] Member ports and protocols cannot be changed after a member group is created.

> [Note] Member ports have values between 1 and 65535.


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

> [Caution] If you have multiple members in a member group with different port numbers, be careful about setting the health check port. For example, if you have two members, such as port 80 on 192.168.0.10 and port 8080 on 192.168.0.10, selecting Health check port as member port will perform health checks on port 80 and port 8080 respectively. If you select Custom as the health check port and type 80, it will check port 80 even if the member port is port 8080. If the 80 port on 192.168.0.10 is active, then the member on the 8080 port for 192.168.0.10 is also considered ACTIVE because it is checking the status of the 80 port for 192.168.0.10.


##### Set up Members
Specify instances or IPs to register as members when the load balancer is created. You can register members even after the load balancer is created. Members can be registered in two ways

* Instance: You can add instances that belong to the VPC to which the load balancer is attached and to VPCs that are peered with that VPC as members. However, if you want to add an instance with a different subnet than the load balancer as a member, you must register both subnets in the routing table.
* IP address: You can register members by entering an IP directly. In this case, the communication path between the load balancer and that IP must be set up appropriately.

#### Delete Proteection
Enabling delete protection protects a load balancer from accidental deletion. You cannot delete that load balancer until you disable delete protection. A load balancer with delete protection enabled cannot delete listeners, member groups, and L7 rules, and also cannot delete and change health monitors.

#### IP Access Control Groups
Specify the IP access control group to apply when the load balancer is created. You can select multiple groups with the same access control type among the IP access control groups. You can change the IP access control group to be applied even after the load balancer is created.

### View Load Balancers
After a load balancer is created, you will be returned to the load balancer list page. In the load balancer list page, you can check the basic information of the created load balancers. The items displayed on the list page are as follows:

* Name: Name of the load balancer specified when it is created.
* Type: Load balancer type
* IP Address: A private IP assigned by the VPC associated with the load balancer. From inside the VPC, you can access the load balancer through this IP. To access the load balancer from outside the VPC, you need to associate a floating IP.
* Network: Name of the VPC associated with the load balancer and the subnet CIDR.
* Status: Status of load balancer creation. ACTIVE means it has been created normally.
* Load balancer details: View details of the listeners and member groups connected to the load balancer.

> [Note] Provisioning status of a load balancer is determined as one of the following:

> | Status | Description |
> |--|--|
> | ACTIVE | A load balancer has been created and is operating normally |
> | PENDING_CREATE | Creating a load balancer <br> If the status does not change to ACTIVE within an hour after creation, contact the administrator. |
> | PENDING_UPDATE | Modifying load balancer configuration <br> If the status does not change to ACTIVE within an hour after modifying the configuration, contact the administrator. |
> | PENDING_DELETE | Deleting a load balancer<br> If the load balancer does not disappear from the list within an hour after deletion, contact the administrator. |
> | ERROR | Failed to create a load balancer<br> Contact the administrator. |
> | ERROR_MIGRATE | Failed to migrate a load balancer<br> Contact the administrator. |

### Modify Load Balancers and Details
Select a load balancer from the list, and a page of details shows up at the bottom, which is composed of the following tabs:

* Details of Load Balancer: Shows detailed information of a load balancer. Name and description of the selected load balancer, and whether to apply subnet static routing can be changed.
* Listener: Check detailed setting of listeners created under a selected load balancer. Add or delete listeners.
* Instance: View the list of instances registered as members to a selected load balancer. Register new instances as members or exclude existing ones.
* Statistics: Statistical information of a selected load balancer is available.

### Listener Changes and Details
On the main screen of the load balancer, select the desired load balancer detail view to see the listeners and member groups connected to the load balancer. From there, you can select the **Listeners** tab to create, change, or delete listeners.

#### Add Listeners
Listeners can be added by clicking the Add Listener button on the Listener tab in the detail screen of the load balancer. Items required to add listeners are the same as those required by the default listener during creation of the load balancer. When a listener is added, the load balancer port used by previous listeners can no longer be used.

#### Modify Listeners
To modify the setting of a listener, click Modify.

> [Note] You cannot change the listener protocol, load balancer port, and instance port.

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

> [Caution] Adding a certificate will restart the load balancer. Existing sessions will be maintained during the restart process, but new sessions will not be processed (about less than 1 second). Therefore, we recommend changing during a time that will not impact the service.

##### Change the Default Certificate
1. In the **Certificate** tab of the listener details screen, click the **Change Default Certificate** button.
2. Select the certificate to use as the default certificate.
3. After reviewing the warning message, select the checkbox and click **OK**.

> [Caution] Changing the default certificate will restart the load balancer. During the restart process, existing sessions will be maintained, but new sessions will not be processed (about less than 1 second). Therefore, we recommend changing during a time that will not impact the service.

##### Delete a Certificate
1. In the **Certificate** tab of the listener details screen, select the certificate to be deleted.
2. Click **Delete Certificate** button. 3. After reviewing the warning message, select the checkbox and click **OK**.

> [Caution] The default certificate cannot be deleted. To delete the default certificate, you must first change another certificate to the default and then delete it.

> [Caution] Deleting a certificate will restart the load balancer. During the restart process, existing sessions will be maintained, but new sessions cannot be processed (about 1 second). Therefore, we recommend performing the process during a time that will not impact the service.

### Custom Response Guide

You can configure custom responses in the load balancer listener. Using custom responses, you can directly deliver custom messages or HTML content to users when a specific HTTP error code occurs.

#### View and Configure Custom Responses

1. Click the **Listeners** tab on the load balancer details screen.
2. Select the listener for which you want to configure a custom response.
3. On the listener details screen, click **View/Change Custom Response Settings**.
4. You can enter and confirm the following items:
   - **Response Code**: select the HTTP status code to which the custom response will be applied. (Supported codes: 400, 403, 408, 500, 502, 503, 504)
   - **Content Type**: select the Content-Type of the response to be delivered to the user. (Choose from `text/html`, `text/plain`, `application/json`, `application/javascript`, and `text/css`)
   - **Response Body**: enter the body of the response to be displayed to the user. (up to 1,024 characters. The content can be HTML, text, or any other format, depending on the content type.)
5. After entering each item, click **Confirm** to generate the response. The generated response can be viewed in the list.

#### Delete a Custom Response

- You can delete a created custom response by selecting it from the list and clicking **Delete** button.
- If an error code corresponding to the deleted response occurs, the default system response will be displayed.

> [Note] Each error code can only be registered as a custom response once within the same listener.

> [Caution] When adding, modifying, or deleting a custom response, the listener may briefly restart (less than 1 second). Therefore, we recommend changing during a time when service impact is minimal.

### Guide to X-Forwarded Header Configuration

You can view and change the X-Forwarded header settings on a load balancer listener. The X-Forwarded header is used to forward the client's source information (protocol, port, IP address) to the backend server.

#### View and Configure the X-Forwarded Header

1. Click the **Listener** tab on the load balancer details screen.
2. Select the listener for which you want to configure the X-Forwarded header.
3. On the **Basic Information** tab on the listener details screen, click **View/Change Settings** button under the **X-Forwarded Header** section.
4. You can configure the following:
   - **X-Forwarded-Proto**: set whether to forward the protocol (http or https) used by the client to the backend server. Select either **Enable** or **Disable**.
   - **X-Forwarded-Port**: set whether to forward the port number the client connected to to the backend server. Select either **Enable** or **Disable**.
   - **X-Forwarded-for**: set whether to forward the client's original IP address to the backend server. Select either **Enable** or **Disable**.
5. After configuring each item, check the box in the warning message and click **OK** to apply the settings.

> [Caution] Changing the X-Forwarded header settings will cause the load balancer to restart. Existing sessions will be maintained during the restart process, but new sessions cannot be processed (about less than 1 second). Therefore, we recommend changing during a time when the service will not be affected.

#### Delete Listeners
To delete a listener, click Delete: cannot delete, though, if the load balancer has only one listener.

> [Caution] Add/Modify/Delete listeners causes reboot of a load balancer. During the reboot, existing connected sessions are maintained, but new sessions cannot be processed (less than 1 second). Therefore, it is recommended to proceed at a time that does not affect the service.

### Member Group Changes and Details
On the Load Balancers screen, select the desired load balancer's **View Details** to see the listeners and member groups connected to the load balancer. From there, you can select the **Member Groups** tab to create, change, or delete member groups.

#### Create Member Groups
Click **Create Member Group** to create additional member groups. The items required to create a member group are the same as those required for a member group when creating a load balancer.

#### Change Member Groups
Click **Change Member Group** to change settings related to the member group.

> [Caution] Member ports and protocols cannot be changed after a member group is created.

#### Delete Member Groups
Select the member group you want to delete and click **Delete Member Group** to delete that member group.

> [Caution] Creating/editing/deleting a member group restarts the load balancer. During the restart, existing connected sessions are preserved, but new sessions cannot be processed (for less than a second). Therefore, we recommend doing this at a time that does not impact service.

> [Note] When a member group is deleted, any L7 rules that had that member group as an action target will have their action type changed to Block.

### Member changes and details
On the Load Balancer **View Details** screen, select the **Member Group** tab, and then select the desired member group to view the details of the member group and the status of the members in the member group.

#### Add a member
After you select a member group, you'll see the **Basic Info**, **Members**, and **Check Status** tabs at the bottom of the screen. Select the **Members** tab to enroll the desired instances or IP addresses as members. You can only add instances that belong to the VPC to which the load balancer is attached and to VPCs that are peered to that VPC. You can specify your own destination port number for each member, and load balancing will be done with that destination port number.

> [Caution] If you have multiple members in a member group with different port numbers, be careful about setting the health check port. For example, if you have two members, such as port 80 on 192.168.0.10 and port 8080 on 192.168.0.10, selecting Health check port as member port will perform health checks on port 80 and port 8080 respectively. If you select Custom as the health check port and type 80, it will check port 80 even if the member port is port 8080. If the 80 port on 192.168.0.10 is active, then the member on the 8080 port for 192.168.0.10 is also considered ACTIVE because it is checking the status of the 80 port for 192.168.0.10.

#### Deactivate a member
You can temporarily exclude specific members from the service. Select the members you want to exclude, click the **Deactivate members** button, and then click **OK**.
The excluded members' permissions will change to **X** and their member status will change to **ONLINE**.

> [Note] The status of a member is determined by one of the following
> 
> | Status | Meaning |
> |--|--|
> | ACTIVE | Member connection complete, working fine |
> | INACTIVE | A member's health check is not being performed |
> | ONLINE | Member is disabled|
> | OFFLINE | Member connection failure <br> Contact your administrator.|

#### Delete Members
Instances that are no longer used may be deleted. Click Detach Instance of the instance to exclude, and it is deleted from the member of load balancer. Deletion from load balancer member does not mean its instance is also deleted.

> [Caution] Add/Disable/Delete Members causes reboot of a load balancer. During the reboot, existing connected sessions are maintained, but new sessions cannot be processed (less than 1 second). Therefore, it is recommended to proceed at a time that does not affect the service.

### Delete Load Balancers
Select the load balancer you want to delete from the load balancer list screen and click the Delete button to delete the load balancer.


## IP Access Control Groups
For more details on the features of IP access control, see [IP Access Control](/Network/Load%20Balancer/en/overview/#load-balancer-ip-access-control).

#### Create IP Access Control Groups
To create an IP access control group, click [Create Access Control Group] and enter the following values:

* Name: Enter the name of the access control group.
* Description: Enter the description of the access control group.
* IP Access Control Type: Select either Block or Allow.
* Add IP Access Control Target: Enter the access control target IP and its description. You can add multiple access control targets at once by clicking the "+" button on the right. To make bulk input easier, you can select [Enter in Mass]. In this case, enter "IP address or CIDR" and "description" on one line. You can enter up to 100 access control targets at one time.


Click [Confirm] and the groups and targets of access control are created.

> [Note] Number of groups and targets of IP access control
>
> Up to 10 access control groups can be created for each project.
> Up to 1,000 access control targets can be created for each project.


#### Change IP Access Control Groups
You can change the properties of an IP access control group. The properties you can change are name and description. The "IP Access Control Type" property cannot be changed.

#### Delete IP Access Control Groups
You can delete the selected IP access control groups. When you delete a group, all access control targets belonging to the group are also deleted.
When you delete an IP access control group, load balancers using the group will no longer use that policy.

#### Add IP Access Control Targets
If you select an access control group, the access control target menu appears at the bottom.
When a target is added to an access control group, the policy of the added IP or CIDR is reflected in all load balancers using this access control group.

#### Change IP Access Control Targets
You can change the properties of the access control target. You can only change the description.

#### Delete IP Access Control Targets
If you select an access control group, the access control target menu appears at the bottom.
If you delete a target belonging to an access control group, the policy of the corresponding IP or CIDR is deleted from all load balancers using this access control group.

#### Apply IP Access Control Groups
Select the load balancer to apply the IP access control group to. Select the group you want to configure for that load balancer and click Confirm.
Multiple groups with the same "access control type" can be applied to the load balancer.

<a id='restarting-guide-for-maintenance'></a>
## Guide to Restarting Load Balancers for Maintenance

NHN Cloud updates software of the load balancer equipment on a regular basis to enhance security and stability of the basic infrastructure services. For maintenance of the load balancer, the load balancer running in the maintenance target equipment must be restarted to be migrated to the load balancer equipment where maintenance has been completed.

Load balancers that require a restart have **! Restart** button displayed next to their names. You can use this button to restart the load balancers.

Go to the project with the load balancer specified as the maintenance target and perform a restart with the following procedure.

1. Check the load balancers for maintenance. A load balancer with the **! Restart** button next to its name is the maintenance target.
    ![image-001](http://static.toastoven.net/prod_load_balancer/lb_p_migration_en_1.png)
    Hover the mouse cursor over the  **! Restart** button to find the detailed maintenance schedule. It is recommended to perform maintenance at a time that does not affect service.
    ![image-002](http://static.toastoven.net/prod_load_balancer/lb_p_migration_en_2.png)
2. Select a load balancer for maintenance and click the **! Restart** button next to its name.
3. When the window asking if you want to restart the load balancer appears, click **Confirm**.
    ![image-003](http://static.toastoven.net/prod_load_balancer/lb_p_migration_en_3.png)
4. Wait until the status indicator turns green and the **! Restart** button disappears.
    If the status indicator does not change or the **! Restart** button does not disappear, try 'Refresh'.
    ![image-004](http://static.toastoven.net/prod_load_balancer/lb_p_migration_en_4.png)

The load balancer becomes inoperable while restarting is underway.

If the load balancer restart is not completed normally, it is automatically reported to the administrator, and NHN Cloud will contact you separately.
