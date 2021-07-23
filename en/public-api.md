## Network > Load Balancer > API v2 Guide 

 To enable APIs, API endpoint and token are required. Preparer information required to enable an API, in reference of  [Preparing for APIs](/Compute/Compute/ko/identity-api/)

Use `network`-type endpoint for Load Balancer, Listener, Pool, Health Monitor, or Member API. For Secret, or Secret Container API, call by using the `key-manager`-type endpoint.  For more details, see `serviceCatalog` from response of token issuance. 

| Type | Region | Endpoint |
|---|---|---|
| network | Korea (Pangyo)<br>Japan | https://kr1-api-network.infrastructure.cloud.toast.com<br>https://jp1-api-network.infrastructure.cloud.toast.com |
| key-manager | Korea (Pangyo)<br>Japan | https://kr1-api-key-manager.infrastructure.cloud.toast.com<br>https://jp1-api-key-manager.infrastructure.cloud.toast.com |


In API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the NHN Cloud internal usage and might be changed without previous notice. 

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
| name | Query | String | - | Land balancer name to query |
| provisioning_status | Query | Enum | - | Provisioning status of load balancer to query |
| description | Query | String | - | Description of load balancer to query |
| vip_address | Query | String | - | IP of load balancer to query |
| vip_port_id | Query | UUID | - | Port ID of load balancer to query |
| vip_subnet_id | Query | UUID | - | Subnet ID of load balancer to query |
| operating_status | Query | Enum | - | Operating status of load balancer to query |
| loadbalancer_type | Query | String | - | The type of load balancer to view<br>Should be either `shared` or `dedicated` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| loadbalancers | Body | Array | List of load balancer information objects |
| loadbalancers.description | Body | String | Load balancer description |
| loadbalancers.provisioning_status | Body | Enum | Provisioning status of load balancer |
| loadbalancers.tenant_id | Body | String | Tenant ID |
| loadbalancers.provider | Body | String | Provider of load balancer |
| loadbalancers.name | Body | String | Name of load balancer |
| loadbalancers.listeners | Body | Object | List of load balancer listener objects |
| loadbalancers.listeners.id | Body | UUID | Listener ID |
| loadbalancers.vip_address | Body | String | Load balancer IP |
| loadbalancers.vip_port_id | Body | UUID | Port ID of load balancer |
| loadbalancers.vip_subnet_id | Body | UUID | Subnet ID of load balancer |
| loadbalancers.id | Body | UUID | Load balancer ID |
| loadbalancers.operating_status | Body | Enum | Operating status of load balancer |
| loadbalancers.admin_state_up | Body | Boolean | Administrator control status of load balancer |
| loadbalancers.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancers.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancers.ipacl_action | Body | UUID | The action of IP ACL groups applied to the load balancer <br>Should be one of the following: `null`, `DENY`, or `ALLOW`  |
| loadbalancers.loadbalancer_type | Body | String | Load Balancer Type<br>` either shared` or `dedicated` |

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
      "vip_address": "192.168.0.187",
      "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
      "workflow_status": "SUCCESS",
      "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
      "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
      "operating_status": "ONLINE",
      "admin_state_up": true
    }
  ]
}
```
</details>



### Get Load Balancer 

```
GET /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| loadbalancerId | URL | UUID | O | Load balancer ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| loadbalancer | Body | Object | Information object of load balancer |
| loadbalancer.description | Body | String | Description of load balancer |
| loadbalancer.provisioning_status | Body | Enum | Provisioning status of load balancer |
| loadbalancer.tenant_id | Body | String | Tenant ID |
| loadbalancer.provider | Body | String | Provider of load balancer |
| loadbalancer.name | Body | String | Name of load balancer |
| loadbalancer.listeners | Body | Object | List of load balancer listener objects |
| loadbalancer.listeners.id | Body | UUID | Listener ID |
| loadbalancer.vip_address | Body | String | Load balancer IP |
| loadbalancer.vip_port_id | Body | UUID | Port ID of load balancer |
| loadbalancer.vip_subnet_id | Body | UUID | Subnet ID of load balancer |
| loadbalancer.id | Body | UUID | Load balancer ID |
| loadbalancer.operating_status | Body | Enum | Operating status of load balancer |
| loadbalancer.admin_state_up | Body | Boolean | Administrator control center of load balancer |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_action | Body | UUID | The action of IP ACL groups applied to the load balancer <br>Should be one of the following: `null`, `DENY`, or `ALLOW`  |
| loadbalancer.loadbalancer_type | Body | String | 로드 밸런서 타입<br>`shared` / `dedicated` 중 하나 |

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
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true
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
| loadbalancer | Body | Object | - | Information object of load balancer |
| loadbalancer.name | Body | String | - | Name of load balancer |
| loadbalancer.description | Body | String | - | Description of load balancer |
| loadbalancer.vip_subnet_id | Body | UUID | O | Subnet ID of load balancer |
| loadbalancer.vip_address | Body | String | - | IP of load balancer |
| loadbalancer.admin_state_up | Body | Boolean | - | Administrator control status of load balancer: if left blank, `true` is set |
| loadbalancer.loadbalancer_type | Body | String | - | It is a load balancer type, which can be used as `shared` / `dedicated`<br> and set as `shared` if omitted |



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
| loadbalancer | Body | Object | Information object of load balancer |
| loadbalancer.description | Body | String | Description of load balancer |
| loadbalancer.provisioning_status | Body | Enum | Provisioning status of load balancer |
| loadbalancer.tenant_id | Body | String | Tenant ID |
| loadbalancer.provider | Body | String | Provider name of load balancer |
| loadbalancer.name | Body | String | Name of load balancer |
| loadbalancer.listeners | Body | Object | List of load balancer listener objects |
| loadbalancer.listeners.id | Body | UUID | Listener ID |
| loadbalancer.vip_address | Body | String | IP of load balancer |
| loadbalancer.vip_port_id | Body | UUID | Port ID of load balancer |
| loadbalancer.vip_subnet_id | Body | UUID | Subnet ID of load balancer |
| loadbalancer.id | Body | UUID | Load balancer ID |
| loadbalancer.operating_status | Body | Enum | Operating status of load balancer |
| loadbalancer.admin_state_up | Body | Boolean | Administrator control status of load balancer |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_action | Body | UUID | The action of IP ACL groups applied to the load balancer <br>Should be one of the following: `null`, `DENY`, or `ALLOW`  |
| loadbalancer.loadbalancer_type | Body | String | Load Balancer Type<br>` either shared` or `dedicated` |

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
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true
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
| loadbalancerId | URL | UUID | O | Load balancer ID |
| loadbalancer | Body | Object | O | Information object of load balancer |
| loadbalancer.name | Body | String | - | Name of load balancer |
| loadbalancer.description | Body | String | - | Description of load balancer |
| loadbalancer.admin_state_up | Body | Boolean | - | Administrator control status of load balancer |

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

