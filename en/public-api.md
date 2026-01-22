## Network > Load Balancer > API v2 Guide

To use the API, you need an API endpoint and token. Please refer to [Prepare to Use the API](/Compute/Compute/ko/identity-api/) to prepare the information required to use the API.

The load balancer, listener, pool, health monitor, and member APIs use `network` type endpoints. The secret and secret container APIs are called using `key-manager` type endpoints. For the exact endpoint, refer to `serviceCatalog` in the token issuance response.

| Type | Region | Endpoint |
|---|---|---|
| network | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Korea (Gwangju) Region<br>Japan (Tokyo) Region<br>USA (California) Region | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://kr3-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com<br>https://us1-api-network-infrastructure.nhncloudservice.com |
| key-manager | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Korea (Gwangju) Region<br>Japan (Tokyo) Region<br>USA (California) Region |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr3-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-key-manager-infrastructure.nhncloudservice.com |


API responses may contain fields not specified in the guide. These fields are used internally by NHN Cloud and are subject to change without prior notice, so they are not used.

## Load Balancer

### List Load Balancers

```
GET /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Load balancer ID to query |
| name | Query | String | - | Load balancer name to query |
| provisioning_status | Query | Enum | - | Provisioning status of the load balancer to query |
| description | Query | String | - | Description of the load balancer to query |
| vip_address | Query | String | - | The IP address of the load balancer to query |
| vip_port_id | Query | UUID | - | The port ID of the load balancer to query |
| vip_subnet_id | Query | UUID | - | The subnet ID of the load balancer to query |
| operating_status | Query | Enum | - | The operational status of the load balancer to query |
| loadbalancer_type | Query | String | - | The type of the load balancer to query, either `shared` or `dedicated` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| loadbalancers | Body | Array | A list of load balancer information objects |
| loadbalancers.description | Body | String | Load balancer description |
| loadbalancers.provisioning_status | Body | Enum | Load Balancer Provisioning Status |
| loadbalancers.tenant_id | Body | String | Tenant ID |
| loadbalancers.provider | Body | String | Load Balancer Provider |
| loadbalancers.name | Body | String | Load Balancer Name |
| loadbalancers.listeners | Body | Object | List of Load Balancer Listener Objects |
| loadbalancers.listeners.id | Body | UUID | Listener ID |
| loadbalancers.pools | Body | Object | List of Load Balancer Pool Objects |
| loadbalancers.pools.id | Body | UUID | Pool ID |
| loadbalancers.vip_address | Body | String | Load Balancer IP |
| loadbalancers.vip_port_id | Body | UUID | Load Balancer Port ID |
| loadbalancers.vip_subnet_id | Body | UUID | Load balancer subnet ID |
| loadbalancers.id | Body | UUID | Load balancer ID |
| loadbalancers.operating_status | Body | Enum | Load balancer operating status |
| loadbalancers.admin_state_up | Body | Boolean | Load balancer admin control status |
| loadbalancers.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancers.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancers.ipacl_group_action | Body | String | Action of the IP ACL groups applied to the load balancer<br>One of `null`/`DENY`/`ALLOW` |
| loadbalancers.loadbalancer_type | Body | String | Load balancer type<br>One of `shared`/`dedicated` |

<details><summary>Example</summary>

```json
{
  "loadbalancers": [
    {
      "ipacl_group_action": "DENY",
      "description": "",
      "provisioning_status": "ACTIVE",
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "provider": "haproxy",
      "ipacl_groups": [
        {
          "ipacl_group_id": "04570ec5-456a-48ac-85ee-38adcc83ee70"
        }
      ],
      "name": "LB-1",
      "loadbalancer_type": "shared",
      "listeners": [
        {
          "id": "fe192219-0d4c-4145-9855-0af8c949dfe8"
        }
      ],
      "pools": [
        {
          "id": "766e51ff-4d29-4ab4-bfb6-4dab8d62803f"
        }
      ],
      "vip_address": "192.168.0.187",
      "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
      "workflow_status": "SUCCESS",
      "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
      "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
      "operating_status": "ONLINE",
      "admin_state_up": true,
      "ipacl_groups": [
        {
         "ipacl_group_id": "79ebf206-3463-4df1-a54c-4fc939f8c26c"
         },
         {
         "ipacl_group_id": "947030cc-635f-42d3-b745-770cf7b562fd"
         }
       ]
    }
  ]
}
```
</details>

---
### View Load Balancer

```
GET /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| loadbalancerId | URL | UUID | Yes | Load Balancer ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| loadbalancer | Body | Object | Load Balancer Information Object |
| loadbalancer.description | Body | String | Load Balancer Description |
| loadbalancer.provisioning_status | Body | Enum | Load Balancer Provisioning Status |
| loadbalancer.tenant_id | Body | String | Tenant ID |
| loadbalancer.provider | Body | String | Load Balancer Provider |
| loadbalancer.name | Body | String | Load Balancer Name |
| loadbalancer.listeners | Body | Object | List of Load Balancer Listener Objects |
| loadbalancer.listeners.id | Body | UUID | Listener ID |
| loadbalancers.pools | Body | Object | List of Load Balancer Pool Objects |
| loadbalancers.pools.id | Body | UUID | Pool ID |
| loadbalancer.vip_address | Body | String | Load Balancer IP |
| loadbalancer.vip_port_id | Body | UUID | Load Balancer Port ID |
| loadbalancer.vip_subnet_id | Body | UUID | Load Balancer Subnet ID |
| loadbalancer.id | Body | UUID | Load Balancer ID |
| loadbalancer.operating_status | Body | Enum | Load balancer operating status |
| loadbalancer.admin_state_up | Body | Boolean | Load balancer admin control status |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_group_action | Body | String | Action of IP ACL groups applied to the load balancer <br>One of `null`/`DENY`/`ALLOW` |
| loadbalancer.loadbalancer_type | Body | String | Load balancer type <br>One of `shared`/`dedicated` |


<details><summary>Example</summary>

```json
{
  "loadbalancer": {
    "ipacl_group_action": "DENY",
    "description": "",
    "provisioning_status": "ACTIVE",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "provider": "haproxy",
    "ipacl_groups": [
      {
        "ipacl_group_id": "04570ec5-456a-48ac-85ee-38adcc83ee70"
      }
    ],
    "name": "LB-1",
    "loadbalancer_type": "shared",
    "listeners": [
      {
        "id": "fe192219-0d4c-4145-9855-0af8c949dfe8"
      }
    ],
      "pools": [
        {
          "id": "766e51ff-4d29-4ab4-bfb6-4dab8d62803f"
        }
      ],
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true,
    "ipacl_groups": [
        {
         "ipacl_group_id": "79ebf206-3463-4df1-a54c-4fc939f8c26c"
         },
         {
         "ipacl_group_id": "947030cc-635f-42d3-b745-770cf7b562fd"
         }
     ]
  }
}
```
</details>

---
### Create Load Balancer

```
POST /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| loadbalancer | Body | Object | - | Load Balancer Information Object |
| loadbalancer.name | Body | String | - | Load Balancer Name |
| loadbalancer.description | Body | String | - | Load Balancer Description |
| loadbalancer.vip_subnet_id | Body | UUID | O | Load Balancer Subnet ID |
| loadbalancer.vip_address | Body | String | - | Load Balancer IP |
| loadbalancer.admin_state_up | Body | Boolean | - | Load Balancer admin control state. If omitted, it is set to `true` |
| loadbalancer.loadbalancer_type | Body | String | - | Load balancer type, `shared`/`dedicated` available<br> If omitted, `shared` is set |




<details><summary>Example</summary>

```json
{
    "loadbalancer": {
        "name": "LB-1",
        "description": "",
        "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
        "vip_address": "192.168.0.187",
        "admin_state_up": true
    }
}
```
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| loadbalancer | Body | Object | Load balancer information object |
| loadbalancer.description | Body | String | Load balancer description |
| loadbalancer.provisioning_status | Body | Enum | Load balancer provisioning status |
| loadbalancer.tenant_id | Body | String | Tenant ID |
| loadbalancer.provider | Body | String | Load balancer provider name |
| loadbalancer.name | Body | String | Load balancer name |
| loadbalancer.listeners | Body | Object | List of load balancer listener objects |
| loadbalancer.listeners.id | Body | UUID | Listener ID |
| loadbalancers.pools | Body | Object | List of load balancer pool objects |
| loadbalancers.pools.id | Body | UUID | Pool ID |
| loadbalancer.vip_address | Body | String | Load balancer IP |
| loadbalancer.vip_port_id | Body | UUID | Load balancer port ID |
| loadbalancer.vip_subnet_id | Body | UUID | Load balancer subnet ID |
| loadbalancer.id | Body | UUID | Load balancer ID |
| loadbalancer.operating_status | Body | Enum | Load balancer operating status |
| loadbalancer.admin_state_up | Body | Boolean | Load balancer admin control status |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_group_action | Body | String | Action of IP ACL groups applied to the load balancer<br>One of `null`/`DENY`/`ALLOW` |
| loadbalancer.loadbalancer_type | Body | String | Load balancer type<br>One of `shared`/`dedicated` |


<details><summary>Example</summary>

```json
{
  "loadbalancer": {
    "ipacl_group_action": "DENY",
    "description": "",
    "provisioning_status": "ACTIVE",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "provider": "haproxy",
    "ipacl_groups": [
      {
        "ipacl_group_id": "04570ec5-456a-48ac-85ee-38adcc83ee70"
      }
    ],
    "name": "LB-1",
    "loadbalancer_type": "shared",
    "listeners": [
      {
        "id": "fe192219-0d4c-4145-9855-0af8c949dfe8"
      }
    ],
      "pools": [
        {
          "id": "766e51ff-4d29-4ab4-bfb6-4dab8d62803f"
        }
      ],
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true,
    "ipacl_groups": []
  }
}
```
</details>

---
### Modify Load Balancer

```
PUT /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| loadbalancerId | URL | UUID | O | Load Balancer ID |
| loadbalancer | Body | Object | O | Load Balancer Information Object |
| loadbalancer.name | Body | String | - | Load Balancer Name |
| loadbalancer.description | Body | String | - | Load Balancer Description |
| loadbalancer.admin_state_up | Body | Boolean | - | Admin Control State of the Load Balancer |

<details><summary>Example</summary>

```json
{
    "loadbalancer": {
        "name": "LB-1",
        "description": "",
        "admin_state_up": true
    }
}
```
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| loadbalancer | Body | Object | Load balancer information object |
| loadbalancer.description | Body | String | Load balancer description |
| loadbalancer.provisioning_status | Body | Enum | Load balancer provisioning status |
| loadbalancer.tenant_id | Body | String | Tenant ID |
| loadbalancer.provider | Body | String | Load balancer provider name |
| loadbalancer.name | Body | String | Load balancer name |
| loadbalancer.listeners | Body | Object | List of load balancer listener objects |
| loadbalancer.listeners.id | Body | UUID | Listener ID |
| loadbalancers.pools | Body | Object | List of load balancer pool objects |
| loadbalancers.pools.id | Body | UUID | Pool ID |
| loadbalancer.vip_address | Body | String | Load balancer IP |
| loadbalancer.vip_port_id | Body | UUID | Load balancer port ID |
| loadbalancer.vip_subnet_id | Body | UUID | Load balancer subnet ID |
| loadbalancer.id | Body | UUID | Load balancer ID |
| loadbalancer.operating_status | Body | Enum | Load balancer operating status |
| loadbalancer.admin_state_up | Body | Boolean | Load balancer admin control status |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_group_action | Body | String | Action of IP ACL groups applied to the load balancer<br>One of `null`/`DENY`/`ALLOW` |
| loadbalancer.loadbalancer_type | Body | String | Load balancer type<br>One of `shared`/`dedicated` |


<details><summary>Example</summary>

```json
{
  "loadbalancer": {
    "ipacl_group_action": "DENY",
    "description": "",
    "provisioning_status": "ACTIVE",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "provider": "haproxy",
    "ipacl_groups": [
      {
        "ipacl_group_id": "04570ec5-456a-48ac-85ee-38adcc83ee70"
      }
    ],
    "name": "LB-1",
    "loadbalancer_type": "shared",
    "listeners": [
      {
        "id": "fe192219-0d4c-4145-9855-0af8c949dfe8"
      }
    ],
      "pools": [
        {
          "id": "766e51ff-4d29-4ab4-bfb6-4dab8d62803f"
        }
      ],
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true,
    "ipacl_groups": []
  }
}
```
</details>

---
### Delete Load Balancer