| Name | Type | Fomat | Description |
|---|---|---|---|
| loadbalancer | Body | Object | Information object of load balancer |
| loadbalancer.description | Body | String | Description of load balancer |
| loadbalancer.provisioning_status | Body | Enum | Provisioning status of load balancer |
| loadbalancer.tenant_id | Body | String | Tenant ID |
| loadbalancer.provider | Body | String | Name of load balancer provider |
| loadbalancer.name | Body | String | Name of load balancer |
| loadbalancer.listeners | Body | Object | List of load balancer listener objects |
| loadbalancer.listeners.id | Body | UUID | Listener ID |
| loadbalancer.vip_address | Body | String | Load balancer IP |
| loadbalancer.vip_port_id | Body | UUID | Port ID of load balancer |
| loadbalancer.vip_subnet_id | Body | UUID | Subnet ID of load balancer |
| loadbalancer.id | Body | UUID | Load balancer ID |
| loadbalancer.operating_status | Body | Enum | Operating status of load balancer |
| loadbalancer.admin_state_up | Body | Boolean | Administrator control status of load balancer |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_action | Body | UUID | The action of IP ACL groups applied to the load balancer <br>Should be one of the following: `null`, `DENY`, or `ALLOW`  |
| loadbalancer.loadbalancer_type | Body | String | Load Balancer Type<br>` either shared` or `dedicated` |

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
    "vip_address": "192.168.0.187",
    "vip_port_id": "f3764f0d-b0da-4be1-a61f-fc5e8914278a",
    "workflow_status": "SUCCESS",
    "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
    "id": "7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "operating_status": "ONLINE",
    "admin_state_up": true
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
| tokenId | Header | String | O | Token ID |
| loadbalancerId | URL | UUID | O | Load balancer ID |


#### Response
This API does not return response body. 





















## Listener
### List Listeners 

```
GET /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| default_pool_id | Query | UUID | - | Pool ID registered at listener |
| protocol | Query | Enum | - | Protocol of listener <br>One of `TCP`, `HTTP`,`HTTPS`, and `TERMINATED_HTTPS` |
| description | Query | String | - | Description of listener |
| name | Query | String | - | Name of listener |
| admin_state_up | Query | Boolean | - | Administrator control status |
| connection_limit | Query | Integer | - | Connection limit of listener |
| keepalive_timeout | Query | Integer | - | Keepalive timeout of listener |
| protocol_port | Query | Integer | - | Port number of listener |
| id | Query | UUID | - | Listener ID |


#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| listeners | Body | Array | List of listener information objects |
| listeners.default_pool_id | Body | UUID | Pool ID registered at listener |
| listeners.protocol | Body | Enum | Protocol of listener <br>One of`TCP`, `HTTP`,`HTTPS`, and `TERMINATED_HTTPS` |
| listeners.description | Body | String | Description of listener |
| listeners.name | Body | String | Name of listener |
| listeners.loadbalancers | Body | Array | List of load balancer objects in which listener is registered |
| listeners.loadbalancers.id | Body | UUID | ID of load balancer |
| listeners.tenant_id | Body | String | Tenant ID |
| listeners.admin_state_up | Body | Boolean | Administrator control status |
| listeners.connection_limit | Body | Integer | Connection limit of listener |
| listeners.keepalive_timeout | Body | Integer | Keepalive timeout of listener |
| listeners.default_tls_container_ref | Body | String| Path of TLS certificate registered at key-manager |
| listeners.sni_container_refs | Body | Array | List of SNI certificate paths registered at key-manager |
| listeners.protocol_port | Body | Integer | Port of listener |
| listeners.id | Body | String| ID of listener |

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
      "default_tls_container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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


### Get Listener

```
GET /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| listenerId | URL | UUID | O | Listener ID |


#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| listener | Body | Object | Information object of listener |
| listener.default_pool_id | Body | UUID | Pool ID registered at listener |
| listener.protocol | Body | Enum | Protocol of listener <br>One of `TCP`, `HTTP`,`HTTPS`, and `TERMINATED_HTTPS` |
| listener.description | Body | String | Description of listener |
| listener.name | Body | String | Name of listener |
| listener.loadbalancers | Body | Array | List of load balancer objects in which listener is registered |
| listener.loadbalancers.id | Body | UUID | Load balancer ID |
| listener.tenant_id | Body | String | Tenant ID |
| listener.admin_state_up | Body | Boolean | Administrator control status |
| listener.connection_limit | Body | Integer | Connection limit of listener |
| listener.keepalive_timeout | Body | Integer | Keepalive timeout of listener |
| listener.default_tls_container_ref | Body | String| Path of TLS certificate registered at key-manager |
| listener.sni_container_refs | Body | Array | List of SNI certificate paths registered at key-manager |
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
    "default_tls_container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
| listener | Body | Object | O | Information object of listener |
| listener.protocol | Body | Enum | O | Listener protocol <br>One of `TCP`, `HTTP`,`HTTPS`, and `TERMINATED_HTTPS` |
| listener.description | Body | String | - | Description of listener |
| listener.name | Body | String | - | Name of listener |
| listener.loadbalancer_id | Body | UUID | O | ID of load balancer |
| listener.admin_state_up | Body | Boolean | - | Administrator control status |
| listener.connection_limit | Body |  Integer | - | Connection limit of listener |
| listener.keepalive_timeout | Body | Integer | - | Keepalive timeout of listener |
| listener.default_tls_container_ref | Body | String | - | Path of TLS certificate registered at key-manager |
| listener.sni_container_refs | Body | Array | - | List of SNI certificate paths registered at key-manager |
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
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "default_tls_container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
| listener | Body | Object | Information object of listener |
| listener.default_pool_id | Body | UUID | Pool ID registered at listener |
| listener.protocol | Body | Enum | Protocol of listener <br>One of `TCP`, `HTTP`,`HTTPS`,  and `TERMINATED_HTTPS` |
| listener.description | Body | String | Description of listener |
| listener.name | Body | String | Name of listener |
| listener.loadbalancers | Body | Array | List of load balancer objects in which listener is registered |
| listener.loadbalancers.id | Body | UUID | ID of load balancer |
| listener.tenant_id | Body | String | Tenant ID |
| listener.admin_state_up | Body | Boolean | Administrator control status |
| listener.connection_limit | Body | Integer | Connection limit of listener |
| listener.keepalive_timeout | Body | Integer | Keepalive timeout of listener |
| listener.default_tls_container_ref | Body | String | Path of TLS certificate registered at key-manager |
| listener.sni_container_refs | Body | Array | List of SNI certificate paths registered at key-manager |
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
    "default_tls_container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
| listener | Body | Object | O | Information object of listener |
| listener.description | Body | String | - | Listener description |
| listener.name | Body | String| - | Listener name |
| listener.admin_state_up | Body | Boolean | - | Administrator control status |
| listener.connection_limit | Body |  Integer | - | Connection limit of listener |
| listener.keepalive_timeout | Body | Integer | - | Keepalive timeout of listener |
| listener.default_tls_container_ref | Body | String | - | Path of TLS certificate registered at key-manager |
| listener.sni_container_refs | Body | Array | - | List of SNI certificate paths registered at key-manager |

<details><summary>Example</summary>
<p>


```json
{
  "listener": {
    "proxy_protocol": false,
    "description": "",
    "name": "",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "default_tls_container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
| listener.default_pool_id | Body | UUID | Pool ID registered at listener |
| listener.protocol | Body | Enum | Listener protocol<br>One of`TCP`, `HTTP`,`HTTPS`, and `TERMINATED_HTTPS` |
| listener.description | Body | String | Listener description |
| listener.name | Body | String | Listener name |
| listener.loadbalancers | Body | Array | List of load balancer objects in which listener is registered |
| listener.loadbalancers.id | Body | UUID | Load balancer ID |
| listener.tenant_id | Body | String | Tenant ID |
| listener.admin_state_up | Body | Boolean | Administrator control status |
| listener.connection_limit | Body | Integer | Connection limit of listener |
| listener.keepalive_timeout | Body | Integer | Keepalive timeout of listener |
| listener.default_tls_container_ref | Body | String | Path of TLS certificate registered at key-manager |
| listener.sni_container_refs | Body | Array | List of SNI certificate paths registered at key-manager |
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
    "default_tls_container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
Delete specified listener. 
```
DELETE /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| listenerId | URL | UUID | O | Listener ID |

#### Response

This API does not return a response body. 