```
DELETE /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| loadbalancerId | URL | UUID | Yes | Load Balancer ID |

#### Response
This API does not return a response body.





















## Listener
### View Listener List

```
GET /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| default_pool_id | Query | UUID | - | Default member group (pool) ID registered with the listener |
| protocol | Query | Enum | - | Listener protocol <br>`TCP`, `HTTP`, `HTTPS`, `TERMINATED_HTTPS` |
| description | Query | String | - | Listener description |
| name | Query | String | - | Listener name |
| admin_state_up | Query | Boolean | - | Admin control state |
| connection_limit | Query | Integer | - | Listener connection limit |
| keepalive_timeout | Query | Integer | - | Listener keepalive timeout |
| protocol_port | Query | Integer | - | Listener port number |
| id | Query | UUID | - | Listener ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| listeners | Body | Array | List of listener information objects |
| listeners.default_pool_id | Body | UUID | Default member group (pool) ID registered with the listener |
| listeners.protocol | Body | Enum | Listener protocol <br>One of `TCP`, `HTTP`, `HTTPS`, `TERMINATED_HTTPS` |
| listeners.description | Body | String | Listener description |
| listeners.name | Body | String | Listener name |
| listeners.loadbalancers | Body | Array | List of load balancer clients with registered listeners |
| listeners.loadbalancers.id | Body | UUID | Load balancer ID |
| listeners.tenant_id | Body | String | Tenant ID |
| listeners.admin_state_up | Body | Boolean | Admin control state |
| listeners.connection_limit | Body | Integer | Listener connection limit |
| listeners.keepalive_timeout | Body | Integer | Listener keepalive timeout |
| listeners.default_tls_container_ref | Body | String | TLS certificate path registered in key-manager |
| listeners.sni_container_refs | Body | Array | List of SNI certificate paths registered in key-manager |
| listeners.protocol_port | Body | Integer | Listener port |
| listeners.id | Body | String | Listener ID |


<details><summary>Example</summary>
<p>

```json
{
  "listeners": [
    {
      "proxy_protocol": false,
      "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
      "protocol": "TERMINATED_HTTPS",
      "description": "",
      "name": "",
      "loadbalancers": [
        {
          "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
        }
      ],
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "admin_state_up": true,
      "connection_limit": 2000,
      "keepalive_timeout": 300,
      "tls_version": "TLSv1.0",
      "sni_container_ids": [],
      "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
      "sni_container_refs": [],
      "protocol_port": 443,
      "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
      "cert_expire_date": "2025-12-27T10:36:20+00:00"
    }
  ]
}
```

</p>
</details>


### View Listener

```
GET /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| listenerId | URL | UUID | Yes | Listener ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| listener | Body | Object | Listener information object |
| listener.default_pool_id | Body | UUID | Default member group (pool) ID registered with the listener |
| listener.protocol | Body | Enum | Listener protocol <br>One of `TCP`, `HTTP`, `HTTPS`, `TERMINATED_HTTPS` |
| listener.description | Body | String | Listener description |
| listener.name | Body | String | Listener name |
| listener.loadbalancers | Body | Array | List of load balancer objects to which the listener is registered |
| listener.loadbalancers.id | Body | UUID | Load balancer ID |
| listener.tenant_id | Body | String | Tenant ID |
| listener.admin_state_up | Body | Boolean | Admin control state |
| listener.connection_limit | Body | Integer | Listener's connection limit |
| listener.keepalive_timeout | Body | Integer | Listener's keepalive timeout |
| listener.default_tls_container_ref | Body | String | TLS certificate path registered in key-manager |
| listener.sni_container_refs | Body | Array | List of SNI certificate paths registered in key-manager |
| listener.protocol_port | Body | Integer | Listener port |
| listener.id | Body | UUID | Listener ID |


<details><summary>Example</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "protocol": "TERMINATED_HTTPS",
    "description": "",
    "name": "",
    "loadbalancers": [
      {
        "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "sni_container_ids": [],
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443,
    "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "cert_expire_date": "2025-12-27T10:36:20+00:00"
  }
}
```

</p>
</details>



---
### Create Listener