## Pool
### List Pools

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
| lb_algorithm | Query | Enum | - | Load balancing method of the pool <br>One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`,  and `SOURCE_IP` |
| protocol | Query | Enum | - | Member protocol |
| admin_state_up | Query | Boolean | - | Administrator control status |
| healthmonitor_id | Query | UUID | - | Health monitor ID of pool |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| pools | Body | Array | List of pool information objects |
| pools.lb_algorithm | Body | Enum | Load balancing method of the pool <br>One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, and `SOURCE_IP` |
| pools.protocol | Body | Enum | Member protocol |
| pools.description | Body | String | Pool description |
| pools.admin_state_up | Body | Boolean | Administrator control status |
| pools.tenant_id | Body | String | Tenant ID |
| pools.session_persistence | Body | Object | Session persistence object of pool |
| pools.session_persistence.type | Body | Enum | Session persistence<br>Set one of `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`.<br>Even if the listener protocol is set with  `TCP` or `HTTPS`, there's no load balancer operation related with session persistence. |
| pools.session_persistence.cookie_name | Body | String | Cookie name <br>Set value is applied when the session persistence type is `APP_COOKIE` |
| pools.healthmonitor_id | Body | String | Health monitor ID |
| pools.listeners | Body | Array | List of listener objects in which pool is registered |
| pools.listeners.id | Body | String | Listener ID |
| pools.members | Body | Array | List of member objects registered at pool |
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


### Get Pool

```
GET /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| pool | Body | Object | Pool information object |
| pool.lb_algorithm | Body | Enum | Load balancing method of the pool <br>One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, and `SOURCE_IP` |
| pool.protocol | Body | Enum | Member protocol |
| pool.description | Body | String | Pool description |
| pool.admin_state_up | Body | Boolean | Administrator control status |
| pool.tenant_id | Body | String | Tenant ID |
| pool.member_port | Body | Integer | Member port<br>Port value of the member specified when created on web console |
| pool.session_persistence | Body | Object | Session persistence object of pool |
| pool.session_persistence.type | Body | Enum | Session persistence<br>Set one of`SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`.<br/>Even if the listener protocol is set with  `TCP` or `HTTPS`, and if session persistence is set with `HTTP_COOKIE`, `APP_COOKIE`, there's no load balancer operation related with session persistence. |
| pool.session_persistence.cookie_name | Body | String | Cookie name <br>Set value is applied only when the session persistence type is `APP_COOKIE` |
| pool.healthmonitor_id | Body | UUID | Health monitor ID |
| pool.listeners | Body | Array | List of listener objects registered at the pool |
| pool.listeners.id | Body | UUID | Listener ID |
| pool.members | Body | Array | List of member objects registered at the pool |
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
| pool.listener_id | Body | UUID | O | Listener ID to register a pool |
| pool.lb_algorithm | Body | Enum | O | Load balancing method of the pool <br>One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, and `SOURCE_IP` |
| pool.protocol | Body | Enum | O | Member protocol |
| pool.description | Body | String | - | Pool description |
| pool.admin_state_up | Body | Boolean | - | Administrator control status |
| pool.member_port | Body | Integer | - | Member's receiving port<br>Traffic is sent to the port.<br>Default is -1. |
| pool.session_persistence | Body | Object | - | Session persistence object of pool |
| pool.session_persistence.type | Body | Enum | - | Session consistency<br>Set one of `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`.<br/>Even if the listener protocol is set with  `TCP` or `HTTPS`, and if session persistence is set with `HTTP_COOKIE`, `APP_COOKIE`, there's no load balancer operation related with session persistence. |
| pools.session_persistence.cookie_name | Body | String | - | Cookie name <br/>Set value is applied only when the session persistence type is `APP_COOKIE` |
| pool.name | Body | String | - | Pool name |



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
| pool.lb_algorithm | Body | Enum | Load balancing method of the pool <br>One of  `ROUND_ROBIN`, `LEAST_CONNECTIONS`, and`SOURCE_IP` |
| pool.protocol | Body | Enum | Member protocol |
| pool.description | Body | String | Pool description |
| pool.admin_state_up | Body | Boolean | Administrator control status |
| pool.tenant_id | Body | String | Tenant ID |
| pool.session_persistence | Body | Object | - |
| pool.session_persistence.type | Body | Enum | Session persistence<br>One of `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`.<br/>Even if the listener protocol is set with  `TCP` or `HTTPS`, and if session persistence is set with `HTTP_COOKIE`, `APP_COOKIE`, there's no load balancer operation related with session persistence. |
| pool.healthmonitor_id | Body | String | Health monitor ID |
| pool.listeners | Body | Array | List of listener objects in which pool is registered |
| pool.listeners.id | Body | UUID | Listener ID |
| pool.members | Body | Array | List of member objects registered at pool |
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
| pool.lb_algorithm | Body | Enum | - | Load balancing method of the pool <br>One of`ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
| pool.description | Body | String | - | Pool description |
| pool.admin_state_up | Body | Boolean | - | Administrator control status |
| pool.session_persistence | Body | Object | - | Session consistency object of pool |
| pool.session_persistence.type | Body | Enum | - | Session consistency<br>Set one of `SOURCE_IP`, `HTTP_COOKIE`, or `APP_COOKIE`<br>For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`.<br/>Even if the listener protocol is set with  `TCP` or `HTTPS`, and if session persistence is set with `HTTP_COOKIE`, `APP_COOKIE`, there's no load balancer operation related with session persistence. |
| pools.session_persistence.cookie_name | Body | String | - | Cookie name <br/>Set value is applied only when the session persistence type is `APP_COOKIE` |
| pool.name | Body | String | - | Pool name |



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

| Name                          | Type | Format  | Description                                                  |
| ----------------------------- | ---- | ------- | ------------------------------------------------------------ |
| pool                          | Body | Object  | Pool information object                                      |
| pool.lb_algorithm             | Body | Enum    | Load balancing method of the pool  One of  `ROUND_ROBIN`, `LEAST_CONNECTIONS`, and`SOURCE_IP` |
| pool.protocol                 | Body | Enum    | Member protocol                                              |
| pool.description              | Body | String  | Pool description                                             |
| pool.admin_state_up           | Body | Boolean | Administrator control status                                 |
| pool.tenant_id                | Body | String  | Tenant ID                                                    |
| pool.session_persistence      | Body | Object  | -                                                            |
| pool.session_persistence.type | Body | Enum    | Session persistence One of `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE`  For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`. Even if the listener protocol is set with  `TCP` or `HTTPS`, and if session persistence is set with `HTTP_COOKIE`, `APP_COOKIE`, there's no load balancer operation related with session persistence. |
| pool.healthmonitor_id         | Body | String  | Health monitor ID                                            |
| pool.listeners                | Body | Array   | List of listener objects in which pool is registered         |
| pool.listeners.id             | Body | UUID    | Listener ID                                                  |
| pool.members                  | Body | Array   | List of member objects registered at pool                    |
| pool.members.id               | Body | UUID    | Member ID                                                    |
| pool.id                       | Body | UUID    | Pool ID                                                      |
| pool.name                     | Body | String  | Pool name                                                    |

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
Delete a pool as specified. 
```
DELETE /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### Request
This API doe not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| poolId | URL | UUID | O | Pool ID |

#### Response

This API does not return a response body. 




























## Health Monitor
### List Health Monitors 

```
GET /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Health monitor ID |
| admin_state_up | Query | Boolean | - | Administrator control status |
| delay | Query | Integer | - | Interval of status check (seconds) |
| expected_codes | Query | String | - | HTTP response code of members to be considered in normal status <br>Available as a single value (200), list (201,202), or range (201-204)<br/>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| max_retries | Query | Integer | - | Number of maximum retries |
| http_method | Query | Enum | - | HTTP method to check status  <br>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| timeout | Query | Integer | - | Timeout for status check (seconds) |
| url_path | Query | String | - | URL requesting of status checks <br>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| type | Query | Enum | - | Protocol to check status: one of  `TCP`, `HTTP`, and `HTTPS` |



#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| healthmonitors | Body | Array | List of information objects of health monitor |
| healthmonitors.admin_state_up | Body | Boolean | Administrator control status |
| healthmonitors.delay | Body | Integer | Interval of status check (seconds) |
| healthmonitors.expected_codes | Body | String | HTTP response code of members to be considered in normal status <br/>Available as a single value (200), list (201,202), or range (201-204)<br/>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| healthmonitors.max_retries | Body | Integer | Number of maximum retries |
| healthmonitors.http_method | Body | Enum | HTTP Method to check status <br>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| healthmonitors.timeout | Body | Integer | Timeout for status check (seconds) |
| healthmonitors.pools | Body | Array | List of pool objects connected to health monitor |
| healthmonitors.pools.id | Body | UUID | Pool ID |
| healthmonitors.url_path | Body | String | URL requesting of status checks<br>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| healthmonitors.type | Body | Enum | Protocol to check status: one of `TCP`, `HTTP`, and `HTTPS` |
| healthmonitors.id | Body | UUID | Health monitor ID |

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


### Get Health Monitor 

```
GET /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| healthMonitorId | URL | UUID | O | Health monitor ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| healthmonitor | Body | Object | Health monitor information object |
| healthmonitor.admin_state_up | Body | Boolean | Administrator control status |
| healthmonitor.delay | Body | Integer | Status check interval (seconds) |
| healthmonitors.expected_codes | Body | String | HTTP response code of the member considered in normal status <br>Available as a single value (200), list (201,202), or range (201-204)<br>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| healthmonitor.max_retries | Body | Integer | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | HTTP method to check status <br>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| healthmonitor.timeout | Body | Integer | Timeout for status check (seconds) |
| healthmonitor.pools | Body | Array | List of pool objects connected to health monitor |
| healthmonitor.pools.id | Body | UUID | Pool ID |
| healthmonitor.url_path | Body | String | URL requesting of status checks <br>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| healthmonitor.type | Body | Enum | Protocol to check status: one of `TCP`, `HTTP`, and `HTTPS` |
| healthmonitor.id | Body | UUID | Health monitor ID |

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
| healthmonitor | Body | Object | O | Health monitor information object |
| healthmonitor.pool_id | Body | UUID | O | ID of pool to be connected with health monitor |
| healthmonitor.admin_state_up | Body | Boolean | - | Administrator control status |
| healthmonitor.health_check_port | Body | Integer | - | Member port to be health-checked |
| healthmonitor.delay | Body | Integer | O | Interval of status check (seconds) |
| healthmonitor.expected_codes | Body | String | - | HTTP response code of members to be considered in normal status: to be set with 200, if left blank. <br>Available as a single value (200), list (201,202), or range (201-204)<br>When the status check type is set with `TCP`, value set in this field shall be ignored. |
| healthmonitor.max_retries | Body | Integer | O | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | - | HTTP method to check status: if left blank, `GET` is enabled. <br>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.timeout | Body | Integer | O | Timeout for status checks |
| healthmonitor.url_path | Body | String | - | URL requesting of status checks: if left blank, `/`is set. <br>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.type | Body | Enum  | O | Protocol to check status: one of `TCP`, `HTTP`, and  `HTTPS` |



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
| healthmonitor | Body | Object | Health monitor information object |
| healthmonitor.admin_state_up | Body | Boolean | Administrator control status |
| healthmonitor.delay | Body | Integer | Status check interval (seconds) |
| healthmonitor.expected_codes | Body | String | HTTP response code of members to be considered in normal status: if left blank, set with 200. <br>Available as a single value (200), list (201,202), or range (201-204)<br>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.max_retries | Body | Integer | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | HTTP method to check status <br>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.timeout | Body | Integer | Timeout for status checks (seconds) |
| healthmonitor.pools | Body | Array | List of pool objects connected to health monitor |
| healthmonitor.pools.id | Body | UUID | Pool ID |
| healthmonitor.url_path | Body | String | URL requesting of status checks<br>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.type | Body | Enum | Protocol to check status: one of  `TCP`, `HTTP`, and`HTTPS` |
| healthmonitor.id | Body | UUID | Health monitor ID |

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
| healthmonitorId | URL | UUID | O | Health monitor ID |
| healthmonitor | Body | Object | O | Health monitor information object |
| healthmonitor.admin_state_up | Body | Boolean | - | Administrator control status |
| healthmonitor.delay | Body | Integer | - | Status check interval (seconds) |
| healthmonitor.expected_codes | Body | String | - | HTTP response code of members to be considered in normal status<br>Available as a single value (200), list (201,202), or range (201-204)<br>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.max_retries | Body | Integer | - | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | - | HTTP Method to check status <br>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.timeout | Body | Integer | - | Timeout for status checks (seconds) |
| healthmonitor.url_path | Body | String | - | URL requesting of status checks<br/>When the status check type is set with `TCP`, value set for this field shall be ignored. |

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
| healthmonitor | Body | Object | Health monitor information object |
| healthmonitor.admin_state_up | Body | Boolean | Administrator control status |
| healthmonitor.delay | Body | Integer | Status check interval (seconds) |
| healthmonitor.expected_codes | Body | String | HTTP response code of members to be considered in normal status<br/>Available as a single value (200), list (201,202), or range (201-204)
When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.max_retries | Body | Integer | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | HTTP Method to check status <br/>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.timeout | Body | Integer | Timeout for status checks (seconds) |
| healthmonitor.pools | Body | Array | List of pool objects connected to health monitor |
| healthmonitor.pools.id | Body | UUID | Pool ID |
| healthmonitor.url_path | Body | String | URL requesting of status checks<br/>When the status check type is set with `TCP`, value set for this field shall be ignored. |
| healthmonitor.type | Body | Enum | Protocol to check status: one of  `TCP`, `HTTP`, and`HTTPS` |
| healthmonitor.id | Body | UUID | Health monitor ID |

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
| tokenId | Header | String | O | Token ID |
| healthMonitorId | URL | UUID | O | Health Monitor ID |