```
POST /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| listener | Body | Object | O | Listener information object |
| listener.protocol | Body | Enum | O | Listener protocol<br>One of `TCP`, `HTTP`, `HTTPS`, `TERMINATED_HTTPS` |
| listener.description | Body | String | - | Listener description |
| listener.name | Body | String | - | Listener name |
| listener.default_pool_id | Body | UUID | - | Default member group (pool) ID registered with the listener<br>If not specified, `Not used` is generated |
| listener.loadbalancer_id | Body | UUID | O | Load balancer ID |
| listener.admin_state_up | Body | Boolean | - | Admin control state |
| listener.connection_limit | Body | Integer | - | Listener connection limit |
| listener.keepalive_timeout | Body | Integer | - | Listener keepalive timeout |
| listener.default_tls_container_ref | Body | String | - | TLS certificate path registered in key-manager |
| listener.sni_container_refs | Body | Array | - | List of SNI certificate paths registered in key-manager |
| listener.protocol_port | Body | Integer | O | Listener port |


<details><summary>Example</summary>
<p>

```json
{
  "listener": {
    "protocol": "TERMINATED_HTTPS",
    "proxy_protocol": false,
    "description": "",
    "name": "",
    "loadbalancer_id":"7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| listener | Body | Object | Listener information object |
| listener.default_pool_id | Body | UUID | Default member group (pool) ID registered with the listener |
| listener.protocol | Body | Enum | Listener protocol <br>One of `TCP`, `HTTP`, `HTTPS`, `TERMINATED_HTTPS` |
| listener.description | Body | String | Listener description |
| listener.name | Body | String | Listener name |
| listener.loadbalancers | Body | Array | List of load balancer objects to which the listener is registered |
| listener.loadbalancers.id | Body | UUID | Load balancer ID |
| listener.tenant_id | Body | String | Tenant ID |
| listener.admin_state_up | Body | Boolean | Admin control status |
| listener.connection_limit | Body | Integer | Listener's connection limit |
| listener.keepalive_timeout | Body | Integer | Listener's keepalive timeout |
| listener.default_tls_container_ref | Body | String | TLS certificate path registered in key-manager |
| listener.sni_container_refs | Body | Array | List of SNI certificate paths registered in key-manager |
| listener.protocol_port | Body | Integer | Listener port |
| listener.id | Body | UUID | Listener ID |


<details><summary>Example</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "protocol": "TERMINATED_HTTPS",
    "description": "",
    "name": "",
    "loadbalancers": [
      {
        "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "sni_container_ids": [],
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443,
    "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "cert_expire_date": "2025-12-27T10:36:20+00:00"
  }
}
```
</p>
</details>

---
### Modify Listener

```
PUT /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| listenerId | URL | UUID | O | Listener ID |
| listener | Body | Object | O | Listener information object |
| listener.description | Body | String | - | Listener description |
| listener.name | Body | String | - | Listener name |
| listener.default_pool_id | Body | UUID | - | Default member group (pool) ID registered to the listener <br>Setting this value to null changes it to `disabled` |
| listener.admin_state_up | Body | Boolean | - | Admin control state |
| listener.connection_limit | Body | Integer | - | Listener connection limit |
| listener.keepalive_timeout | Body | Integer | - | Listener keepalive timeout |
| listener.enable_x_forwarded_proto | Body | Boolean | - | X-Forwarded-Proto/X-Forwarded-Prot headers on/off<br>Default: `true` |
| listener.enable_x_forwarded_port | Body | Boolean | - | X-Forwarded-Port header on/off<br>Default: `true` |
| listener.enable_x_forwarded_for | Body | Boolean | - | X-Forwarded-For header on/off<br>Default: `true` |
| listener.default_tls_container_ref | Body | String | - | TLS certificate path registered in key-manager |
| listener.sni_container_refs | Body | Array | - | List of SNI certificate paths registered in key-manager |

<details><summary>Example</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "description": "",
    "name": "",
    "default_pool_id": null,
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "enable_x_forwarded_proto": true,
    "enable_x_forwarded_port": true,
    "enable_x_forwarded_for": true,
    "tls_version": "TLSv1.0",
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": []
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| listener | Body | Object | Listener information object |
| listener.default_pool_id | Body | UUID | Default member group (pool) ID registered with the listener |
| listener.protocol | Body | Enum | Listener protocol <br>One of `TCP`, `HTTP`, `HTTPS`, `TERMINATED_HTTPS` |
| listener.description | Body | String | Listener description |
| listener.name | Body | String | Listener name |
| listener.loadbalancers | Body | Array | List of load balancer objects to which the listener is registered |
| listener.loadbalancers.id | Body | UUID | Load balancer ID |
| listener.tenant_id | Body | String | Tenant ID |
| listener.admin_state_up | Body | Boolean | Admin control status |
| listener.connection_limit | Body | Integer | Listener's connection limit |
| listener.keepalive_timeout | Body | Integer | Listener's keepalive timeout |
| listener.enable_x_forwarded_proto | Body | Boolean | X-Forwarded-Proto/X-Forwarded-Prot header on/off |
| listener.enable_x_forwarded_port | Body | Boolean | X-Forwarded-Port header on/off |
| listener.enable_x_forwarded_for | Body | Boolean | X-Forwarded-For header on/off |
| listener.default_tls_container_ref | Body | String | TLS certificate path registered in key-manager |
| listener.sni_container_refs | Body | Array | List of SNI certificate paths registered in key-manager |
| listener.protocol_port | Body | Integer | Listener Port |
| listener.id | Body | UUID | Listener ID |


<details><summary>Example</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "default_pool_id": null,
    "protocol": "TERMINATED_HTTPS",
    "description": "",
    "name": "",
    "loadbalancers": [
      {
        "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "enable_x_forwarded_proto": true,
    "enable_x_forwarded_port": true,
    "enable_x_forwarded_for": true,
    "tls_version": "TLSv1.0",
    "sni_container_ids": [],
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443,
    "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "cert_expire_date": "2025-12-27T10:36:20+00:00"
  }
}
```

</p>
</details>

---
### Delete Listener
Delete a specified listener.
```
DELETE /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| listenerId | URL | UUID | Yes | Listener ID |

#### Response

This API does not return a response body.

---

### Create Custom Response

```
POST /v2.0/lbaas/listeners/{listenerId}/errorpages
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| listenerId | URL | UUID | O | Listener ID |
| errorpage | Body | Object | O | Custom response information object |
| errorpage.code | Body | Integer | O | One of the error code
400, 403, 408, 500, 502, 503, and 504 |
| errorpage.content_type | Body | Enum | O | Content type<br>One of `application/javascript`, `application/json`, `text/css`, `text/html`, `text/plain` |
| errorpage.body | Body | String | O | Custom response body (up to 1024 characters) |

**Note**: Duplicate codes cannot be created for the same listener. (For example, creating multiple 504s.)

<details><summary>Example</summary>
<p>

```json
{
  "errorpage": {
    "code": 502,
    "content_type": "text/html",
    "body": "<html><body><h1>502 Bad Gateway</h1><p>The server encountered a temporary error and could not complete your request.</p></body></html>"
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| errorpage | Body | Object | Custom response information object |
| errorpage.id | Body | UUID | Custom response ID |
| errorpage.code | Body | Integer | Error code |
| errorpage.content_type | Body | Enum | Content type |
| errorpage.body | Body | String | Custom response body |
| errorpage.tenant_id | Body | String | Tenant ID |

<details><summary>Example</summary>
<p>

```json
{
  "errorpage": {
    "id": "9413aeba-b796-46eb-9ae5-862cc20897e2",
    "code": 502,
    "content_type": "text/html",
    "body": "<html><body><h1>502 Bad Gateway</h1><p>The server encountered a temporary error and could not complete your request.</p></body></html>",
    "tenant_id": "419a823563124dc5b5627f5e79db8174"
  }
}
```
</p>
</details>

---

### Modify Custom Response

```
PUT /v2.0/lbaas/listeners/{listenerId}/errorpages/{errorpageId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| listenerId | URL | UUID | O | Listener ID |
| errorpageId | URL | UUID | O | Custom response ID |
| errorpage | Body | Object | O | Custom response information object |
| errorpage.content_type | Body | Enum | O | Content type <br>`application/javascript`, `application/json`, `text/css`, `text/html`, `text/plain` |
| errorpage.body | Body | String | O | Custom response body (maximum 1,024 characters) |

**Note**: `code` cannot be modified.

<details><summary>Example</summary>
<p>

```json
{
  "errorpage": {
    "content_type": "application/json",
    "body": "{\"error\": {\"code\": 502, \"message\": \"Bad Gateway\"}}"
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| errorpage | Body | Object | Custom response information object |
| errorpage.id | Body | UUID | Custom response ID |
| errorpage.code | Body | Integer | Error code |
| errorpage.content_type | Body | Enum | Content type |
| errorpage.body | Body | String | Custom response body |
| errorpage.tenant_id | Body | String | Tenant ID |

<details><summary>Example</summary>
<p>

```json
{
  "errorpage": {
    "id": "9413aeba-b796-46eb-9ae5-862cc20897e2",
    "code": 502,
    "content_type": "application/json",
    "body": "{\"error\": {\"code\": 502, \"message\": \"Bad Gateway\"}}",
    "tenant_id": "419a823563124dc5b5627f5e79db8174"
  }
}
```
</p>
</details>

---

### Delete Custom Response

```
DELETE /v2.0/lbaas/listeners/{listenerId}/errorpages/{errorpageId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| listenerId | URL | UUID | O | Listener ID |
| errorpageId | URL | UUID | O | Custom Response ID |

#### Response

This API does not return a response body.

---

### View Custom Response

```
GET /v2.0/lbaas/listeners/{listenerId}/errorpages/{errorpageId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| listenerId | URL | UUID | Yes | Listener ID |
| errorpageId | URL | UUID | Yes | Custom Response ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| errorpage | Body | Object | Custom Response Information Object |
| errorpage.id | Body | UUID | Custom Response ID |
| errorpage.code | Body | Integer | Error Code |
| errorpage.content_type | Body | Enum | Content Type |
| errorpage.body | Body | String | Custom Response Body |
| errorpage.tenant_id | Body | String | Tenant ID |

<details><summary>Example</summary>
<p>

```json
{
  "errorpage": {
    "id": "9413aeba-b796-46eb-9ae5-862cc20897e2",
    "code": 502,
    "content_type": "text/html",
    "body": "<html><body><h1>502 Bad Gateway</h1><p>The server encountered a temporary error and could not complete your request.</p></body></html>",
    "tenant_id": "419a823563124dc5b5627f5e79db8174"
  }
}
```
</p>
</details>

---

### View Custom Response List

```
GET /v2.0/lbaas/listeners/{listenerId}/errorpages
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| listenerId | URL | UUID | Yes | Listener ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| errorpages | Body | Array | List of custom response information objects |
| errorpages.id | Body | UUID | Custom response ID |
| errorpages.code | Body | Integer | Error code |
| errorpages.content_type | Body | Enum | Content type |
| errorpages.body | Body | String | Custom response body |
| errorpages.tenant_id | Body | String | Tenant ID |

<details><summary>Example</summary>
<p>

```json
{
  "errorpages": [
    {
      "id": "9413aeba-b796-46eb-9ae5-862cc20897e2",
      "code": 502,
      "content_type": "text/html",
      "body": "<html><body><h1>502 Bad Gateway</h1><p>The server encountered a temporary error and could not complete your request.</p></body></html>",
      "tenant_id": "419a823563124dc5b5627f5e79db8174"
    },
    {
      "id": "d7dfd308-051a-46aa-a1af-753f2c110133",
      "code": 503,
      "content_type": "text/html",
      "body": "<html><body><h1>503 Service Unavailable</h1><p>The service is temporarily unavailable. Please try again later.</p></body></html>",
      "tenant_id": "419a823563124dc5b5627f5e79db8174"
    }
  ]
}
```
</p>
</details>

---














## Pool
### View Pool List

```
GET /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Pool ID |
| name | Query | String | - | Pool name |
| lb_algorithm | Query | Enum | - | Load balancing method for the pool <br> One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
| protocol | Query | Enum | - | Protocol of the member |
| admin_state_up | Query | Boolean | - | Admin control state |
| healthmonitor_id | Query | UUID | - | Health monitor ID for the pool |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| pools | Body | Array | List of pool information objects |
| pools.lb_algorithm | Body | Enum | The load balancing method for the pool <br> One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
| pools.protocol | Body | Enum | The protocol of the member |
| pools.description | Body | String | The description of the pool |
| pools.admin_state_up | Body | Boolean | The admin control state |
| pools.tenant_id | Body | String | Tenant ID |
| pools.session_persistence | Body | Object | The session persistence object for the pool |
| pools.session_persistence.type | Body | Enum | Session Persistence<br> Set to one of `SOURCE_IP`, `HTTP_COOKIE`, or `APP_COOKIE`<br> If you set it to `HTTP_COOKIE` or `APP_COOKIE`, it is recommended that you check whether the protocol of the connected listener is set to `HTTP` or `TERMINATED_HTTPS`.<br> If the protocol of the listener is set to `TCP` or `HTTPS`, the load balancer will not perform any session persistence-related actions even if you set session persistence to `HTTP_COOKIE` or `APP_COOKIE`. |
| pools.session_persistence.cookie_name | Body | String | Cookie name <br> The setting value is applied only when the session persistence type is `APP_COOKIE`. |
| pools.healthmonitor_id | Body | String | Health Monitor ID |
| pools.loadbalancers | Body | Array | List of load balancer objects registered with the pool |
| pools.loadbalancers.id | Body | UUID | Load Balancer ID |
| pools.listeners | Body | Array | List of listener objects registered to the pool |
| pools.listeners.id | Body | String | Listener ID |
| pools.members | Body | Array | List of member objects registered to the pool |
| pools.members.id | Body | String | Member ID |
| pools.id | Body | UUID | Pool ID |
| pools.name | Body | String | Pool name |

<details><summary>Example</summary>
<p>

```json
{
  "pools": [
    {
      "lb_algorithm": "ROUND_ROBIN",
      "protocol": "HTTP",
      "description": "",
      "admin_state_up": true,
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "member_port": 80,
      "session_persistence": null,
      "healthmonitor_id": "607c4da1-4fe2-4a3a-9527-82dd5a5c430e",
      "loadbalancers": [
        {
          "id": "2997cb9d-9c31-475d-b679-040569c9e27b"
        }
      ],
      "listeners": [
        {
          "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20"
        }
      ],
      "members": [
        {
          "id": "3e9a04d9-24a6-4304-83cc-6cf1e8deb7a7"
        },
        {
          "id": "2c60e53b-5ca0-4d22-bed8-dffc1e5276be"
        }
      ],
      "id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
      "name": ""
    }
  ]
}
```

</p>
</details>


### View Pool

```
GET /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| pool | Body | Object | Pool information object |
| pool.lb_algorithm | Body | Enum | The load balancing method for the pool <br> One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
| pool.protocol | Body | Enum | The member's protocol |
| pool.description | Body | String | Pool description |
| pool.admin_state_up | Body | Boolean | Admin control state |
| pool.tenant_id | Body | String | Tenant ID |
| pool.member_port | Body | Integer | Member port<br> The member port value specified when creating a member in the web console |
| pool.session_persistence | Body | Object | The pool's session persistence object |
| pool.session_persistence.type | Body | Enum | Session persistence<br> Set to one of `SOURCE_IP`, `HTTP_COOKIE`, or `APP_COOKIE`<br> If you set it to `HTTP_COOKIE` or `APP_COOKIE`, it is recommended that you check whether the protocol of the connected listener is set to `HTTP` or `TERMINATED_HTTPS`.<br> If the protocol of the listener is set to `TCP` or `HTTPS`, the load balancer will not perform any session persistence-related actions even if you set session persistence to `HTTP_COOKIE` or `APP_COOKIE`. |
| pool.session_persistence.cookie_name | Body | String | Cookie name <br>The setting applies only when the session persistence type is `APP_COOKIE`. |
| pool.healthmonitor_id | Body | UUID | Health Monitor ID |
| pool.loadbalancers | Body | Array | List of load balancer objects registered to the pool |
| pool.loadbalancers.id | Body | UUID | Load Balancer ID |
| pool.listeners | Body | Array | List of listener objects registered to the pool |
| pool.listeners.id | Body | UUID | Listener ID |
| pool.members | Body | Array | List of member objects registered to the pool |
| pool.members.id | Body | UUID | Member ID |
| pool.id | Body | UUID | Pool ID |
| pool.name | Body | String | Pool name |

<details><summary>Example</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "description": "",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "member_port": 80,
    "session_persistence": null,
    "healthmonitor_id": "607c4da1-4fe2-4a3a-9527-82dd5a5c430e",
    "loadbalancers": [
      {
        "id": "2997cb9d-9c31-475d-b679-040569c9e27b"
      }
    ],
    "listeners": [
      {
        "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20"
      }
    ],
    "members": [
      {
        "id": "3e9a04d9-24a6-4304-83cc-6cf1e8deb7a7"
      },
      {
        "id": "2c60e53b-5ca0-4d22-bed8-dffc1e5276be"
      }
    ],
    "id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "name": ""
  }
}
```

</p>
</details>



---
### Create Pool

```
POST /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| pool | Body | Object | O | Pool information object |
| pool.loadbalancer_id | Body | UUID | - | The load balancer ID to which the pool will be registered. Either the load balancer ID or the listener ID must be entered. |
| pool.listener_id | Body | UUID | - | The listener ID to which the pool will be registered. Either the load balancer ID or the listener ID must be entered. |
| pool.lb_algorithm | Body | Enum | O | The load balancing method of the pool <br> One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
| pool.protocol | Body | Enum | O | The member's protocol |
| pool.description | Body | String | - | Pool description |
| pool.admin_state_up | Body | Boolean | - | Admin control state |
| pool.member_port | Body | Integer | - | Member listening port<br>Forward traffic to this port.<br>Default is -1. |
| pool.session_persistence | Body | Object | - | Pool's session persistence object |
| pool.session_persistence.type | Body | Enum | - | Session Persistence<br> Set to `SOURCE_IP`, `HTTP_COOKIE`, or `APP_COOKIE`<br> If you set it to `HTTP_COOKIE` or `APP_COOKIE`, it is recommended that you also set the protocol of the connected listener to `HTTP` or `TERMINATED_HTTPS`.<br> If the listener's protocol is set to `TCP` or `HTTPS`, the load balancer will not perform any session persistence-related actions even if you set session persistence to `HTTP_COOKIE` or `APP_COOKIE`. |
| pools.session_persistence.cookie_name | Body | String | - | Cookie Name <br>The setting applies only when the session persistence type is `APP_COOKIE`. |
| pool.name | Body | String | - | Pool Name |



<details><summary>Example</summary>
<p>

```json
{
  "pool": {
    "listener_id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "description": "",
    "admin_state_up": true,
    "member_port": 80,
    "session_persistence": null,
    "name": ""
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| pool | Body | Object | Pool information object |
| pool.lb_algorithm | Body | Enum | Pool load balancing method <br> One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
| pool.protocol | Body | Enum | Member protocol |
| pool.description | Body | String | Pool description |
| pool.admin_state_up | Body | Boolean | Admin control state |
| pool.tenant_id | Body | String | Tenant ID |
| pool.session_persistence | Body | Object | - | Pool session persistence object |
| pool.session_persistence.type | Body | Enum | Session Persistence<br> Set to one of `SOURCE_IP`, `HTTP_COOKIE`, or `APP_COOKIE`<br> If you set it to `HTTP_COOKIE` or `APP_COOKIE`, it is recommended that you also set the protocol of the connected listener to `HTTP` or `TERMINATED_HTTPS`.<br> If the protocol of the listener is set to `TCP` or `HTTPS`, the load balancer will not perform any session persistence-related actions even if you set session persistence to `HTTP_COOKIE` or `APP_COOKIE`. |
| pool.healthmonitor_id | Body | String | Health Monitor ID |
| pool.loadbalancers | Body | Array | List of load balancer objects registered to the pool |
| pool.loadbalancers.id | Body | UUID | Load Balancer ID |
| pool.listeners | Body | Array | List of listener objects registered to the pool |
| pool.listeners.id | Body | UUID | Listener ID |
| pool.members | Body | Array | List of member objects registered in the pool |
| pool.members.id | Body | UUID | Member ID |
| pool.id | Body | UUID | Pool ID |
| pool.name | Body | String | Pool name |

<details><summary>Example</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "description": "",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "member_port": 80,
    "session_persistence": null,
    "healthmonitor_id": "607c4da1-4fe2-4a3a-9527-82dd5a5c430e",
    "loadbalancers": [
      {
        "id": "2997cb9d-9c31-475d-b679-040569c9e27b"
      }
    ],
    "listeners": [
      {
        "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20"
      }
    ],
    "members": [
      {
        "id": "3e9a04d9-24a6-4304-83cc-6cf1e8deb7a7"
      },
      {
        "id": "2c60e53b-5ca0-4d22-bed8-dffc1e5276be"
      }
    ],
    "id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "name": ""
  }
}
```

</p>
</details>

---
### Modify Pool

```
PUT /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```


#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| poolId | URL | UUID | O | Pool ID |
| pool | Body | Object | O | Pool information object |
| pool.lb_algorithm | Body | Enum | - | The load balancing method for the pool <br> One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
| pool.description | Body | String | - | Pool description |
| pool.admin_state_up | Body | Boolean | - | Admin control state |
| pool.session_persistence | Body | Object | - | The session persistence object for the pool |
| pool.session_persistence.type | Body | Enum | - | Session Persistence<br> Set to one of `SOURCE_IP`, `HTTP_COOKIE`, or `APP_COOKIE`<br> If you set it to `HTTP_COOKIE` or `APP_COOKIE`, it is recommended that you also check if the protocol of the connected listener is set to `HTTP` or `TERMINATED_HTTPS`.<br> If the protocol of the listener is set to `TCP` or `HTTPS`, the load balancer will not perform any session persistence-related actions even if you set session persistence to `HTTP_COOKIE` or `APP_COOKIE`. |
| pools.session_persistence.cookie_name | Body | String | - | Cookie Name <br>The setting is applied only when the session persistence type is `APP_COOKIE`. |
| pool.name | Body | String | - | Pool Name |



<details><summary>Example</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "description": "",
    "admin_state_up": true,
    "member_port": 80,
    "session_persistence": null,
    "name": ""
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| pool | Body | Object | Pool information object |
| pool.lb_algorithm | Body | Enum | Pool load balancing method <br> One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
| pool.protocol | Body | Enum | Member protocol |
| pool.description | Body | String | Pool description |
| pool.admin_state_up | Body | Boolean | Admin control state |
| pool.tenant_id | Body | String | Tenant ID |
| pools.session_persistence | Body | Object | Pool session persistence object |
| pool.session_persistence.type | Body | Enum | Session Persistence<br> Set to one of `SOURCE_IP`, `HTTP_COOKIE`, or `APP_COOKIE`<br> If you set it to `HTTP_COOKIE` or `APP_COOKIE`, it is recommended that you check whether the protocol of the connected listener is set to `HTTP` or `TERMINATED_HTTPS`.<br> If the protocol of the listener is set to `TCP` or `HTTPS`, the load balancer will not perform any session persistence-related actions even if you set session persistence to `HTTP_COOKIE` or `APP_COOKIE`. |
| pools.session_persistence.cookie_name | Body | String | Cookie name <br> The setting value is applied only when the session persistence type is `APP_COOKIE`. |
| pool.healthmonitor_id | Body | UUID | Health Monitor ID |
| pool.loadbalancers | Body | Array | List of load balancer objects registered with the pool |
| pool.loadbalancers.id | Body | UUID | Load Balancer ID |
| pool.listeners | Body | Array | List of listener objects registered to the pool |
| pool.listeners.id | Body | UUID | Listener ID |
| pool.members | Body | Array | List of member objects registered to the pool |
| pool.members.id | Body | UUID | Member ID |
| pool.id | Body | UUID | Pool ID |
| pool.name | Body | String | Pool name |

<details><summary>Example</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "description": "",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "member_port": 80,
    "session_persistence": null,
    "healthmonitor_id": "607c4da1-4fe2-4a3a-9527-82dd5a5c430e",
    "loadbalancers": [
      {
        "id": "2997cb9d-9c31-475d-b679-040569c9e27b"
      }
    ],
    "listeners": [
      {
        "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20"
      }
    ],
    "members": [
      {
        "id": "3e9a04d9-24a6-4304-83cc-6cf1e8deb7a7"
      },
      {
        "id": "2c60e53b-5ca0-4d22-bed8-dffc1e5276be"
      }
    ],
    "id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "name": ""
  }
}
```

</p>
</details>

---
### Delete Pool
Delete a specified pool.
```
DELETE /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| poolId | URL | UUID | O | Pool ID |

#### Response

This API does not return a response body.




























## Health Monitor
### View Health Monitor List

```
GET /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Health Monitor ID |
| admin_state_up | Query | Boolean | - | Admin Control State |
| delay | Query | Integer | - | Health Check Interval (seconds) |
| expected_codes | Query | String | - | HTTP response codes for members to be considered healthy <br> Available as a single value (200), a list (201, 202), or a range (201-204) <br> If the health check type is set to `TCP`, this field is ignored. |
| max_retries | Query | Integer | - | Maximum number of retries |
| http_method | Query | Enum | - | HTTP method to use for health check <br> If the health check type is set to `TCP`, the value set in this field will be ignored.|
| timeout | Query | Integer | - | Health check response wait time (in seconds) |
| url_path | Query | String | - | Health check request URL <br> If the health check type is set to `TCP`, the value set in this field will be ignored.|
| type | Query | Enum | - | Protocol to use for health check. One of `TCP`, `HTTP`, or `HTTPS` |
| host_header | Query | String | - | Host header field value to use for health check <br> If the health check type is set to `TCP`, the value set in this field will be ignored.|

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| healthmonitors | Body | Array | List of health monitor information objects |
| healthmonitors.admin_state_up | Body | Boolean | Admin control state |
| healthmonitors.delay | Body | Integer | Health check interval (seconds) |
| healthmonitors.health_check_port | Body | Integer | Member port for health check <br> * If `member-port` or 0, health checks are performed on the specified port number for each member. <br> * If positive, health checks are performed on the entered port number regardless of the specified port number for each member. |
| healthmonitors.expected_codes | Body | String | HTTP response codes for members to be considered healthy <br> Available as a single value (200), a list (201,202), or a range (201-204). <br> If the health check type is set to `TCP`, the value set in this field is ignored. |
| healthmonitors.max_retries | Body | Integer | Maximum number of retries |
| healthmonitors.http_method | Body | Enum | HTTP method to use for health checks <br> If the health check type is set to `TCP`, the value set in this field will be ignored.|
| healthmonitors.timeout | Body | Integer | Time to wait for health check responses (in seconds) |
| healthmonitors.pools | Body | Array | List of pool objects to which health monitors are connected |
| healthmonitors.pools.id | Body | UUID | Pool ID |
| healthmonitors.url_path | Body | String | Health check request URL <br> If the health check type is set to `TCP`, the value set in this field will be ignored.|
| healthmonitors.type | Body | Enum | Protocol to use for health checks. One of `TCP`, `HTTP`, or `HTTPS` |
| healthmonitors.id | Body | UUID | Health monitor ID |
| healthmonitor.host_header | Body | String | Field value of the host header to be used for status check<br> If the status check type is set to `TCP`, the value set in this field is ignored.|



<details><summary>Example</summary>
<p>

```json
{
  "healthmonitors": [
    {
      "admin_state_up": true,
      "health_check_port": 80,
      "delay": 30,
      "expected_codes": "200",
      "max_retries": 2,
      "http_method": "GET",
      "timeout": 5,
      "pools": [
        {
          "id": "872dc92f-777b-4e0f-9413-0132b98bc60b"
        }
      ],
      "url_path": "/",
      "type": "HTTP",
      "id": "a567e19b-260f-4fda-8a66-d5e4c237a780"
    }
  ]
}
```

</p>
</details>


### View Health Monitor

```
GET /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| healthMonitorId | URL | UUID | Yes | Health Monitor ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| healthmonitor | Body | Object | Health Monitor Information Object |
| healthmonitor.admin_state_up | Body | Boolean | Admin Control State |
| healthmonitor.delay | Body | Integer | Health Check Interval (Seconds) |
| healthmonitor.health_check_port | Body | Integer | Member Port for Health Check <br> * If `member-port` or 0, health checks are performed on the specified port number for each member. <br> * If the number is a positive number, the health check will be performed using the entered port number, regardless of the port number specified for each member. |
| healthmonitor.expected_codes | Body | String | HTTP response code of the member to be considered healthy <br> Available as a single value (200), a list (201,202), or a range (201-204) <br> If the health check type is set to `TCP`, the value set in this field will be ignored. |
| healthmonitor.max_retries | Body | Integer | Maximum number of retries |
| healthmonitor.http_method | Body | Enum | HTTP method to use for the health check <br> If the health check type is set to `TCP`, the value set in this field will be ignored. |
| healthmonitor.timeout | Body | Integer | Time to wait for a health check response (in seconds) |
| healthmonitor.pools | Body | Array | List of pool objects to which the health monitor is connected |
| healthmonitor.pools.id | Body | UUID | Full ID |
| healthmonitor.url_path | Body | String | Health check request URL<br> If the health check type is set to `TCP`, the value set in this field will be ignored.|
| healthmonitor.type | Body | Enum | Protocol to use for health check. One of `TCP`, `HTTP`, or `HTTPS` |
| healthmonitor.id | Body | UUID | Health Monitor ID |
| healthmonitor.host_header | Body | String | Field value of the host header to use for health check<br> If the health check type is set to `TCP`, the value set in this field will be ignored.|



<details><summary>Example</summary>
<p>

```json
{
  "healthmonitor": {
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "pools": [
      {
        "id": "872dc92f-777b-4e0f-9413-0132b98bc60b"
      }
    ],
    "url_path": "/",
    "type": "HTTP",
    "id": "a567e19b-260f-4fda-8a66-d5e4c237a780"
  }
}
```

</p>
</details>



---
### Create Health Monitor

```
POST /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| healthmonitor | Body | Object | O | Health Monitor Information Object |
| healthmonitor.pool_id | Body | UUID | O | Pool ID to which the health monitor will be connected |
| healthmonitor.admin_state_up | Body | Boolean | - | Admin Control State |
| healthmonitor.health_check_port | Body | Integer | - | Member Port to be Checked <br> * If `member-port` or 0 is specified, a health check will be performed on the specified port number for each member. <br> * If a positive number is entered, a health check will be performed on the entered port number regardless of the port number specified for each member. |
| healthmonitor.delay | Body | Integer | O | Health Check Interval (seconds) |
| healthmonitor.expected_codes | Body | String | - | HTTP response code of the member to be considered healthy. If omitted, it will be set to 200. <br> Can be a single value (200), a list (201, 202), or a range (201-204). <br> This field is ignored if the health check type is set to `TCP`. |
| healthmonitor.max_retries | Body | Integer | O | Maximum number of retries |
| healthmonitor.http_method | Body | Enum | - | HTTP method to use for the health check. If omitted, `GET` will be used. <br> This field is ignored if the health check type is set to `TCP`. |
| healthmonitor.timeout | Body | Integer | O | Time to wait for a health check response (in seconds). |
| healthmonitor.url_path | Body | String | - | The health check request URL. If omitted, `/` will be used. <br> If the health check type is set to `TCP`, the value set in this field will be ignored.|
| healthmonitor.type | Body | Enum | O | Protocol to use for health check. One of `TCP`, `HTTP`, or `HTTPS` |
| healthmonitor.host_header | Body | String | - | Field value of the host header to use for health check<br> If the health check type is set to `TCP`, the value set in this field will be ignored.|





<details><summary>Example</summary>
<p>

```json
{
  "healthmonitor": {
    "pool_id": "872dc92f-777b-4e0f-9413-0132b98bc60b",
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "url_path": "/",
    "type": "HTTP"
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| healthmonitor | Body | Object | Health Monitor Information Object |
| healthmonitor.admin_state_up | Body | Boolean | Admin Control State |
| healthmonitor.delay | Body | Integer | Health Check Interval (seconds) |
| healthmonitor.health_check_port | Body | Integer | Member Port for Health Check <br> * If `member-port` or 0, the health check will be performed on the specified port number for each member. <br> * If positive, the health check will be performed on the entered port number regardless of the specified port number for each member. |
| healthmonitor.expected_codes | Body | String | HTTP response code of the member to be considered healthy. If omitted, it will be set to 200. <br> Can be a single value (200), a list (201,202), or a range (201-204). <br> If the health check type is set to `TCP`, the value set in this field will be ignored. |
| healthmonitor.max_retries | Body | Integer | Maximum number of retries |
| healthmonitor.http_method | Body | Enum | HTTP method to use for health checks. <br> If the health check type is set to `TCP`, the value set in this field will be ignored. |
| healthmonitor.timeout | Body | Integer | Time to wait for health check responses (in seconds). |
| healthmonitor.pools | Body | Array | List of pool objects to which health monitors are connected. |
| healthmonitor.pools.id | Body | UUID | Pool ID |
| healthmonitor.url_path | Body | String | Health Check Request URL<br> If the health check type is set to `TCP`, the value set in this field will be ignored.|
| healthmonitor.type | Body | Enum | Protocol to use for health check. One of `TCP`, `HTTP`, or `HTTPS` |
| healthmonitor.id | Body | UUID | Health Monitor ID |
| healthmonitor.host_header | Body | String | Field value of the host header to use for health check<br> If the health check type is set to `TCP`, the value set in this field will be ignored.|



<details><summary>Example</summary>
<p>

```json
{
  "healthmonitor": {
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "pools": [
      {
        "id": "872dc92f-777b-4e0f-9413-0132b98bc60b"
      }
    ],
    "url_path": "/",
    "type": "HTTP",
    "id": "a567e19b-260f-4fda-8a66-d5e4c237a780"
  }
}
```

</p>
</details>

---
### Modify Health Monitor

```
PUT /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| healthmonitorId | URL | UUID | O | Health Monitor ID |
| healthmonitor | Body | Object | O | Health Monitor Information Object |
| healthmonitor.admin_state_up | Body | Boolean | - | Admin Control State |
| healthmonitor.health_check_port | Body | Integer | - | Member Port for Health Check <br> * If `member-port` or 0 is specified, a health check will be performed on the specified port number for each member. <br> * If a positive number is entered, a health check will be performed on the entered port number regardless of the port number specified for each member. |
| healthmonitor.delay | Body | Integer | - | Health Check Interval (seconds) |
| healthmonitor.expected_codes | Body | String | - | HTTP response code of the member to be considered healthy<br>Can be a single value (200), a list (201, 202), or a range (201-204)<br> If the health check type is set to `TCP`, the value set in this field is ignored.|
| healthmonitor.max_retries | Body | Integer | - | Maximum number of retries |
| healthmonitor.http_method | Body | Enum | - | HTTP method to use for the health check <br> If the health check type is set to `TCP`, the value set in this field is ignored.|
| healthmonitor.timeout | Body | Integer | - | Time to wait for a health check response (in seconds) |
| healthmonitor.url_path | Body | String | - | Health check request URL<br> If the health check type is set to `TCP`, the value set in this field is ignored.|
| healthmonitor.host_header | Body | String | - | Field value of the host header to be used for status check<br> If the status check type is set to `TCP`, the value set in this field is ignored.|


<details><summary>Example</summary>
<p>

```json
{
  "healthmonitor": {
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "url_path": "/"
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| healthmonitor | Body | Object | Health Monitor Information Object |
| healthmonitor.admin_state_up | Body | Boolean | Administrator Control State |
| healthmonitor.delay | Body | Integer | Health Check Interval (Seconds) |
| healthmonitor.health_check_port | Body | Integer | Member Port Targeted by Health Check <br> * If `member-port` or 0, the health check is performed on the specified port number for each member. <br> * If positive, the health check is performed on the entered port number regardless of the specified port number for each member. |
| healthmonitor.expected_codes | Body | String | The HTTP response code of the member to be considered healthy. <br>A single value (200), a list (201, 202), or a range (201-204) can be used. <br> This field is ignored if the health check type is set to `TCP`. |
| healthmonitor.max_retries | Body | Integer | Maximum number of retries |
| healthmonitor.http_method | Body | Enum | HTTP method to use for the health check. <br> This field is ignored if the health check type is set to `TCP`. |
| healthmonitor.timeout | Body | Integer | Time to wait for a health check response (in seconds) |
| healthmonitor.pools | Body | Array | List of pool objects to which the health monitor is connected |
| healthmonitor.pools.id | Body | UUID | Pool ID |
| healthmonitor.url_path | Body | String | Health Check Request URL<br> If the health check type is set to `TCP`, the value set in this field will be ignored.|
| healthmonitor.type | Body | Enum | Protocol to use for health check. One of `TCP`, `HTTP`, or `HTTPS` |
| healthmonitor.id | Body | UUID | Health Monitor ID |
| healthmonitor.host_header | Body | String | Field value of the host header to use for health check<br> If the health check type is set to `TCP`, the value set in this field will be ignored.|

<details><summary>Example</summary>
<p>

```json
{
  "healthmonitor": {
    "admin_state_up": true,
    "health_check_port": 80,
    "delay": 30,
    "expected_codes": "200",
    "max_retries": 2,
    "http_method": "GET",
    "timeout": 5,
    "pools": [
      {
        "id": "872dc92f-777b-4e0f-9413-0132b98bc60b"
      }
    ],
    "url_path": "/",
    "type": "HTTP",
    "id": "a567e19b-260f-4fda-8a66-d5e4c237a780"
  }
}
```

</p>
</details>

---
### Delete Health Monitor

```
DELETE /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| healthMonitorId | URL | UUID | Yes | Health Monitor ID |

#### Response

This API does not return a response body.






























## Member
### View Member List

```
GET /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| poolId | URL | UUID | Yes | Pool ID to which the member belongs |
| id | Query | UUID | - | Member ID |
| weight | Query | Integer | - | Member weight |
| admin_state_up | Query | Boolean | - | Admin control status |
| subnet_id | Query | UUID | - | Member's subnet ID |
| tenant_id | Query | String | - | Tenant ID |
| address | Query | String | - | Member's IP address |
| protocol_port | Query | Integer | - | Member's port |
| operating_status | Query | Enum | - | Member's operating status |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| members | Body | Array | List of member information objects |
| members.weight | Body | Integer | Member weight |
| members.admin_state_up | Body | Boolean | Admin control status |
| members.subnet_id | Body | UUID | Member's subnet ID |
| members.tenant_id | Body | String | Tenant ID |
| members.address | Body | String | Member's IP address |
| members.protocol_port | Body | Integer | Member's port |
| members.id | Body | UUID | Member ID |
| members.operating_status | Body | Enum | Member's operating status |

<details><summary>Example</summary>
<p>

```json
{
  "members": [
    {
      "weight": 1,
      "admin_state_up": true,
      "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "address": "192.168.0.188",
      "protocol_port": 80,
      "id": "699d5013-ce45-4471-9cc3-6c2f5ad56b7f",
      "operating_status": "INACTIVE"
    }
  ]
}
```

</p>
</details>


### View Member

```
GET /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| poolId | URL | UUID | Yes | Pool ID to which the member belongs |
| memberId | URL | UUID | Yes | Member ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| member | Body | Object | Member information object |
| member.weight | Body | Integer | Member weight |
| member.admin_state_up | Body | Boolean | Admin control state |
| member.subnet_id | Body | UUID | Member's subnet ID |
| member.tenant_id | Body | String | Tenant ID |
| member.address | Body | String | Member's IP address |
| member.protocol_port | Body | Integer | Member's port |
| member.id | Body | UUID | Member ID |
| member.operating_status | Body | Enum | Member's operating status |

<details><summary>Example</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true,
    "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "address": "192.168.0.188",
    "protocol_port": 80,
    "id": "699d5013-ce45-4471-9cc3-6c2f5ad56b7f",
    "operating_status": "INACTIVE"
  }
}
```

</p>
</details>

---
### Create Member

```
POST /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| poolId | URL | UUID | O | Pool ID to which the member belongs |
| member | Body | Object | O | Member information object |
| member.weight | Body | Integer | - | Member weight |
| member.admin_state_up | Body | Boolean | - | Admin control state |
| member.subnet_id | Body | UUID | O | Member's subnet ID |
| member.address | Body | String | O | Member's IP address |
| member.protocol_port | Body | Integer | O | Member's port |


<details><summary>Example</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true,
    "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "address": "192.168.0.188",
    "protocol_port": 80
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| member | Body | Object | Member Information Object |
| member.weight | Body | Integer | Member Weight |
| member.admin_state_up | Body | Boolean | Admin Control Status |
| member.subnet_id | Body | UUID | Member's Subnet ID |
| member.tenant_id | Body | String | Tenant ID |
| member.address | Body | String | Member's IP Address |
| member.protocol_port | Body | Integer | Member's Port |
| member.id | Body | UUID | Member ID |
| member.operating_status | Body | Enum | Member's Operating Status |

<details><summary>Example</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true,
    "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "address": "192.168.0.188",
    "protocol_port": 80,
    "id": "699d5013-ce45-4471-9cc3-6c2f5ad56b7f",
    "operating_status": "INACTIVE"
  }
}
```

</p>
</details>

---
### Modify Member

```
PUT /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| poolId | URL | UUID | O | Pool ID to which the member belongs |
| memberId | URL | UUID | O | Member ID |
| member | Body | Object | O | Member Information Object |
| member.weight | Body | Integer | - | Member Weight |
| member.admin_state_up | Body | Boolean | - | Admin Control State |

<details><summary>Example</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| member | Body | Object | Member information object |
| member.weight | Body | Integer | Member weight |
| member.admin_state_up | Body | Boolean | Admin control status |
| member.subnet_id | Body | UUID | Member's subnet ID |
| member.tenant_id | Body | String | Tenant ID |
| member.address | Body | String | Member's IP address |
| member.protocol_port | Body | Integer | Member's port |
| member.id | Body | UUID | Member ID |
| member.operating_status | Body | Enum | Member's operating status |

<details><summary>Example</summary>
<p>

```json
{
  "member": {
    "weight": 1,
    "admin_state_up": true,
    "subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "address": "192.168.0.188",
    "protocol_port": 80,
    "id": "699d5013-ce45-4471-9cc3-6c2f5ad56b7f",
    "operating_status": "INACTIVE"
  }
}
```

</p>
</details>

---
### Delete Member

```
DELETE /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| poolId | URL | UUID | O | Pool ID to which the member belongs |
| memberId | URL | UUID | O | Member ID |

#### Response

This API does not return a response body.


















## L7 Polilcy

### View L7 Policy List

```
GET /v2.0/lbaas/l7policies
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | L7 policy ID to query |
| name | Query | String | - | L7 policy name to query |
| description | Query | String | - | Description of the L7 policy to query |
| listener_id | Query | UUID | - | Listener ID of the L7 policy to query |
| action | Query | Enum | - | Action of the L7 policy to query<br> One of `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT` |
| redirect_pool_id | Query | UUID | - | Redirect pool ID of the L7 policy to be retrieved <br>Applies only when the action is `REDIRECT_TO_POOL` |
| redirect_url | Query | String | - | Redirect URL of the L7 policy to be retrieved <br>Applies only when the action is `REDIRECT_TO_URL` |
| redirect_http_code | Query | Integer | - | Redirect HTTP response code of the L7 policy |
| position | Query | Integer | - | Priority of the L7 policy to be retrieved |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| l7policies | Body | Array | List of L7 policy objects |
| l7policies.description | Body | String | L7 policy description |
| l7policies.tenant_id | Body | String | Tenant ID |
| l7policies.listener_id | Body | UUID | Listener ID of the L7 policy |
| l7policies.name | Body | String | L7 policy name |
| l7policies.rules | Body | Object | List of L7 policy rule objects |
| l7policies.rules.id | Body | UUID | L7 rule ID |
| l7policies.id | Body | UUID | L7 policy ID |
| l7policies.admin_state_up | Body | Boolean | L7 policy admin control state |
| l7policies.action | Body | Enum | Action of the L7 policy<br> One of `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT` |
| l7policies.redirect_pool_id | Body | UUID | Redirect pool ID of the L7 policy<br> Applies only when the action is `REDIRECT_TO_POOL` |
| l7policies.redirect_url | Body | String | Redirect URL of the L7 policy <br>Applies only when the action is `REDIRECT_TO_URL` |
| l7policies.redirect_http_code | Body | Integer | - | Redirect HTTP response code of the L7 policy |
| l7policies.position | Body | Integer | Priority of the L7 policy |

<details><summary>Example</summary>

```json
{
  "l7policies": [
    {
      "redirect_pool_id": null,
      "description": "",
      "admin_state_up": true,
      "rules": [
        {
          "id": "1e982fc1-0e54-4e1c-96c3-c9796cba373b"
        }
      ],
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "listener_id": "2a38f448-c898-4694-9808-685dd6360dab",
      "redirect_url": null,
      "action": "REJECT",
      "position": 1,
      "id": "9376c901-64cc-46a0-bab3-1b4bf42699ad",
      "name": "L7Policy"
    }
  ]
}
```
</details>

---
### View L7 Policy

```
GET /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| l7policyId | URL | UUID | Yes | L7 Policy ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| l7policy | Body | Object | L7 Policy Object |
| l7policy.description | Body | String | L7 Policy Description |
| l7policy.tenant_id | Body | String | Tenant ID |
| l7policy.listener_id | Body | UUID | L7 Policy Listener ID |
| l7policy.name | Body | String | L7 Policy Name |
| l7policy.rules | Body | Object | List of L7 policy rule objects |
| l7policy.rules.id | Body | UUID | L7 rule ID |
| l7policy.id | Body | UUID | L7 policy ID |
| l7policy.admin_state_up | Body | Boolean | L7 policy administrator control state |
| l7policy.action | Body | Enum | Action of the L7 policy<br> One of `REDIRECT_TO_POOL`, `REDIRECT_TO_URL`, or `REJECT` |
| l7policy.redirect_pool_id | Body | UUID | Redirect pool ID of the L7 policy<br> Only applies when the action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | Redirect URL of the L7 policy<br> Only applies when the action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code | Body | Integer | - | L7 policy's redirect HTTP response code |
| l7policy.position | Body | Integer | Priority of the L7 policy |

<details><summary>Example</summary>

```json
{
  "l7policy": {
    "redirect_pool_id": null,
    "description": "",
    "admin_state_up": true,
    "rules": [
      {
        "id": "1e982fc1-0e54-4e1c-96c3-c9796cba373b"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "listener_id": "2a38f448-c898-4694-9808-685dd6360dab",
    "redirect_url": null,
    "action": "REJECT",
    "position": 1,
    "id": "9376c901-64cc-46a0-bab3-1b4bf42699ad",
    "name": "L7Policy"
  }
}
```
</details>

---
### Create L7 Policy

```
POST /v2.0/lbaas/l7policies
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| l7policy | Body | Object | - | L7 policy object |
| l7policy.description | Body | String | - | L7 policy description |
| l7policy.listener_id | Body | UUID | O | Listener ID of the L7 policy |
| l7policy.name | Body | String | - | L7 policy name |
| l7policy.admin_state_up | Body | Boolean | - | L7 policy administrator control state. If omitted, set to `true` |
| l7policy.action | Body | Enum | O | L7 policy action<br> One of `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT` |
| l7policy.redirect_pool_id | Body | UUID | - | Redirect pool ID of the L7 policy<br>Required if the action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | - | Redirect URL of the L7 policy<br>Required if the action is `REDIRECT_TO_URL` <br> * The input format is `#{protocol}://#{host}:#{port}/#{path}?#{query}`. If you input it in the `#{_}` format, the value of the existing request will be maintained. If you directly input a value other than `#{_}`, the value will be applied to the redirect URL and returned to the client. <br> * To prevent infinite redirects, at least one of protocol, host, port, and path must be changed. <br> * If you input it in an incorrect format, the redirect URL may be converted to a value different from the actual input.| | l7policy.redirect_http_code | Body | Integer | - | The redirect HTTP response code of the L7 policy <br> One of 301 or 302. The default is 302. |
| l7policy.position | Body | Integer | - | The priority of the L7 policy. If omitted, it is set to the last priority. |



<details><summary>Example</summary>

```json
{
  "l7policy": {
    "action": "REJECT",
    "position": 1,
    "listener_id": "2a38f448-c898-4694-9808-685dd6360dab",
    "admin_state_up": true
  }
}
```
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| l7policy | Body | Object | L7 policy object |
| l7policy.description | Body | String | L7 policy description |
| l7policy.tenant_id | Body | String | Tenant ID |
| l7policy.listener_id | Body | UUID | L7 policy listener ID |
| l7policy.name | Body | String | L7 policy name |
| l7policy.rules | Body | Object | L7 policy rule object list |
| l7policy.rules.id | Body | UUID | L7 rule ID |
| l7policy.id | Body | UUID | L7 policy ID |
| l7policy.admin_state_up | Body | Boolean | L7 policy administrator control state |
| l7policy.action | Body | Enum | Action of L7 policy<br> One of `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT` |
| l7policy.redirect_pool_id | Body | UUID | Redirect pool ID of L7 policy<br> Only applies when action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | Redirect URL of L7 policy<br> Only applies when action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code | Body | Integer | - | Redirect HTTP response code of L7 policy |
| l7policy.position | Body | Integer | Priority of L7 policy |


<details><summary>Example</summary>

```json
{
  "l7policy": {
    "redirect_pool_id": null,
    "description": "",
    "admin_state_up": true,
    "rules": [
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "listener_id": "2a38f448-c898-4694-9808-685dd6360dab",
    "redirect_url": null,
    "action": "REJECT",
    "position": 1,
    "id": "9376c901-64cc-46a0-bab3-1b4bf42699ad",
    "name": ""
  }
}
```
</details>

---
### Modify L7 Policy

```
PUT /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| l7policyId | URL | UUID | O | L7 Policy ID |
| l7policy | Body | Object | O | L7 Policy Object |
| l7policy.name | Body | String | - | L7 Policy Name |
| l7policy.description | Body | String | - | L7 Policy Description |
| l7policy.admin_state_up | Body | Boolean | - | Admin Control State of the L7 Policy |
| l7policy.action | Body | Enum | - | L7 Policy Action<br> One of `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT` |
| l7policy.redirect_pool_id | Body | UUID | - | Redirect pool ID of the L7 policy<br>Required if action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | - | Redirect URL of the L7 policy<br>Required if action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code | Body | Integer | - | Redirect HTTP response code of the L7 policy |
| l7policy.position | Body | Integer | - | Priority of the L7 policy |

<details><summary>Example</summary>

```json
{
  "l7policy": {
    "name": "L7Policy",
    "position": 255,
    "admin_state_up": true
  }
}
```
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| l7policy | Body | Object | L7 policy object |
| l7policy.description | Body | String | L7 policy description |
| l7policy.tenant_id | Body | String | Tenant ID |
| l7policy.listener_id | Body | UUID | L7 policy listener ID |
| l7policy.name | Body | String | L7 policy name |
| l7policy.rules | Body | Object | L7 policy rule object list |
| l7policy.rules.id | Body | UUID | L7 rule ID |
| l7policy.id | Body | UUID | L7 policy ID |
| l7policy.admin_state_up | Body | Boolean | L7 policy administrator control state |
| l7policy.action | Body | Enum | The action of the L7 policy<br> One of `REDIRECT_TO_POOL`, `REDIRECT_TO_URL`, or `REJECT` |
| l7policy.redirect_pool_id | Body | UUID | The redirect pool ID of the L7 policy<br> Only applies when the action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | The redirect URL of the L7 policy<br> Only applies when the action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code | Body | Integer | - | The redirect HTTP response code of the L7 policy |
| l7policy.position | Body | Integer | The priority of the L7 policy |


<details><summary>Example</summary>

```json
{
  "l7policy": {
    "redirect_pool_id": null,
    "description": "",
    "admin_state_up": true,
    "rules": [
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "listener_id": "2a38f448-c898-4694-9808-685dd6360dab",
    "redirect_url": null,
    "action": "REJECT",
    "position": 255,
    "id": "9376c901-64cc-46a0-bab3-1b4bf42699ad",
    "name": "L7Policy"
  }
}
```
</details>

---
### Delete L7 Policy

```
DELETE /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| l7policyId | URL | UUID | Yes | L7 Policy ID |

#### Response
This API does not return a response body.
















## L7 Rule

### View L7 Rule List

```
GET /v2.0/lbaas/l7policies/{l7policyId}/rules
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| l7policyId | URL | UUID | Yes | L7 policy ID to which the L7 rule belongs |
| id | Query | UUID | - | L7 rule ID to query |
| type | Query | Enum | - | Type of L7 rule to query <br> One of `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH` |
| compare_type | Query | Enum | - | Comparison method of L7 rule to query <br> One of `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| rules | Body | Array | L7 rule object list |
| rules.tenant_id | Body | String | Tenant ID |
| rules.id | Body | UUID | L7 rule ID |
| rules.admin_state_up | Body | Boolean | L7 rule administrator control state |
| rules.invert | Body | Boolean | Invert setting for matching results |
| rules.key | Body | String | Key used for L7 rule matching <br> Applies only to `COOKIE`/`HEADER` |
| rules.value | Body | String | Value used for L7 rule matching |
| rules.type | Query | Enum | L7 Rule Type <br> One of `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH` |
| rules.compare_type | Query | Enum | L7 Rule Comparison Method <br> One of `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX` |

<details><summary>Example</summary>

```json
{
  "rules": [
    {
      "compare_type": "EQUAL_TO",
      "admin_state_up": true,
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "invert": false,
      "value": "Value",
      "key": null,
      "type": "HOST_NAME",
      "id": "37492146-9105-40eb-9640-4da2e10c748a"
    }
  ]
}
```
</details>

---
### View L7 Rule

```
GET /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| l7policyId | URL | UUID | Yes | L7 Policy ID |
| l7ruleId | URL | UUID | Yes | L7 Rule ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| rule | Body | Object | L7 Rule Object |
| rule.tenant_id | Body | String | Tenant ID |
| rule.id | Body | UUID | L7 Rule ID |
| rule.admin_state_up | Body | Boolean | L7 Rule Admin Control State |
| rule.invert | Body | Boolean | Invert setting for matching results |
| rule.key | Body | String | Key used for L7 rule matching <br> Applies only to `COOKIE`/`HEADER` |
| rule.value | Body | String | Value used for L7 rule matching |
| rule.type | Query | Enum | L7 rule type <br> One of `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH` |
| rule.compare_type | Query | Enum | L7 rule comparison method <br> One of `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX` |

<details><summary>Example</summary>

```json
{
  "rule": {
    "compare_type": "EQUAL_TO",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "invert": false,
    "value": "Value",
    "key": null,
    "type": "HOST_NAME",
    "id": "37492146-9105-40eb-9640-4da2e10c748a"
  }
}
```
</details>

---
### Create L7 Rule

```
POST /v2.0/lbaas/l7policies/{l7policyId}/rules
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| l7policyId | URL | UUID | O | L7 Policy ID |
| rule | Body | Object | O | L7 Rule Object |
| rule.admin_state_up | Body | Boolean | - | L7 Rule Administrator Control State |
| rule.invert | Body | Boolean | - | Invert setting for matching results. If omitted, it is set to `true` |
| rule.key | Body | String | - | Key used for L7 rule matching <br> Required for `COOKIE`/`HEADER` |
| rule.value | Body | String | O | Value used for L7 rule matching |
| rule.type | Query | Enum | O | L7 Rule Type <br> One of `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH` |
| rule.compare_type | Query | Enum | O | L7 Rule Comparison Method <br> One of `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX` |


<details><summary>Example</summary>

```json
{
  "rule": {
    "compare_type": "STARTS_WITH",
    "invert": false,
    "type": "PATH",
    "value": "/images",
    "admin_state_up": true
  }
}
```
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| rule | Body | Object | L7 rule object |
| rule.tenant_id | Body | String | Tenant ID |
| rule.id | Body | UUID | L7 rule ID |
| rule.admin_state_up | Body | Boolean | L7 rule administrator control state |
| rule.invert | Body | Boolean | Invert setting for matching results |
| rule.key | Body | String | Key used for L7 rule matching <br> Applies only to `COOKIE`/`HEADER` |
| rule.value | Body | String | Value used for L7 rule matching |
| rule.type | Query | Enum | L7 Rule Type <br> One of `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH` |
| rule.compare_type | Query | Enum | L7 Rule Comparison Method <br> One of `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX` |


<details><summary>Example</summary>

```json
{
  "rule": {
    "compare_type": "STARTS_WITH",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "invert": false,
    "value": "/images",
    "key": null,
    "type": "PATH",
    "id": "3c88bc9b-8fac-4a73-a611-df85417b656e"
  }
}
```
</details>

---
### Modify L7 Rule

```
PUT /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| l7policyId | URL | UUID | O | L7 Policy ID |
| l7ruleId | URL | UUID | O | L7 Rule ID |
| rule | Body | Object | O | L7 Rule Object |
| rule.admin_state_up | Body | Boolean | - | L7 Rule Administrator Control State |
| rule.invert | Body | Boolean | - | Invert setting for matching results |
| rule.key | Body | String | - | Key used when matching L7 rules<br> Applies only to `COOKIE`/`HEADER` |
| rule.value | Body | String | - | Value used when matching L7 rules |
| rule.type | Query | Enum | - | L7 Rule Type <br> One of `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH` |
| rule.compare_type | Query | Enum | - | L7 Rule Comparison Method <br> One of `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX` |


<details><summary>Example</summary>

```json
{
  "rule": {
    "compare_type": "REGEX",
    "invert": true,
    "type": "PATH",
    "value": "/images/modify",
    "admin_state_up": true
  }
}
```
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| rule | Body | Object | L7 rule object |
| rule.tenant_id | Body | String | Tenant ID |
| rule.id | Body | UUID | L7 rule ID |
| rule.admin_state_up | Body | Boolean | L7 rule administrator control state |
| rule.invert | Body | Boolean | Invert setting for matching results |
| rule.key | Body | String | Key used for L7 rule matching <br> Applies only to `COOKIE`/`HEADER` |
| rule.value | Body | String | Value used for L7 rule matching |
| rule.type | Query | Enum | L7 Rule Type <br>One of `COOKIE`, `FILE_TYPE`, `HEADER`, `HOST_NAME`, or `PATH` |
| rule.compare_type | Query | Enum | L7 Rule Comparison Method <br>One of `CONTAINS`, `ENDS_WITH`, `STARTS_WITH`, `EQUAL_TO`, or `REGEX` |

<details><summary>Example</summary>

```json
{
  "rule": {
    "compare_type": "REGEX",
    "admin_state_up": true,
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "invert": true,
    "value": "/images/modify",
    "key": null,
    "type": "PATH",
    "id": "3c88bc9b-8fac-4a73-a611-df85417b656e"
  }
}
```
</details>

---
### Delete L7 Rule

```
DELETE /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| l7policyId | URL | UUID | Yes | L7 Policy ID |
| l7ruleId | URL | UUID | Yes | L7 Rule ID |

#### Response
This API does not return a response body.





















## Secret

The Secret API is called using the `key-manager` type endpoint. The exact endpoint can be found in the `serviceCatalog` field in the token issuance response.

| Type | Region | Endpoint |
|---|---|---|
| key-manager | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Korea (Gwangju) Region<br>Japan (Tokyo) Region<br>USA (California) Region |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr3-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-key-manager-infrastructure.nhncloudservice.com |

Fields not specified in the guide may be exposed in API responses. These fields are used internally by NHN Cloud and are subject to change without notice, so they are not used.

### View Secret List

Return a list of secrets.

```
GET /v1/secrets
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| offset | Query | Integer | - | Offset in the response list, default: 0 |
| limit | Query | Integer | - | Maximum number of responses to display, default: 10 |
| name | Query | String | - | Secret name |
| alg | Query | String | - | Secret algorithm |
| mode | Query | String | - | Block cipher operation method |
| bits | Query | Integer | - | Encryption key length |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| secrets | Body | Array | List of secret objects |
| secrets.secret_ref | Body | String | Secret address <br>`<barbican endpoint>/v1/secrets/<secret id>` format |
| secrets.secret_type | Body | Enum | Secret type <br> One of `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` |
| secrets.status | Body | Enum | Secret status |
| secrets.content_types | Body | Array | List of content types in the secret payload |
| secrets.content_types.default | Body | String | Default content type |
| secrets.creator_id | Body | String | User ID that created the secret |
| secrets.mode | Body | String | Block cipher operation method. User-supplied metadata |
| secrets.algorithm | Body | String | Encryption algorithm. User-supplied metadata |
| secrets.bit_length | Body | Integer | Encryption key length. User-supplied metadata |
| secrets.expiration | Body | Datetime | Expiration date. User-supplied metadata <br>`YYYY-MM-DDThh:mm:ss`<br> Secrets that have passed their expiration date are automatically deleted. |
| secrets.name | Body | String | Secret name |
| secrets.created | Body | Datetime | Creation time <br> `YYYY-MM-DDThh:mm:ss` |
| secrets.updated | Body | Datetime | Modification time <br> `YYYY-MM-DDThh:mm:ss` |
| total | Body | Integer | Total number of secrets in the request query |
| next | Body | String | URL for the next list in the currently viewed list |
| previous | Body | String | URL for the previous list in the currently viewed list |

<details><summary>Example</summary>
<p>

```json
{
  "secrets": [
    {
      "algorithm": null,
      "bit_length": null,
      "content_types": {
        "default": "text/plain"
      },
      "created": "2019-12-17T08:50:39",
      "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
      "expiration": null,
      "mode": null,
      "name": "certificate",
      "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
      "secret_type": "certificate",
      "status": "ACTIVE",
      "updated": "2019-12-17T08:50:39"
    },
    {
      "algorithm": null,
      "bit_length": null,
      "content_types": {
        "default": "text/plain"
      },
      "created": "2019-12-17T08:50:39",
      "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
      "expiration": null,
      "mode": null,
      "name": "private_key",
      "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
      "secret_type": "private",
      "status": "ACTIVE",
      "updated": "2019-12-17T08:50:39"
    }
  ],
  "total": 10,
  "next": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets?limit=1&offset=2",
  "previous": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets?limit=1&offset=0"
}

```

</p>
</details>


### View Secret
Returns information about the specified secret.
```
GET /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| secretId | URL | UUID | Yes | Secret ID |

#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| secret | Body | Object | Secret Object |
| secret.secret_ref | Body | String | Secret Address <br>`<barbican endpoint>/v1/secrets/<secret id>` Format |
| secret.secret_type | Body | Enum | Secret type <br> One of `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` |
| secret.status | Body | Enum | Secret status |
| secret.content_types | Body | Array | List of content types in the secret payload |
| secret.content_types.default | Body | String | Default content type |
| secret.creator_id | Body | String | User ID that created the secret |
| secret.mode | Body | String | Block cipher operation mode. User-supplied metadata |
| secret.algorithm | Body | String | Encryption algorithm. User-supplied metadata |
| secret.bit_length | Body | Integer | Encryption key length. User-supplied metadata |
| secret.expiration | Body | Datetime | Expiration date. User-entered metadata <br>`YYYY-MM-DDThh:mm:ss`<br> Secrets that have expired will be automatically deleted |
| secret.name| Body | String | Secret name |
| secret.created | Body | Datetime | Creation time <br> `YYYY-MM-DDThh:mm:ss` |
| secret.updated | Body | Datetime | Modification time <br> `YYYY-MM-DDThh:mm:ss` |

<details><summary>Example</summary>
<p>

```json
{
  "status": "ACTIVE",
  "secret_type": "certificate",
  "updated": "2019-12-17T08:50:39",
  "name": "certificate",
  "algorithm": null,
  "created": "2019-12-17T08:50:39",
  "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
  "content_types": {
    "default": "text/plain"
  },
  "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
  "mode": null,
  "bit_length": null,
  "expiration": null
}
```
</p>
</details>

---
### Create Secret
Create a new secret.
```
POST /v1/secrets
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| name | Body | String | - | Secret name |
| expiration | Body | Datetime | - | Expiration date. Request in ISO8601 format |
| algorithm | Body | String | - | Encryption algorithm |
| bit_length | Body | String | - | Encryption key length |
| mode | Body | String | - | Block cipher operation method |
| payload | Body | String | - | Encryption key payload |
| payload_content_type | Body | String | - | Encryption key payload content type <br> Required when entering a payload <br> List of supported content types: `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` |
| payload_content_encoding | Body | Enum | - | Encryption key payload encoding method <br>Required if payload_content_type is not text/plain<br> Only `base64` is supported |
| secret_type | Body | Enum | - | Secret type <br> One of `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` |

<details><summary>Example</summary>
Create metadata only
```json
{
    "name": "example key",
    "expiration": "2025-12-31T00:00:00.000000Z",
    "algorithm": "example-algorithm",
    "bit_length": 256,
    "mode": "example-mode"
}
```

Send payload as text
```json
{
    "name": "example key",
    "expiration": "2025-12-31T00:00:00.000000Z",
    "algorithm": "example-algorithm",
    "bit_length": 256,
    "mode": "example-mode",
	"payload": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANQE .... nyxm\n-----END PRIVATE KEY-----\n",
    "payload_content_type": "text/plain"
}
```

Send payload as base64
```json
{
    "name": "example key",
    "expiration": "2025-12-31T00:00:00.000000Z",
    "algorithm": "example-algorithm",
    "bit_length": 256,
    "mode": "example-mode",
    "payload": "ZXhhbXBsZQo=",
    "payload_content_type": "application/octet-stream",
    "payload_content_encoding": "base64"
}
```
</details>

#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| secret_ref | Body | String | Secret address<br>`<barbican endpoint>/v1/secrets/<secret id>` format |

<details><summary>Example</summary>
<p>

```json
{
    "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/9b2dcb7b-51fe-4408-a2bb-23da731758a6"
}
```
</p>
</details>

---
### Modify Secret
Enter the payload data for the secret for which only metadata was previously entered.
```
PUT /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
Content-Type: {ContentType}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| secretId | URL | UUID | Yes | Secret ID |
| ContentType | Header | Enum | Yes | One of `text/plain`, `application/octet-stream`, `application/pkcs8`, or `application/pkix-cert` <br> If omitted, `text/plain` is set |
| payload | Body | String | Yes | Encryption Key Payload |

<details><summary>Example</summary>
```
{
"payload": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANQE .... nyxm\n-----END PRIVATE KEY-----\n"
}
```
</details>

#### Response

This API does not return a response body.

---
### Delete Secret
Delete the specified secret.
```
DELETE /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| secretId | URL | UUID | O | Secret ID |

#### Response

This API does not return a response body.






































## Secret Container

The Secret Container API is called using the `key-manager` type endpoint. The exact endpoint can be found in the `serviceCatalog` field in the token issuance response.

| Type | Region | Endpoint |
|---|---|---|
| key-manager | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Korea (Gwangju) Region<br>Japan (Tokyo) Region<br>USA (California) Region |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr3-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-key-manager-infrastructure.nhncloudservice.com |

Fields not specified in the guide may be exposed in API responses. These fields are used internally by NHN Cloud and are subject to change without notice, so they are not used.

### View Secret Container List

Returns a list of secret containers.

```
GET /v1/containers
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| offset | Query | Integer | - | Offset in the response list, default: 0 |
| limit | Query | Integer | - | Maximum number of items to display in the response list, default: 10 |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| containers | Body | Array | List of container objects |
| containers.status | Body | Enum | Container status |
| containers.updated | Body | Datetime | Modification time `YYYY-MM-DDThh:mm:ss` |
| containers.name | Body | String | Container name |
| containers.consumers | Body | Array | Consumer list |
| containers.consumers.URL | Body | String | Consumer URL |
| containers.consumers.name | Body | String | Consumer name |
| containers.created | Body | Datetime | Creation time `YYYY-MM-DDThh:mm:ss` |
| containers.container_ref | Body | String | Container address |
| containers.creator_id | Body | String | User ID who created the container |
| containers.secret_refs | Body | Array | Secret list |
| containers.secret_refs.secret_ref | Body | String | Secret address |
| containers.secret_refs.name | Body | String | The secret name specified by the container.<br> If the container type is `certificate`: Specify `certificate`, `private_key`, `private_key_passphrase`, `intermediates`.<br> If the container type is `rsa`: Specify `private_key`, `private_key_passphrase`, `public_key`. |
| containers.type | Body | Enum | Container type.<br> One of `generic`, `rsa`, `certificate`. |
| containers.common_name | Body | String | The common name of the certificate registered in the container.<br> Only displayed if the container type is `certificate`. |
| containers.expiration | Body | Datetime | The expiration date of the certificate registered in the container.<br> Only displayed if the container type is `certificate`. Example: `YYYY-MM-DDThh:mm:ss`. |
| total | Body | Integer | Total number of secret containers in the request query |
| next | Body | String | Next list URL of the currently retrieved list |
| previous | Body | String | Previous list URL of the currently retrieved list |



<details><summary>Example</summary>
<p>

```json
{
  "total": 10,
  "previous": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers?limit=1&offset=0",
  "next": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers?limit=1&offset=2",
  "containers": [
    {
      "status": "ACTIVE",
      "updated": "2024-10-18T05:07:11",
      "name": "The Certificate",
      "consumers": [],
      "created": "2019-12-17T08:50:39",
      "container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/2d1dcf4d-2e92-475e-bde7-e469880be924",
      "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
      "secret_refs": [
        {
          "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
          "name": "certificate"
        },
        {
          "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
          "name": "private_key"
        }
      ],
      "type": "certificate",
      "common_name": "nhn.com.",
      "expiration": "2025-10-18T05:07:11"
    }
  ]
}


```
</p>
</details>


### View Secret Container
Return the information about the specified secret container.
```
GET /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| containerId | URL | UUID | Yes | Secret Container ID |

#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| status | Body | Enum | Container Status |
| updated | Body | Datetime | Modification Time `YYYY-MM-DDThh:mm:ss` |
| name | Body | String | Container Name |
| consumers | Body | Array | Consumer List |
| consumers.URL | Body | String | Consumer URL |
| consumers.name | Body | String | Consumer Name |
| created | Body | Datetime | Creation time `YYYY-MM-DDThh:mm:ss` |
| container_ref | Body | String | Container address |
| creator_id | Body | String | User ID who created the container |
| secret_refs | Body | Array | List of secrets registered in the container |
| secret_refs.secret_ref | Body | String | Secret address |
| secret_refs.name | Body | String | Secret name specified by the container<br>If the container type is `certificate`: Specify `certificate`, `private_key`, `private_key_passphrase`, `intermediates`<br>If the container type is `rsa`: Specify `private_key`, `private_key_passphrase`, `public_key` |
| type | Body | Enum | Container type<br> One of `generic`, `rsa`, `certificate` |
| common_name | Body | String | The Common Name of the certificate registered in the container. <br>Only displayed when the container type is `certificate`. |
| expiration | Body | Datetime | The expiration date of the certificate registered in the container. <br>Only displayed when the container type is `certificate`. Example: `YYYY-MM-DDThh:mm:ss`. |

<details><summary>Example</summary>

```json
{
    "status": "ACTIVE",
    "updated": "2024-10-18T05:07:11",
    "name": "The Certificate",
    "consumers": [],
    "created": "2019-12-17T08:50:39",
    "container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/2d1dcf4d-2e92-475e-bde7-e469880be924",
    "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
    "secret_refs": [
        {
            "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
            "name": "private_key"
        },
        {
            "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
            "name": "certificate"
        }
    ],
    "type": "certificate",
    "common_name": "nhn.com.",
    "expiration": "2025-10-18T05:07:11"
}
```
</details>

---
### Create Secret Container
Create a new secret container.
```
POST /v1/containers
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | Yes | Token ID |
| type | Body | Enum | Yes | Container type <br> One of `generic`, `rsa`, or `certificate` |
| name | Body | String | - | Container name |
| secret_refs | Body | Array | - | List of secrets to register in the container |
| secret_refs.secret_ref | Body | String | - | Secret address |
| secret_refs.name | Body | String | - | Secret name specified by the container<br> If the container type is `certificate`: Specify `certificate`, `private_key`, `private_key_passphrase`, `intermediates`<br> If the container type is `rsa`: Specify `private_key`, `private_key_passphrase`, `public_key` |


<details><summary>Example</summary>
<p>

```json
{
    "type": "certificate",
    "name": "test cert",
    "secret_refs": [
        {
            "name": "private_key",
            "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/cf11edcf-f475-47f3-92c3-29de8bcdd639"
        }
    ]
}
```
</p>
</details>

#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| container_ref | Body | String | Secret container address |

<details><summary>Example</summary>
<p>

```json
{
    "container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/ea2e90fc-1ba2-412b-b7a0-61da4402bf58"
}
```
</p>
</details>

---
### Delete Secret Container
Deletes the specified secret container.
```
DELETE /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| containerId | URL | UUID | Secret Container ID |

#### Response

This API does not return a response body.



































## IP ACL Group

### View IP ACL Group List

Return a IP ACL group list.

```
GET /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| id | Query | String | - | IP ACL group ID |
| name | Query | String | - | IP ACL group name |
| description | Query | String | - | IP ACL group description |
| action | Query | Enum | - | Control action for the IP ACL group <br>One of `ALLOW` or `DENY` |

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| ipacl_groups | Body | Array | List of IP ACL group objects |
| ipacl_groups.ipacl_target_count | Body | String | Number of targets contained in the IP ACL group |
| ipacl_groups.description | Body | String | IP ACL group description |
| ipacl_groups.loadbalancers | Body | Object | List of load balancer objects to which the IP ACL group is applied |
| ipacl_groups.loadbalancers.loadbalancer_id | Body | String | Load balancer ID |
| ipacl_groups.tenant_id | Body | String | Tenant ID |
| ipacl_groups.action | Body | Enum | Control action of the IP access control group <br>One of `ALLOW` or `DENY` |
| ipacl_groups.id | Body | UUID | IP ACL group ID |
| ipacl_groups.name | Body | String | IP ACL group name |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_groups": [
      {
      "ipacl_target_count": "1",
      "description": "",
      "loadbalancers": [
        {
          "loadbalancer_id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
        }
      ],
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "action": "DENY",
      "id": "04570ec5-456a-48ac-85ee-38adcc83ee70",
      "name": "ip-acl-group-1"
    }
  ]
}
```
</p>
</details>

### View IP ACL Group

Return a specified IP ACL.

```
GET /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- |
| tokenId | Header | String | Yes | Token ID |
| ipaclGroupId | Header | String | Yes | Token ID |

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACL group object |
| ipacl_group.ipacl_target_count | Body | String | Number of targets included in the IP ACL group |
| ipacl_group.description | Body | String | IP ACL group description |
| ipacl_group.loadbalancers | Body | Object | List of load balancer objects to which the IP ACL group is applied |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | Load Balancer ID |
| ipacl_group.tenant_id | Body | String | Tenant ID |
| ipacl_group.action | Body | Enum | Control action for the IP ACL group <br>One of `ALLOW` or `DENY` |
| ipacl_group.id | Body | UUID | IP ACL group ID |
| ipacl_group.name | Body | String | IP ACL group name |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_group": {
    "ipacl_target_count": "1",
    "description": "",
    "loadbalancers": [
      {
        "loadbalancer_id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "action": "DENY",
    "id": "04570ec5-456a-48ac-85ee-38adcc83ee70",
    "name": "ip-acl-group-1"
  }
}
```
</p>
</details>

- - -

### Create IP ACL Group

Create a new IP ACL group.

```
POST /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| ipacl_group | Body | Object | O | IP ACL group object |
| ipacl_group.description | Body | String | - | IP ACL group description |
| ipacl_group.action | Body | Enum | O | IP ACL group control action <br>One of `ALLOW` or `DENY` |
| ipacl_group.name | Body | String | - | IP ACL group name |
| ipacl_group.ipacl_targets | Body | Object | - | IP ACL target object. When a value is entered, the target is also created |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (if an ipacl_targets object is added) | IP ACL Target CIDR<br>Enter a single IP address or an IP range in CIDR format |
| ipacl_group.ipacl_targets.descripion | Body | String | - | IP ACL Target Description |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_group": {
    "action": "ALLOW",
    "name": "example",
    "description": "description",
    "ipacl_targets": [
			{
				"cidr_address" : "192.168.0.5",
				"description": "My Friend"
			},
			{
				"cidr_address" : "10.10.22.3/24",
				"description": "Your Friends"
			}
     ]
  }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACL group object |
| ipacl_group.ipacl_target_count | Body | String | Number of targets included in the IP ACL group |
| ipacl_group.description | Body | String | IP ACL group description |
| ipacl_group.loadbalancers | Body | String | List of load balancer objects to which the IP ACL group is applied |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | Load balancer ID |
| ipacl_group.tenant_id | Body | String | Tenant ID |
| ipacl_group.action | Body | Enum | Control action of the IP ACL group<br>One of `ALLOW` and `DENY` |
| ipacl_group.id | Body | UUID | IP ACL Group ID |
| ipacl_group.name | Body | String | IP ACL Group Name |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_group": {
    "ipacl_target_count": "0",
    "description": "description",
    "loadbalancers": [],
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "action": "ALLOW",
    "id": "e5e2627e-c1fc-4deb-a96d-f1213bb8227e",
    "name": "example"
  }
}
```
</p>
</details>

- - -

### Modify IP ACL Group

Modify an existing IP ACL group.
ipacl_group.action cannot be changed.
This API can be used to completely replace the list of sub-IP ACL targets.
However, all existing targets belonging to the IP ACL group will be deleted and replaced with the specified target list.

The cidr_address of the specified targets must be unique.

```
PUT /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | Yes | Token ID |
| ipaclGroupId | URL | UUID | Yes | IP ACL Group ID |
| ipacl_group | Body | String | Yes | IP ACL Group Object |
| ipacl_group.name | Body | String | - | IP ACL group name |
| ipacl_group.description | Body | String | - | IP ACL group description |
| ipacl_group.ipacl_targets | Body | Object | - | IP ACL target object. When a value is entered, the target is also created |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (if an ipacl_targets object is added) | IP ACL target CIDR<br>Enter a single IP address or an IP range in CIDR format |
| ipacl_group.ipacl_targets.descripion | Body | String | - | IP ACL target description |


<details><summary>Example</summary>
<p>

``` json
{
    "ipacl_group" : {
    "name" : "HouseLannister",
    "description" : "A Lannister always pays his debts",
    "ipacl_targets" : [
        {
            "cidr_address" : "11.11.11.11",
            "description" : "Jamie"
        },
        {
            "cidr_address" : "22.22.22.22",
            "description" : "Cercei"
        },
        {
            "cidr_address" : "33.33.33.33",
            "description" : "Tyrion"
        }
    ]
    }
}
```
</p>
</details>

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACL group object |
| ipacl_group.ipacl_target_count | Body | String | Number of targets included in the IP ACL group |
| ipacl_group.description | Body | String | IP ACL group description |
| ipacl_group.loadbalancers | Body | String | List of load balancer objects to which the IP ACL group is applied |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | Load balancer ID |
| ipacl_group.tenant_id | Body | String | Tenant ID |
| ipacl_group.action | Body | Enum | Control action of the IP ACL group<br>One of `ALLOW` and `DENY` |
| ipacl_group.id | Body | UUID | IP ACL Group ID |
| ipacl_group.name | Body | String | IP ACL Group Name |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_group": {
    "ipacl_target_count": "3",
    "description": "A Lannister always pays his debts",
    "loadbalancers": [],
    "tenant_id": "18717b5d8a9d45b9af440c75d61235c7",
    "action": "DENY",
    "id": "acc655d4-4735-4892-b32b-669cc21925ff",
    "name": "HouseLannister"
  }
}
```
</p>
</details>

- - -

### Delete IP ACL Group

Delete a specified IP ACL group.

```
DELETE /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