#### Response

This API does not return a response body. 






























## Member
### List Members

```
GET /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| poolId | URL | UUID | O | ID of pool including a member |
| id | Query | UUID | - | Member ID |
| weight | Query | Integer | - | Member weight |
| admin_state_up | Query | Boolean | - | Administrator control status |
| subnet_id | Query | UUID | - | Subnet ID of member |
| tenant_id | Query | String | - | Tenant ID |
| address | Query | String | - | IP address of member |
| protocol_port | Query | Integer | - | Member port |
| operating_status | Query | Enum | - | Operating status of member |


#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| members | Body | Array | List of member information objects |
| members.weight | Body | Integer | Member weight |
| members.admin_state_up | Body | Boolean | Administrator control status |
| members.subnet_id | Body | UUID | Subnet ID of member |
| members.tenant_id | Body | String | Tenant ID |
| members.address | Body | String | IP address of member |
| members.protocol_port | Body | Integer | Member port |
| members.id | Body | UUID | Member ID |
| members.operating_status | Body | Enum | Operating status of member |

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


### Get Member

```
GET /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| poolId | URL | UUID | O | ID of pool to which member is included |
| memberId | URL | UUID | O | Member ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| member | Body | Object | Member information object |
| member.weight | Body | Integer | Member weight |
| member.admin_state_up | Body | Boolean | Administrator control status |
| member.subnet_id | Body | UUID | Subnet ID of member |
| member.tenant_id | Body | String | Tenant ID |
| member.address | Body | String | IP address of member |
| member.protocol_port | Body | Integer | Member port |
| member.id | Body | UUID | Member ID |
| member.operating_status | Body | Enum | Operating status of member |

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
| poolId | URL | UUID | O | ID of pool including member |
| member | Body | Object | O | Member information object |
| member.weight | Body | Integer | - | Member weight |
| member.admin_state_up | Body | Boolean | -| Administrator control status |
| member.subnet_id | Body | UUID | O | Subnet ID of member |
| member.address | Body | String | O | IP address of member |
| member.protocol_port | Body | Integer | O | Member port |

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

| Name                    | Type | Format  | Description                  |
| ----------------------- | ---- | ------- | ---------------------------- |
| member                  | Body | Object  | Member information object    |
| member.weight           | Body | Integer | Member weight                |
| member.admin_state_up   | Body | Boolean | Administrator control status |
| member.subnet_id        | Body | UUID    | Subnet ID of member          |
| member.tenant_id        | Body | String  | Tenant ID                    |
| member.address          | Body | String  | IP address of member         |
| member.protocol_port    | Body | Integer | Member port                  |
| member.id               | Body | UUID    | Member ID                    |
| member.operating_status | Body | Enum    | Operating status of member   |

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
| poolId | URL | UUID | O | ID of pool including member |
| memberId | URL | UUID | O | Member ID |
| member | Body | Object | O | Member information object |
| member.weight | Body | Integer | - | Member weight |
| member.admin_state_up | Body | Boolean | - | Administrator control status |

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
| member.admin_state_up | Body | Boolean | Administrator control status |
| member.subnet_id | Body | UUID | Subnet ID of member |
| member.tenant_id | Body | String | Tenant ID |
| member.address | Body | String | IP address of member |
| member.protocol_port | Body | Integer | Member port |
| member.id | Body | UUID | Member ID |
| member.operating_status | Body | Enum | Operating status of member |

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
| poolId | URL | UUID | O | ID of pool including member |
| memberId | URL | UUID | O | Member ID |

#### Response

This API does not return a response body.

























## Secret

Call Secret API by using the the `key-manager`-type endpoint. For more details,  see `serviceCatalog` from response of Issue Token. 

| Type | Region | Endpoint |
|---|---|---|
| key-manager | Korea (Pangyo)<br>Japan | https://kr1-api-key-manager.infrastructure.cloud.toast.com<br>https://jp1-api-key-manager.infrastructure.cloud.toast.com |

In API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the NHN Cloud internal usage and might be changed without previous notice. 


### List Secrets 

Return the list of secrets. 

```
GET /v1/secrets
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| offset | Query | Integer | - | Offset of response list: default is 0 |
| limit | Query | Integer| - | Maximum number of exposure on response list: default is 10 |
| name | Query | String | - | Secret name |
| alg | Query | String | - | Secret algorithm |
| mode | Query | String| - | Operating mode of block encryption |
| bits | Query | Integer| - | Length of encryption key |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| secrets | Body | Array | List of secret objects |
| secrets.secret_ref | Body | String | Secret address<br>In the`<barbican endpoint>/v1/secrets/<secret id>` format |
| secrets.secret_type | Body | Enum | Secret type <br>One of `symmetric`, `public`, `private`, `passphrase`, `certificate`, and`opaque` |
| secrets.status | Body | Enum | Secret status |
| secrets.content_types | Body | Array | List of content types of secret payload |
| secrets.content_types.default | Body | String | Default for content type |
| secrets.creator_id | Body | String | User ID creating a secret |
| secrets.mode | Body | String | Operating mode of block encryption: user-input metadata |
| secrets.algorithm | Body | String | Encryption algorithm: user-input metadata |
| secrets.bit_length | Body | Integer | Length of encryption key: user-input metadata |
| secrets.expiration | Body | Datetime | Expiration date: user-input metadata <br>`YYYY-MM-DDThh:mm:ss`<br>Expired secrets are automatically deleted |
| secrets.name| Body | String | Secret name |
| secrets.created | Body | Datetime | Created time <br> `YYYY-MM-DDThh:mm:ss` |
| secrets.updated | Body | Datetime | Updated time <br> `YYYY-MM-DDThh:mm:ss` |
| total | Body | Integer | Total secret count of requested queries |
| next | Body | String | URL of the next list to the current queried list |
| previous | Body | String | URL of the previous list of the current queried list |

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
      "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
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
      "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
      "secret_type": "private",
      "status": "ACTIVE",
      "updated": "2019-12-17T08:50:39"
    }
  ],
  "total": 10,
  "next": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets?limit=1&offset=2",
  "previous": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets?limit=1&offset=0"
}

```

</p>
</details>


### Get Secret
Return secret information as specified. 
```
GET /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| secretId | URL | UUID | O | Secret ID |

#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| secret | Body | Object | Secret object |
| secret.secret_ref | Body | String | Secret address<br>In the`<barbican endpoint>/v1/secrets/<secret id>` format |
| secret.secret_type | Body | Enum | Secret type <br>One of `symmetric`, `public`, `private`, `passphrase`, `certificate`,  and`opaque` |
| secret.status | Body | Enum | Secret status |
| secret.content_types | Body | Array | List of content types of secret payload |
| secret.content_types.default | Body | String | Default for content type |
| secret.creator_id | Body | String | User ID creating a secret |
| secret.mode | Body | String | Operating mode of block encryption: user-input metadata |
| secret.algorithm | Body | String | Encryption algorithm: user-input metadata |
| secret.bit_length | Body | Integer | Length of encryption key: user-input metadata |
| secret.expiration | Body | Datetime | Expiration date: user-input metadata <br>`YYYY-MM-DDThh:mm:ss`<br>Expired secrets are automatically deleted |
| secret.name| Body | String | Secret name |
| secret.created | Body | Datetime | Created time <br> `YYYY-MM-DDThh:mm:ss` |
| secret.updated | Body | Datetime | Updated time <br> `YYYY-MM-DDThh:mm:ss` |

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
  "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
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
| expiration | Body | Datetime | - | Expiration date: requested in the ISO8601 format |
| algorithm | Body | String | - | Encryption algorithm |
| bit_length | Body | String | - | Length of encryption key |
| mode | Body | String | - | Operating mode of block encryption |
| payload | Body | String | - | Payload of encryption key |
| payload_content_type | Body | String | - | Content type of encryption key payload <br>Must be included to enter payload <br>List of supported content types:  `text/plain`, `application/octet-stream`, `application/pkcs8`, and`application/pkix-cert` |
| payload_content_encoding | Body | Enum | - | Encoding mode of encryption key payload <br>Must be included if the payload_content_type is not text/plain<br>Supports only `base64` |
| secret_type | Body | Enum | - | Secret type <br>One of `symmetric`, `public`, `private`, `passphrase`, `certificate`, and  `opaque` |



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


Send payload on text
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

Send payload via base64
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
| secret_ref | Body | String | Secret address<br>In the format of`<barbican endpoint>/v1/secrets/<secret id>` |

<details><summary>Example</summary>
<p>


```json
{
    "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/9b2dcb7b-51fe-4408-a2bb-23da731758a6"
}
```
</p>
</details>

---
### Modify Secret 
Enter payload data of secret which previously had metadata only. 
```
PUT /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
Content-Type: {ConetentType}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| secretId | URL | UUID | O | Secret ID |
| ContentType| Header | Enum | O | One of `text/plain`, `application/octet-stream`, `application/pkcs8`, and `application/pkix-cert` <br>If left blank, `text/plain` is configured. |
| payload | Body | String | O | Payload for encryption key |

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
Delete a secret as specified. 
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

Call Secret Container API by using the the `key-manager`-type endpoint. For more details,  see `serviceCatalog` from response of Issue Token. 