When deleting an IP ACL group, all IP ACL targets below it are also deleted.

Rules related to this IP ACL group will be deleted from all load balancers using the deleted IP ACL group.

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| ipaclGroupId | URL | UUID | O | IP ACL Group ID |

#### Response

This API does not return a response body.

- - -

### Apply IP ACL Group to Load Balancer

Apply an IP ACL group to a load balancer.
The IP ACL target rules included in the group will be applied to load balancers to which the IP ACL group is applied.
Multiple groups can be applied to a load balancer. However, the actions for all groups must be the same.
All IP ACL groups previously applied to the load balancer will be deleted and reapplied to the entered group list.

```
PUT /v2.0/lbaas/loadbalancers/{lb_id}/bind_ipacl_groups
X-auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| lb_id | URL | UUID | O | Load Balancer ID |
| ipacl_groups_binding | Body | Object | O | IP ACL Binding Object |
| ipacl_groups_binding.ipacl_group_id | Body | UUID | O | IP ACL group ID to apply to the load balancer |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_groups_binding": [
    {
      "ipacl_group_id": "acc655d4-4735-4892-b32b-669cc21925ff"
    },
    {
      "ipacl_group_id": "ef33c087-2dc9-4be6-a0d2-d24c9d84e66e"
    }
  ]
}
```

</p>
</details>

#### Response
| Name | Type | Format | Description |
| --- | --- | --- | --- |
| loadbalancer_id | Body | UUID | Load Balancer ID |
| ipacl_group_id | Body | UUID | IP ACL Group ID |

<details><summary>Example</summary>
<p>

``` json
[
  {
    "loadbalancer_id": "096ddfbf-aaf9-42d6-b93d-0036ec219479",
    "ipacl_group_id": "acc655d4-4735-4892-b32b-669cc21925ff"
  },
  {
    "loadbalancer_id": "096ddfbf-aaf9-42d6-b93d-0036ec219479",
    "ipacl_group_id": "ef33c087-2dc9-4be6-a0d2-d24c9d84e66e"
  }
]
```

</p>
</details>







































## IP ACL Target

### View IP ACL Target List

Return a IP ACL target list.

```
GET /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | Yes | Token ID |
| id | Query | String | - | IP ACL Target ID |
| cidr_address | Query | String | - | IP ACL Target CIDR <br>A single IP address or IP range in CIDR format |
| ipacl_group_id | Query | String | - | IP ACL Group ID |
| description | Query | String | - | IP ACL Group Description |

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| ipacl_targets | Body | Array | List of IP ACL Target Information Objects |
| ipacl_targets.ipacl_group_id | Body | UUID | IP ACL Group ID |
| ipacl_targets.tenant_id | Body | String | Tenant ID |
| ipacl_targets.cidr_address | Body | String | IP ACL Target CIDR |
| ipacl_targets.description | Body | String | IP ACL Target Description |
| ipacl_targets.id | Body | UUID | IP ACL Target ID |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_targets": [
    {
      "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "cidr_address": "10.0.0.0/24",
      "description": "description",
      "id": "08d06560-919d-4383-a491-70fd2aca3fb2"
    }
  ]
}
```