| Type | Region | Endpoint |
|---|---|---|
| key-manager | Korea (Pangyo)<br>Japan | https://kr1-api-key-manager.infrastructure.cloud.toast.com<br>https://jp1-api-key-manager.infrastructure.cloud.toast.com |

In API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the NHN Cloud internal usage and might be changed without previous notice. 


### List Secret Containers 

Return the list of secret containers. 

```
GET /v1/containers
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| offset | Query | Integer | - | Offset of response list, default: 0 |
| limit | Query | Integer | - | Maximum count to show on the response list, default: 10 |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| containers | Body | Array | List of container objects |
| containers.status | Body | Enum | Container status |
| containers.updated | Body | Datetime | Updated time, `YYYY-MM-DDThh:mm:ss` |
| containers.name | Body | String | Container name |
| containers.consumers | Body | Array | List of consumers |
| containers.consumers.URL | Body | String | Consumer URL |
| containers.consumers.name | Body | String | Consumer name |
| containers.created | Body | Datetime | Created time, `YYYY-MM-DDThh:mm:ss` |
| containers.container_ref | Body | String | Container address |
| containers.creator_id | Body | String | User ID creating container |
| containers.secret_refs | Body | Array | List of secrets |
| containers.secret_refs.secret_ref | Body | String | Secret address |
| containers.secret_refs.name | Body | String | Secret name as specified by container<br>When the container type is `certificate`: specify `certificate`, `private_key`, `private_key_passphrase`, or`intermediates`<br>When the container type is `rsa`: specify  `private_key`, `private_key_passphrase`, or`public_key` |
| containers.type | Body | Enum | Container type<br>One of `generic`, `rsa`, and`certificate` |
| total | Body | Integer | Total number of secret containers of a requested query |
| next | Body | String | URL of the next list to the current queried list |
| previous | Body | String | URL of the previous list of the current queried list |



<details><summary>Example</summary>
<p>


```json
{
  "total": 10,
  "previous": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers?limit=1&offset=0",
  "next": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers?limit=1&offset=2",
  "containers": [
    {
      "status": "ACTIVE",
      "updated": "2019-12-17T08:50:39",
      "name": "The Certificate",
      "consumers": [],
      "created": "2019-12-17T08:50:39",
      "container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/2d1dcf4d-2e92-475e-bde7-e469880be924",
      "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
      "secret_refs": [
        {
          "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
          "name": "certificate"
        },
        {
          "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
          "name": "private_key"
        }
      ],
      "type": "certificate"
    }
  ]
}


```
</p>
</details>


### Get Secret Container
Return secret container information as specified. 
```
GET /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| containerId | URL | UUID | O | Secret container ID |

#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| status | Body | Enum | Container status |
| updated | Body | Datetime | Updated time, `YYYY-MM-DDThh:mm:ss` |
| name | Body | String | Container name |
| consumers | Body | Array | List of consumers |
| consumers.URL | Body | String | Consumer URL |
| consumers.name | Body | String | Consumer name |
| created | Body | Datetime | Created time,  `YYYY-MM-DDThh:mm:ss` |
| container_ref | Body | String | Container address |
| creator_id | Body | String | User ID creating container |
| secret_refs | Body | Array | List of secrets registered at container |
| secret_refs.secret_ref | Body | String | Secret address |
| secret_refs.name | Body | String| Secret name as specified by container<br>When the container type is `certificate`: specify  `certificate`, `private_key`, `private_key_passphrase`, or `intermediates`<br> When the container type is `rsa`: Specify`private_key`, `private_key_passphrase`, or `public_key` |
| type | Body | Enum | Container type<br>One of `generic`, `rsa`, and `certificate` |

<details><summary>Example</summary>


```json
{
    "status": "ACTIVE",
    "updated": "2019-12-17T08:50:39",
    "name": "The Certificate",
    "consumers": [],
    "created": "2019-12-17T08:50:39",
    "container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/2d1dcf4d-2e92-475e-bde7-e469880be924",
    "creator_id": "1da4ce9f59ed4f6487c9be39fa792be4",
    "secret_refs": [
        {
            "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/36f88d4c-16f0-4db2-80bc-4dda0125589b",
            "name": "private_key"
        },
        {
            "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/adffcd66-ff63-4c66-8139-2f254e63aef5",
            "name": "certificate"
        }
    ],
    "type": "certificate"
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
| tokenId | Header | String | O | Token ID |
| type | Body | Enum | O | Container type<br>One of `generic`, `rsa`, and  `certificate` |
| name | Body | String | - | Container name |
| secret_refs | Body | Array | - | Secret list to be registered at container |
| secret_refs.secret_ref | Body | String | - | Secret address |
| secret_refs.name | Body | String | - | Secret name as specified by container<br>When the container type is `certificate`: Specify `certificate`, `private_key`, `private_key_passphrase`, or `intermediates`<br>When the container type is `rsa`: Specify `private_key`, `private_key_passphrase`, or`public_key` |

<details><summary>Example</summary>
<p>


```json
{
    "type": "certificate",
    "name": "test cert",
    "secret_refs": [
        {
            "name": "private_key",
            "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/cf11edcf-f475-47f3-92c3-29de8bcdd639"
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
    "container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/ea2e90fc-1ba2-412b-b7a0-61da4402bf58"
}
```
</p>
</details>

---
### Delete Secret Container
Delete specified secret container. 
```
DELETE /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| containerId | URL | UUID | Secret container ID ||


#### Response

This API does not return a response body. 