</p>
</details>

### View IP ACL Target

Return a specified IP ACL target information.

```
GET /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- |
| tokenId | Header | String | Yes | Token ID |
| ipaclTargetId | URL | UUID | Yes | IP ACL Target ID |

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| ipacl_target | Body | Array | IP ACL Target Information Object |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL Group ID |
| ipacl_target.tenant_id | Body | String | Tenant ID |
| ipacl_target.cidr_address | Body | String | IP ACL Target CIDR<br>A single IP address or an IP range in CIDR format |
| ipacl_target.description | Body | String | IP ACL target description |
| ipacl_target.id | Body | UUID | IP ACL target ID |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "cidr_address": "10.0.0.0/24",
    "description": "description",
    "id": "08d06560-919d-4383-a491-70fd2aca3fb2"
  }
}
```

</p>
</details>

- - -

### Create IP ACL Target

Create a IP ACL target.

```
POST /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| ipacl_target | Body | Object | O | IP ACL Target Information Object |
| ipacl_target.ipacl_group_id | Body | UUID | O | IP ACL Group ID |
| ipacl_target.cidr_address | Body | String | O | IP ACL Target CIDR<br>A single IP address or an IP range in CIDR format |
| ipacl_target.description | Body | String | - | IP ACL Target Description |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "cidr_address": "10.0.0.0/24",
    "description": "description"
  }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| ipacl_target | Body | Object | IP ACL target information object |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL group ID |
| ipacl_target.tenant_id | Body | String | Tenant ID |
| ipacl_target.cidr_address | Body | String | IP ACL target CIDR<br>A single IP address or an IP range in CIDR format |
| ipacl_target.description | Body | String | IP ACL target description |
| ipacl_target.id | Body | UUID | IP ACL target ID |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "cidr_address": "10.0.0.0/24",
    "description": "description",
    "id": "08d06560-919d-4383-a491-70fd2aca3fb2"
  }
}
```

</p>
</details>

- - -

### Modify IP ACL Target

Change an existing IP ACL target.
Only the description can be changed.

```
PUT /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| ipaclTargetId | URL | UUID | O | IP ACL Target ID |
| ipacl_target | Body | Object | O | IP ACL Target Information Object |
| ipacl_target.description | Body | String | - | IP ACL Target Description |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_target": {
    "description": "description"
  }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| ipacl_target | Body | Object | IP ACL Target Information Object |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL Group ID |
| ipacl_target.tenant_id | Body | String | Tenant ID |
| ipacl_target.cidr_address | Body | String | IP ACL Target CIDR<br>A single IP address or IP RANGE in CIDR format |
| ipacl_target.description | Body | String | IP ACL Target Description |
| ipacl_target.id | Body | UUID | IP ACL Target ID |

<details><summary>Example</summary>
<p>

``` json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "cidr_address": "10.0.0.0/24",
    "description": "description",
    "id": "08d06560-919d-4383-a491-70fd2aca3fb2"
  }
}
```

</p>
</details>

- - -

### Delete IP ACL Target

Delete a specified load balancer.

```
DELETE /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | Yes | Token ID |
| ipaclTargetId | URL | UUID | Yes | IP ACL Target ID |

#### Response

This API does not return a response body.

- - -

