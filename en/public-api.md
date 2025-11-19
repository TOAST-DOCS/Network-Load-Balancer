## Network > Load Balancer > API v2 Guide

 To use the API, API endpoint and token are required. Prepare information required to use an API by referring to [Preparations for Using the API](/Compute/Compute/en/identity-api/)

For Load Balancer, Listener, Pool, Health Monitor, and Member API, use `network` type endpoint. For Secret and Secret Container API, call the API by using the `key-manager` type endpoint. For the exact endpoint, see `serviceCatalog` in the response of token issuance.

| Type | Region | Endpoint |
|---|---|---|
| network | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Japan(Tokyo) Region<br>USA (California) Region | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com<br>https://us1-api-network-infrastructure.nhncloudservice.com |
| key-manager | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Japan (Tokyo) Region<br>USA (California) Region | https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-key-manager-infrastructure.nhncloudservice.com |


In the API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the NHN Cloud internal usage and might be changed without prior notice.

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
| loadbalancer_type | Query | String | - | The type of load balancer to view<br>Either `shared` or `dedicated` |


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
| loadbalancers.pools | Body | Object | List of load balancer pool objects |
| loadbalancers.pools.id | Body | UUID | Pool ID |
| loadbalancers.vip_address | Body | String | Load balancer IP |
| loadbalancers.vip_port_id | Body | UUID | Port ID of load balancer |
| loadbalancers.vip_subnet_id | Body | UUID | Subnet ID of load balancer |
| loadbalancers.id | Body | UUID | Load balancer ID |
| loadbalancers.operating_status | Body | Enum | Operating status of load balancer |
| loadbalancers.admin_state_up | Body | Boolean | Administrator control status of load balancer |
| loadbalancers.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancers.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancers.ipacl_group_action | Body | String | The action of IP ACL groups applied to the load balancer<br>One of `null`, `DENY`, and `ALLOW` |
| loadbalancers.loadbalancer_type | Body | String | Load Balancer Type<br>Either `shared` or `dedicated` |

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
| loadbalancers.pools | Body | Object | List of load balancer pool objects |
| loadbalancers.pools.id | Body | UUID | Pool ID |
| loadbalancer.vip_address | Body | String | Load balancer IP |
| loadbalancer.vip_port_id | Body | UUID | Port ID of load balancer |
| loadbalancer.vip_subnet_id | Body | UUID | Subnet ID of load balancer |
| loadbalancer.id | Body | UUID | Load balancer ID |
| loadbalancer.operating_status | Body | Enum | Operating status of load balancer |
| loadbalancer.admin_state_up | Body | Boolean | Administrator control center of load balancer |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_group_action | Body | String | The action of IP ACL groups applied to the load balancer<br>One of `null`, `DENY`, and `ALLOW` |
| loadbalancer.loadbalancer_type | Body | String | Load Balancer Type<br>Either `shared` or `dedicated` |


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
| loadbalancers.pools | Body | Object | List of load balancer pool objects |
| loadbalancers.pools.id | Body | UUID | Pool ID |
| loadbalancer.vip_address | Body | String | IP of load balancer |
| loadbalancer.vip_port_id | Body | UUID | Port ID of load balancer |
| loadbalancer.vip_subnet_id | Body | UUID | Subnet ID of load balancer |
| loadbalancer.id | Body | UUID | Load balancer ID |
| loadbalancer.operating_status | Body | Enum | Operating status of load balancer |
| loadbalancer.admin_state_up | Body | Boolean | Administrator control status of load balancer |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_group_action | Body | String | The action of IP ACL groups applied to the load balancer <br>Should be one of the following: `null`, `DENY`, or `ALLOW`  |
| loadbalancer.loadbalancer_type | Body | String | Load Balancer Type<br>Either `shared` or `dedicated` |


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

| Name | Type | Format | Description |
|---|---|---|---|
| loadbalancer | Body | Object | Information object of load balancer |
| loadbalancer.description | Body | String | Description of load balancer |
| loadbalancer.provisioning_status | Body | Enum | Provisioning status of load balancer |
| loadbalancer.tenant_id | Body | String | Tenant ID |
| loadbalancer.provider | Body | String | Name of load balancer provider |
| loadbalancer.name | Body | String | Name of load balancer |
| loadbalancer.listeners | Body | Object | List of load balancer listener objects |
| loadbalancer.listeners.id | Body | UUID | Listener ID |
| loadbalancers.pools | Body | Object | List of load balancer pool objects |
| loadbalancers.pools.id | Body | UUID | Pool ID |
| loadbalancer.vip_address | Body | String | Load balancer IP |
| loadbalancer.vip_port_id | Body | UUID | Port ID of load balancer |
| loadbalancer.vip_subnet_id | Body | UUID | Subnet ID of load balancer |
| loadbalancer.id | Body | UUID | Load balancer ID |
| loadbalancer.operating_status | Body | Enum | Operating status of load balancer |
| loadbalancer.admin_state_up | Body | Boolean | Administrator control status of load balancer |
| loadbalancer.ipacl_groups | Body | Object | IP ACL group object applied to the load balancer |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACL group ID |
| loadbalancer.ipacl_group_action | Body | String | The action of IP ACL groups applied to the load balancer <br>Should be one of the following: `null`, `DENY`, or `ALLOW`  |
| loadbalancer.loadbalancer_type | Body | String | Load Balancer Type<br>Either `shared` or `dedicated` |


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
| default_pool_id | Query | UUID | - | ID of default member group (pool) registered at listener |
| protocol | Query | Enum | - | Protocol of listener <br>One of `TCP`, `HTTP`, `HTTPS`, and `TERMINATED_HTTPS` |
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
| listeners.default_pool_id | Body | UUID | ID of default member group (pool) registered at listener |
| listeners.protocol | Body | Enum | Protocol of listener <br>One of `TCP`, `HTTP`, `HTTPS`, and `TERMINATED_HTTPS` |
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
| listener.default_pool_id | Body | UUID | ID of default member group (pool) registered at listener |
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
| listener | Body | Object | O | Information object of listener |
| listener.protocol | Body | Enum | O | Listener protocol <br>One of `TCP`, `HTTP`,`HTTPS`, and `TERMINATED_HTTPS` |
| listener.description | Body | String | - | Description of listener |
| listener.name | Body | String | - | Name of listener |
| listener.default_pool_id | Body | UUID | - | ID of the default member group (pool) registered at listener<br>Created as `Not use` if not specified |
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
| listener | Body | Object | Information object of listener |
| listener.default_pool_id | Body | UUID | ID of default group member (pool) registered at listener |
| listener.protocol | Body | Enum | Protocol of listener <br>One of `TCP`, `HTTP`,`HTTPS`, and `TERMINATED_HTTPS` |
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
| listener | Body | Object | O | Information object of listener |
| listener.description | Body | String | - | Listener description |
| listener.name | Body | String| - | Listener name |
| listener.default_pool_id | Body | UUID | - | ID of the default member group (pool) registered at listener<br>Created as `Not use` if not specified |
| listener.admin_state_up | Body | Boolean | - | Administrator control status |
| listener.connection_limit | Body |  Integer | - | Connection limit of listener |
| listener.keepalive_timeout | Body | Integer | - | Keepalive timeout of listener |
| listener.enable_x_forwarded_proto | Body | Boolean | - | X-Forwarded-Proto/X-Forwarded-Prot header on/off<br>Default: `true` |
| listener.enable_x_forwarded_port | Body | Boolean | - | X-Forwarded-Port header on/off<br>Default: `true` |
| listener.enable_x_forwarded_for | Body | Boolean | - | HAProxy `option forwardfor` and X-Forwarded-For header on/off<br>Default: `true` |
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
| listener.default_pool_id | Body | UUID | ID of default member group (pull) registered at listener |
| listener.protocol | Body | Enum | Listener protocol<br>One of `TCP`, `HTTP`,`HTTPS`, and `TERMINATED_HTTPS` |
| listener.description | Body | String | Listener description |
| listener.name | Body | String | Listener name |
| listener.loadbalancers | Body | Array | List of load balancer objects in which listener is registered |
| listener.loadbalancers.id | Body | UUID | Load balancer ID |
| listener.tenant_id | Body | String | Tenant ID |
| listener.admin_state_up | Body | Boolean | Administrator control status |
| listener.connection_limit | Body | Integer | Connection limit of listener |
| listener.keepalive_timeout | Body | Integer | Keepalive timeout of listener |
| listener.enable_x_forwarded_proto | Body | Boolean | X-Forwarded-Proto/X-Forwarded-Prot header on/off |
| listener.enable_x_forwarded_port | Body | Boolean | X-Forwarded-Port header on/off |
| listener.enable_x_forwarded_for | Body | Boolean | HAProxy `option forwardfor` and X-Forwarded-For header on/off |
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
Deletes the specified listener.
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
---
### Create Custom Response

```
POST /v2.0/lbaas/listeners/{listenerId}/errorpages
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| listenerId | URL | UUID | O | Listener ID |
| errorpage | Body | Object | O | Custom response information object |
| errorpage.code | Body | Integer | O | Error code<br>One of `200`, `400`, `403`, `405`, `408`, `425`, `429`, `500`, `502`, `503`, `504` |
| errorpage.content_type | Body | Enum | O | Content type<br>One of `application/javascript`, `application/json`, `text/css`, `text/html`, `text/plain` |
| errorpage.body | Body | String | O | Custom response body (within 1024 characters) |

**Note**: Duplicate codes cannot be created for the same listener. (e.g., creating multiple 504 codes)

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
| errorpage.content_type | Body | Enum | O | Content type<br>One of `application/javascript`, `application/json`, `text/css`, `text/html`, `text/plain` |
| errorpage.body | Body | String | O | Custom response body (within 1024 characters) |

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
| errorpageId | URL | UUID | O | Custom response ID |

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
| tokenId | Header | String | O | Token ID |
| listenerId | URL | UUID | O | Listener ID |
| errorpageId | URL | UUID | O | Custom response ID |

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

### List Custom Responses

```
GET /v2.0/lbaas/listeners/{listenerId}/errorpages
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
| lb_algorithm | Query | Enum | - | Load balancing method of the pool <br>One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, and `SOURCE_IP` |
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
| pools.loadbalancers | Body | Array | List of load balancer objects with the pool registered |
| pools.loadbalancers.id | Body | UUID | Load balancer ID |
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
| pool.session_persistence.type | Body | Enum | Session persistence<br>Set one of `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`.<br/>Even if the listener protocol is set with  `TCP` or `HTTPS`, and if session persistence is set with `HTTP_COOKIE`, `APP_COOKIE`, there's no load balancer operation related with session persistence. |
| pool.session_persistence.cookie_name | Body | String | Cookie name <br>Set value is applied only when the session persistence type is `APP_COOKIE` |
| pool.healthmonitor_id | Body | UUID | Health monitor ID |
| pools.loadbalancers | Body | Array | List of load balancer objects with the pool registered |
| pools.loadbalancers.id | Body | UUID | Load balancer ID |
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
| pool.loadbalancer_id | Body | UUID | - | Load balancer ID to which the pool will be attached. At least one of the load balancer ID or listener ID must be specified. |
| pool.listener_id | Body | UUID | - | Listener ID to which the pool will be attached. At least one of the load balancer ID or listener ID must be specified. |
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
| pool.lb_algorithm | Body | Enum | Load balancing method of the pool <br>One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, and `SOURCE_IP` |
| pool.protocol | Body | Enum | Member protocol |
| pool.description | Body | String | Pool description |
| pool.admin_state_up | Body | Boolean | Administrator control status |
| pool.tenant_id | Body | String | Tenant ID |
| pool.session_persistence | Body | Object | - |
| pool.session_persistence.type | Body | Enum | Session persistence<br>One of `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`.<br/>Even if the listener protocol is set with  `TCP` or `HTTPS`, and if session persistence is set with `HTTP_COOKIE`, `APP_COOKIE`, there's no load balancer operation related with session persistence. |
| pool.healthmonitor_id | Body | String | Health monitor ID |
| pool.loadbalancers | Body | Array | List of load balancer objects with the pool registered |
| pool.loadbalancers.id | Body | UUID | Load balancer ID |
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
| pool.lb_algorithm | Body | Enum | - | Load balancing method of the pool <br>One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, or `SOURCE_IP` |
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
| pool.lb_algorithm             | Body | Enum    | Load balancing method of the pool  One of `ROUND_ROBIN`, `LEAST_CONNECTIONS`, and `SOURCE_IP` |
| pool.protocol                 | Body | Enum    | Member protocol                                              |
| pool.description              | Body | String  | Pool description                                             |
| pool.admin_state_up           | Body | Boolean | Administrator control status                                 |
| pool.tenant_id                | Body | String  | Tenant ID                                                    |
| pool.session_persistence      | Body | Object  | -                                                            |
| pool.session_persistence.type | Body | Enum    | Session persistence One of `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE`  For `HTTP_COOKIE` or `APP_COOKIE` settings, it is recommended to check if the protocol of attached listener is set with `HTTP` or `TERMINATED_HTTPS`. Even if the listener protocol is set with  `TCP` or `HTTPS`, and if session persistence is set with `HTTP_COOKIE`, `APP_COOKIE`, there's no load balancer operation related with session persistence. |
| pools.session_persistence.cookie_name | Body | String | Cookie name <br>The setting value is applied only when the session persistence type is `APP_COOKIE`. |
| pool.healthmonitor_id         | Body | UUID    | Health monitor ID                                            |
| pool.listeners                | Body | Array   | List of listener objects in which pool is registered         |
| pool.loadbalancers.id | Body | UUID | Load balancer ID |
| pool.listeners | Body | Array | List of listener objects with the pool registered |
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
Deletes the specified pool.
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
| expected_codes | Query | String | - | HTTP response code of members to be considered in normal status <br>Available as a single value (200), list (201,202), or range (201-204)<br/>When the status check type is set with `TCP`, value set in this field will be ignored. |
| max_retries | Query | Integer | - | Number of maximum retries |
| http_method | Query | Enum | - | HTTP method to use for status check  <br>When the status check type is set with `TCP`, value set in this field will be ignored. |
| timeout | Query | Integer | - | Timeout for status check (seconds) |
| url_path | Query | String | - | URL requesting of status checks <br>When the status check type is set with `TCP`, value set in this field will be ignored. |
| type | Query | Enum | - | Protocol to use for status check: One of `TCP`, `HTTP`, and `HTTPS` |
| host_header | Query | String | - | Host header field value to use for status check<br> When the status check type is set with `TCP`, value set in this field will be ignored.|



#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| healthmonitors | Body | Array | List of information objects of health monitor |
| healthmonitors.admin_state_up | Body | Boolean | Administrator control status |
| healthmonitors.delay | Body | Integer | Interval of status check (seconds) |
| healthmonitor.health_check_port | Body | Integer | - | Member port targeted by health check <br> * If you specify 0, health checks are performed on the specified port number for each member. <br> * If you enter a non-zero positive number, health checks are performed on the entered port number, regardless of the port number specified for each member.|
| healthmonitors.expected_codes | Body | String | HTTP response code of members to be considered in normal status <br/>Available as a single value (200), list (201,202), or range (201-204)<br/>When the status check type is set with `TCP`, value set in this field will be ignored. |
| healthmonitors.max_retries | Body | Integer | Number of maximum retries |
| healthmonitors.http_method | Body | Enum | HTTP Method to use for status check <br>When the status check type is set with `TCP`, value set in this field will be ignored. |
| healthmonitors.timeout | Body | Integer | Timeout for status check (seconds) |
| healthmonitors.pools | Body | Array | List of pool objects connected to health monitor |
| healthmonitors.pools.id | Body | UUID | Pool ID |
| healthmonitors.url_path | Body | String | URL requesting of status checks<br>When the status check type is set with `TCP`, value set in this field will be ignored. |
| healthmonitors.type | Body | Enum | Protocol to use for status check: one of `TCP`, `HTTP`, and `HTTPS` |
| healthmonitors.id | Body | UUID | Health monitor ID |
| healthmonitors.host_header | Body | String | Host header field value to use for status check<br> When the status check type is set with `TCP`, value set in this field will be ignored.|



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
| healthmonitors.expected_codes | Body | String | HTTP response code of the member considered in normal status <br>Available as a single value (200), list (201,202), or range (201-204)<br>When the status check type is set with `TCP`, value set in this field will be ignored. |
| healthmonitor.health_check_port | Body | Integer | - | Member port targeted by health check <br> * If you specify 0, health checks are performed on the specified port number for each member. <br> * If you enter a non-zero positive number, health checks are performed on the entered port number, regardless of the port number specified for each member.|
| healthmonitor.expected_codes | Body | String | HTTP(S) response code of members to be considered as normal status<br>Available as a single value (200), list (201,202), or range (201-204)<br> If you set the health check type to `TCP`, the value you set in this field is ignored.|
| healthmonitor.max_retries | Body | Integer | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | HTTP method to use for status check <br>When the status check type is set with `TCP`, value set in this field will be ignored. |
| healthmonitor.timeout | Body | Integer | Timeout for status check (seconds) |
| healthmonitor.pools | Body | Array | List of pool objects connected to health monitor |
| healthmonitor.pools.id | Body | UUID | Pool ID |
| healthmonitor.url_path | Body | String | URL requesting of status checks <br>When the status check type is set with `TCP`, value set in this field will be ignored. |
| healthmonitor.type | Body | Enum | Protocol to use for status check: one of `TCP`, `HTTP`, and `HTTPS` |
| healthmonitor.id | Body | UUID | Health monitor ID |
| healthmonitors.host_header | Body | String | Host header field value to use for status check<br> When the status check type is set with `TCP`, value set in this field will be ignored.|



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
| healthmonitor.health_check_port | Body | Integer | - | Member port targeted by health check <br> * If you specify 0, health checks are performed on the specified port number for each member. <br> * If you enter a non-zero positive number, health checks are performed on the entered port number, regardless of the port number specified for each member.|
| healthmonitor.delay | Body | Integer | O | Interval of status check (seconds) |
| healthmonitor.expected_codes | Body | String | - | HTTP response code of members to be considered in normal status: to be set with 200, if left blank. <br>Available as a single value (200), list (201,202), or range (201-204)<br>When the status check type is set with `TCP`, value set in this field will be ignored. |
| healthmonitor.max_retries | Body | Integer | O | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | - | HTTP method to use for status check: if left blank, `GET` is used. <br>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.timeout | Body | Integer | O | Timeout for status checks |
| healthmonitor.url_path | Body | String | - | URL requesting of status checks: if left blank, `/`is set. <br>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.type | Body | Enum  | O | Protocol to use for status check: one of `TCP`, `HTTP`, and  `HTTPS` |
| healthmonitors.host_header | Body | String | - | Host header field value to use for status check<br> When the status check type is set with `TCP`, value set in this field will be ignored.|





<details><summary>예시</summary>
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
| healthmonitor.health_check_port | Body | Integer | - | Member port targeted by health check <br> * If you specify 0, health checks are performed on the specified port number for each member. <br> * If you enter a non-zero positive number, health checks are performed on the entered port number, regardless of the port number specified for each member.|
| healthmonitor.expected_codes | Body | String | HTTP response code of members to be considered in normal status: if left blank, set with 200. <br>Available as a single value (200), list (201,202), or range (201-204)<br>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.max_retries | Body | Integer | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | HTTP method to use for status check <br>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.timeout | Body | Integer | Timeout for status checks (seconds) |
| healthmonitor.pools | Body | Array | List of pool objects connected to health monitor |
| healthmonitor.pools.id | Body | UUID | Pool ID |
| healthmonitor.url_path | Body | String | URL requesting of status checks<br>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.type | Body | Enum | Protocol to use for status check: One of `TCP`, `HTTP`, and `HTTPS` |
| healthmonitor.id | Body | UUID | Health monitor ID |
| healthmonitor.host_header | Body | String | Host header field value to use for status check<br> When the status check type is set with `TCP`, value set in this field will be ignored.|



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
| healthmonitor.health_check_port | Body | Integer | - | Member port targeted by health check <br> * If you specify 0, health checks are performed on the specified port number for each member. <br> * If you enter a non-zero positive number, health checks are performed on the entered port number, regardless of the port number specified for each member.|
| healthmonitor.delay | Body | Integer | - | Status check interval (seconds) |
| healthmonitor.expected_codes | Body | String | - | HTTP response code of members to be considered in normal status<br>Available as a single value (200), list (201,202), or range (201-204)<br>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.max_retries | Body | Integer | - | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | - | HTTP Method to use for status check <br>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.timeout | Body | Integer | - | Timeout for status checks (seconds) |
| healthmonitor.url_path | Body | String | - | URL requesting of status checks<br/>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.host_header | Body | String | - | Host header field value to use for status check<br> When the status check type is set with `TCP`, value set in this field will be ignored.|


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
| healthmonitor.health_check_port | Body | Integer | - | Member port targeted by health check <br> * If you specify 0, health checks are performed on the specified port number for each member. <br> * If you enter a non-zero positive number, health checks are performed on the entered port number, regardless of the port number specified for each member.|
| healthmonitor.expected_codes | Body | String | HTTP response code of members to be considered in normal status<br/>Available as a single value (200), list (201,202), or range (201-204) When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.max_retries | Body | Integer | Number of maximum retries |
| healthmonitor.http_method | Body | Enum | HTTP Method to use for status check <br/>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.timeout | Body | Integer | Timeout for status checks (seconds) |
| healthmonitor.pools | Body | Array | List of pool objects connected to health monitor |
| healthmonitor.pools.id | Body | UUID | Pool ID |
| healthmonitor.url_path | Body | String | URL requesting of status checks<br/>When the status check type is set with `TCP`, value set for this field will be ignored. |
| healthmonitor.type | Body | Enum | Protocol to use for status check: One of `TCP`, `HTTP`, and `HTTPS` |
| healthmonitor.id | Body | UUID | Health monitor ID |
| healthmonitor.host_header | Body | String | Host header field value to use for status check<br> When the status check type is set with `TCP`, value set in this field will be ignored.|


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


















## L7 Policy

### List L7 Policies

```
GET /v2.0/lbaas/l7policies
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | L7 policy to query |
| name | Query | String | - | L7 policy name to query |
| description | Query | String | - | Description of the L7 policy to query |
| listener_id | Query | UUID | - | Listener ID of L7 policy to query |
| Endpoint stage ID | Query | Enum | - | Action of  L7 policy to query <br> Either `REDIRECT_TO_POOL or REDIRECT_TO_URL/REJECT`  |
| redirect_pool_id | Query | UUID | - | Redirect pool ID of L7 policy to query<br>Applies only if action is `REDIRECT_TO_POOL` |
| redirect_url |  | String | - | Redirect URL of the L7 policy to look up<br>Applies only if action is `REDIRECT_TO_URL`|
| redirect_http_code | Query | Integer | - | Redirect HTTP response codes for L7 policies |
| position | Query | Integer | - | Priority of L7 policy to query |


#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| l7policies | Body | Array | List of L7 policy objects |
| l7policies.description | Body | String | L7 policy description |
| l7policies.tenant_id | Body | String | Tenant ID |
| l7policies.listener_id | Body | UUID | Listener ID for L7 policy |
| l7policies.name | Body | String | L7 policy description |
| l7policies.rules | Body | Object | List of L7 policy rule objects |
| l7policies.rules.id | Body | UUID | L7 Rule ID |
| l7policies.id | Body | UUID | L7 Policy ID |
| l7policies.admin_state_up | Body | Boolean | L7 policy manager control status |
| l7policies.action | Body | Enum | Action of L7 policy<br> One of the following: `REDIRECT_TO_POOL`, `REDIRECT_TO_URL`, or `REJECT` |
| l7policies.redirect_pool_id | Body | UUID | Redirect pool ID of  L7 policy<br>Applies only if action is `REDIRECT_TO_POOL` |
| l7policies.redirect_url | Body | String | Redirect URL of L7 policy<br>Applies only if action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code  | Body | Integer | - | Redirect HTTP response codes for L7 policies |
| l7policies.position | Body | Integer | Prioritization of L7 policies |

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
### List L7 policies

```
GET /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| l7policyId | URL | UUID | O | L7 Policy ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| l7policy | Body | Object | L7 policy object |
| l7policy.description | Body | String | L7 policy description |
| l7policy.tenant_id | Body | String | Tenant ID |
| l7policy.listener_id | Body | UUID | Listener ID for L7 policy |
| l7policy.name | Body | String | L7 policy description |
| l7policy.rules | Body | Object | List of L7 policy rule objects |
| l7policy.rules.id | Body | UUID | L7 Rule ID |
| l7policy.id | Body | UUID | L7 Policy ID |
| l7policy.admin_state_up | Body | Boolean | L7 policy manager control status |
| l7policy.action | Body | Enum | Action of L7 policy<br> One of the following: `REDIRECT_TO_POOL`, `REDIRECT_TO_URL`, or `REJECT` |
| l7policy.redirect_pool_id | Body | UUID | Redirect pool ID of L7 policy<br>Applies only if action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | Redirect URL of L7 policy<br>Applies only if action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code  | Body | Integer | - | Redirect HTTP response codes for L7 policies |
| l7policy.position | Body | Integer | Prioritization of L7 policies |


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
### Create L7 policy

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
| l7policy.listener_id | Body | UUID | O | Listener ID for L7 policy |
| l7policy.name | Body | String | - | L7 policy description |
| l7policy.admin_state_up | Body | Boolean | - | Set to `true`if omitted as L7 Policy Manager control status |
| l7policy.action | Body | Enum | O | Action of L7 policy<br> One of the following: `REDIRECT_TO_POOL`, `REDIRECT_TO_URL`, or `REJECT` |
| l7policy.redirect_pool_id | Body | UUID | - | Redirect pool ID of L7 policy<br>Required if action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | - | Redirect URL of L7 policy<br>Required if action is `REDIRECT_TO_URL` <br> \* The acceptable format is `#{protocol}://#{host}:#{port}/#{path}?#{query}`, which will retain the value from the existing request when entered in `#{_}` form. If you enter a non `#{_}` value directly, the redirect URL will apply that value and return it to the client. <br> \* At least one of protocol, host, port, and path must be changed to prevent infinite redirects.|
| l7policy.redirect_http_code | Body | Integer | - | Redirect HTTP response codes for L7 policies <br>  |
| l7policy.position | Body | Integer | - | Priority of the L7 policy. Sets to last priority if omitted |



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
| l7policy.listener_id | Body | UUID | Listener ID for L7 policy |
| l7policy.name | Body | String | L7 policy description |
| l7policy.rules | Body | Object | List of L7 policy rule objects |
| l7policy.rules.id | Body | UUID | L7 Rule ID |
| l7policy.id | Body | UUID | L7 Policy ID |
| l7policy.admin_state_up | Body | Boolean | Administrator control status of L7 policy |
| l7policy.action | Body | Enum | Action of L7 policy<br> One of the following: `REDIRECT_TO_POOL`, `REDIRECT_TO_URL`, or `REJECT` |
| l7policy.redirect_pool_id | Body | UUID | Redirect pool ID of L7 policy<br>Applies only if action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | Redirect URL of L7 policy<br>Applies only if action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code | Body | Integer | - | Redirect HTTP response codes for L7 policies |
| l7policy.position | Body | Integer | Prioritization of L7 policies |


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
| l7policy | Body | Object | O | L7 policy object |
| l7policy.name | Body | String | - | L7 policy description |
| l7policy.description | Body | String | - | L7 policy description |
| l7policy.admin_state_up | Body | Boolean | - | Administrator control status of L7 policy |
| l7policy.action | Body | Enum | - | Action of L7 policy<br> One of the following: `REDIRECT_TO_POOL`, `REDIRECT_TO_URL`, or `REJECT` |
| l7policy.redirect_pool_id | Body | UUID | - | Redirect pool ID of L7 policy<br>Required if action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | - | Redirect URL of L7 policy<br>Required if action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code | Body | Integer | - | Redirect HTTP response codes for L7 policies |
| l7policy.position | Body | Integer | - | Prioritization of L7 policies |

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
| l7policy.listener_id | Body | UUID | Listener ID for L7 policy |
| l7policy.name | Body | String | L7 policy description |
| l7policy.rules | Body | Object | List of L7 policy rule objects |
| l7policy.rules.id | Body | UUID | L7 Rule ID |
| l7policy.id | Body | UUID | L7 Policy ID |
| l7policy.admin_state_up | Body | Boolean | Administrator control status of L7 policy |
| l7policy.action | Body | Enum | Action of L7 policy<br> One of the following: `REDIRECT_TO_POOL`, `REDIRECT_TO_URL`, or `REJECT` |
| l7policy.redirect_pool_id | Body | UUID | Redirect pool ID of L7 policy<br>Applies only if action is `REDIRECT_TO_POOL` |
| l7policy.redirect_url | Body | String | Redirect URL of L7 policy<br>Applies only if action is `REDIRECT_TO_URL` |
| l7policy.redirect_http_code | Body | Integer | - | Redirect HTTP response codes for L7 policies |
| l7policy.position | Body | Integer | Prioritization of L7 policies |


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
| tokenId | Header | String | O | Token ID |
| l7policyId | URL | UUID | O | L7 Policy ID |


#### Response
This API does not return a response body.
















## L7 Rules

### View an ACL List

```
GET /v2.0/lbaas/l7policies/{l7policyId}/rules
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| l7policyId | URL | UUID | O | The L7 policy ID to which the L7 rule belongs |
| id | Query | UUID | - | L7 rule ID to query |
| type | Query | Enum | - | Type of L7 rule to query <br> One of the following: Either `COOKIE`, `FILE_TYPE`, `HEADER`, `HOST_NAME`, or `PATH`  |
| compare_type | Query | Enum | - | Compare type of L7 rule to query<br> One of the following: `CONTAINS`, `ENDS_WITH`, `STARTS_WITH`, `EQUAL_TO`, or `REGEX` |


#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| rules | Body | Array | L7 rule objects |
| rules.tenant_id | Body | String | Tenant ID |
| rules.id | Body | UUID | L7 Rule ID |
| rules.admin_state_up | Body | Boolean | Administrator control status of L7 rule |
| rules.invert | Body | Boolean | Setting invert for matching results |
| rules.key | Body | String | Key used when matching L7 rules<br> Applies only if COOKIE or `HEADER` |
| rules.value | Body | String | Value used when matching L7 rules |
| rules.type | Query | Enum | L7 rule type <br> One of the following: Either `COOKIE`, `FILE_TYPE`, `HEADER`, `HOST_NAME`, or `PATH`  |
| rules.compare_type | Query | Enum | Compare type of L7 rule<br> One of the following: `CONTAINS`, `ENDS_WITH`, `STARTS_WITH`, `EQUAL_TO`, or `REGEX` |

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
### List L7 Rule

```
GET /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| l7policyId | URL | UUID | O | L7 Policy ID |
| l7ruleId | URL | UUID | O | L7 Rule ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| rule | Body | Object | L7 rule object |
| rule.tenant_id | Body | String | Tenant ID |
| rule.id | Body | UUID | L7 Rule ID |
| rule.admin_state_up | Body | Boolean | Administrator control status of L7 rule |
| rule.invert | Body | Boolean | Setting invert for matching results |
| rule.key | Body | String | Key used when matching L7 rules<br> Applies only if COOKIE or `HEADER` |
| rule.value | Body | String | Value used when matching L7 rules |
| rule.type | Query | Enum | L7 rule type <br> One of the following: Either `COOKIE`, `FILE_TYPE`, `HEADER`, `HOST_NAME`, or `PATH`  |
| rule.compare_type | Query | Enum | Compare type of L7 rule<br> One of the following: `CONTAINS`, `ENDS_WITH`, `STARTS_WITH`, `EQUAL_TO`, or `REGEX` |


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
| rule | Body | Object | O | L7 rule object |
| rule.admin_state_up | Body | Boolean | - | Administrator control status of L7 rule |
| rule.invert | Body | Boolean | - | Set to `true`if omitted with invert setting for matching results |
| rule.key | Body | String | - | Key used when matching L7 rules<br> Required if `COOKIE` or `HEADER` |
| rule.value | Body | String | O | Value used when matching L7 rules |
| rule.type | Query | Enum | O | L7 rule type <br> One of the following: Either `COOKIE`, `FILE_TYPE`, `HEADER`, `HOST_NAME`, or `PATH`  |
| rule.compare_type | Query | Enum | O | Compare type of L7 rule<br> One of the following: `CONTAINS`, `ENDS_WITH`, `STARTS_WITH`, `EQUAL_TO`, or `REGEX` |


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
| rule.id | Body | UUID | L7 Rule ID |
| rule.admin_state_up | Body | Boolean | Administrator control status of L7 rule |
| rule.invert | Body | Boolean | Setting invert for matching results |
| rule.key | Body | String | Key used when matching L7 rules<br> Applies only if COOKIE or `HEADER` |
| rule.value | Body | String | Value used when matching L7 rules |
| rule.type | Query | Enum | L7 rule type <br> One of the following: Either `COOKIE`, `FILE_TYPE`, `HEADER`, `HOST_NAME`, or `PATH`  |
| rule.compare_type | Query | Enum | Compare type of L7 rule<br> One of the following: `CONTAINS`, `ENDS_WITH`, `STARTS_WITH`, `EQUAL_TO`, or `REGEX` |


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
| rule | Body | Object | O | L7 rule object |
| rule.admin_state_up | Body | Boolean | - | Administrator control status of L7 rule |
| rule.invert | Body | Boolean | - | Setting invert for matching results |
| rule.key | Body | String | - | Key used when matching L7 rules<br> Applies only if COOKIE or `HEADER` |
| rule.value | Body | String | - | Value used when matching L7 rules |
| rule.type | Query | Enum | - | L7 rule type <br> One of the following: Either `COOKIE`, `FILE_TYPE`, `HEADER`, `HOST_NAME`, or `PATH`  |
| rule.compare_type | Query | Enum | - |Compare type of L7 rule<br> One of the following: `CONTAINS`, `ENDS_WITH`, `STARTS_WITH`, `EQUAL_TO`, or `REGEX` |


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
| rule.id | Body | UUID | L7 Rule ID |
| rule.admin_state_up | Body | Boolean | Administrator control status of L7 rule |
| rule.invert | Body | Boolean | Setting invert for matching results |
| rule.key | Body | String | Key used when matching L7 rules<br> Applies only if COOKIE or `HEADER` |
| rule.value | Body | String | Value used when matching L7 rules |
| rule.type | Query | Enum | L7 rule type <br> One of the following: Either `COOKIE`, `FILE_TYPE`, `HEADER`, `HOST_NAME`, or `PATH`  |
| rule.compare_type | Query | Enum | Compare type of L7 rule<br> One of the following: `CONTAINS`, `ENDS_WITH`, `STARTS_WITH`, `EQUAL_TO`, or `REGEX` |


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
| tokenId | Header | String | O | Token ID |
| l7policyId | URL | UUID | O | L7 Policy ID |
| l7ruleId | URL | UUID | O | L7 Rule ID |


#### Response
This API does not return a response body.





















## Secret

You can call the Secret API by using the `key-manager` type endpoint. For the exact endpoint, see `serviceCatalog` in the response of token issuance.

| Type | Region | Endpoint |
|---|---|---|
| key-manager | Korea (Pangyo) region<br>Korea (Pyeongchon) region<br>Japan region<br>US region |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-key-manager-infrastructure.nhncloudservice.com |

In the API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the NHN Cloud internal usage and might be changed without prior notice.


### List Secrets

Returns the list of secrets.

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
| secrets.secret_type | Body | Enum | Secret type <br>One of `symmetric`, `public`, `private`, `passphrase`, `certificate`, and `opaque` |
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


### Get Secret
Returns the specified secret information.
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
| secret.secret_type | Body | Enum | Secret type <br>One of `symmetric`, `public`, `private`, `passphrase`, `certificate`, and `opaque` |
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
| expiration | Body | Datetime | - | Expiration date: requested in the ISO8601 format |
| algorithm | Body | String | - | Encryption algorithm |
| bit_length | Body | String | - | Length of encryption key |
| mode | Body | String | - | Operating mode of block encryption |
| payload | Body | String | - | Payload of encryption key |
| payload_content_type | Body | String | - | Content type of encryption key payload <br>Must be included to enter payload <br>List of supported content types:  `text/plain`, `application/octet-stream`, `application/pkcs8`, and `application/pkix-cert` |
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
    "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/9b2dcb7b-51fe-4408-a2bb-23da731758a6"
}
```
</p>
</details>

---
### Modify Secret
Enters payload data of the secret that previously had metadata only.
```
PUT /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
Content-Type: {ContentType}
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

You can call the Secret Container API by using the `key-manager` type endpoint. For the exact endpoint, see `serviceCatalog` from the response of token issuance.

| Type | Region | Endpoint |
|---|---|---|
| key-manager | Korea (Pangyo) region<br>Korea (Pyeongchon) region<br>Japan region<br>US region |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-key-manager-infrastructure.nhncloudservice.com |

In the API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the NHN Cloud internal usage and might be changed without prior notice.


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
| containers.type | Body | Enum | Container type<br>One of `generic`, `rsa`, and `certificate` |
| containers.common_name | Body | String | Common Name of the certificate registered in the container<br>Only exposed when container type is `certificate` |
| containers.expiration | Body | Datetime | Expiration date of the certificate registered in the container<br>Only exposed when container type is `certificate`, example: `YYYY-MM-DDThh:mm:ss` |
| total | Body | Integer | Total number of secret containers of a requested query |
| next | Body | String | URL of the next list to the current queried list |
| previous | Body | String | URL of the previous list of the current queried list |



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


### Get Secret Container
Returns the specified secret container information.
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
| created | Body | Datetime | Created time, `YYYY-MM-DDThh:mm:ss` |
| container_ref | Body | String | Container address |
| creator_id | Body | String | User ID creating container |
| secret_refs | Body | Array | List of secrets registered at container |
| secret_refs.secret_ref | Body | String | Secret address |
| secret_refs.name | Body | String| Secret name as specified by container<br>When the container type is `certificate`: specify  `certificate`, `private_key`, `private_key_passphrase`, or `intermediates`<br> When the container type is `rsa`: Specify`private_key`, `private_key_passphrase`, or `public_key` |
| type | Body | Enum | Container type<br>One of `generic`, `rsa`, and `certificate` |
| common_name | Body | String | Common Name of the certificate registered in the container<br>Only exposed when container type is `certificate` |
| expiration | Body | Datetime | Expiration date of the certificate registered in the container<br>Only exposed when container type is `certificate`, example: `YYYY-MM-DDThh:mm:ss` |


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
Delete the specified secret container.
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



































## IP ACL Group

### List IP ACL Groups

Returns a list of IP ACL groups.

```
GET /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name        | Type   | Format   | Required                                              | Description             |
| ----------- | ------ | ------ | ------------------------------------------------- | ---------------- |
| tokenId     | Header | String | O                                                 | Token ID          |
| id          | Query  | String | -                                                 | IP ACL group ID   |
| name        | Query  | String | -                                                 | IP ACL group name |
| description | Query  | String | -                                                 | IP ACL group description |
| action      | Body   | Enum   | Control action of IP ACL group<br>One of `ALLOW`, `DENY` |                  |

#### Response

| Name                                       | Type | Format   | Description                                                   |
| ------------------------------------------ | ---- | ------ | ------------------------------------------------------ |
| ipacl_groups                               | Body | Array  | IP ACL group object list                                  |
| ipacl_groups.ipacl_target_count            | Body | String | Number of targets included in IP ACL group                         |
| ipacl_groups.description                   | Body | String | IP ACL group description                                       |
| ipacl_groups.loadbalancers                 | Body | Object | List of load balancer objects with the IP ACL group applied              |
| ipacl_groups.loadbalancers.loadbalancer_id | Body | String | Load balancer ID                                          |
| ipacl_groups.tenant_id                     | Body | String | Tenant ID                                              |
| ipacl_groups.action                        | Body | Enum   | Control action of IP ACL group<br>One of `ALLOW`, `DENY` |
| ipacl_groups.id                            | Body | UUID   | IP ACL group ID                                         |
| ipacl_groups.name                          | Body | String | IP ACL group name                                         |

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

### Get IP ACL Group

Returns the specified IP ACL group.

```
GET /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name         | Type   | Format   | Required | Description    |
| ------------ | ------ | ------ | ---- | ------- |
| tokenId      | Header | String | O    | Token ID |
| ipaclGroupId | Header | String | O    | IP ACL group ID |

#### Response

| Name                                      | Type | Format   | Description                                              |
| ----------------------------------------- | ---- | ------ | ------------------------------------------------- |
| ipacl_group                               | Body | Object | IP ACL group object                                  |
| ipacl_group.ipacl_target_count            | Body | String | Number of targets included in IP ACL group                    |
| ipacl_group.description                   | Body | String | IP ACL group description                                  |
| ipacl_group.loadbalancers                 | Body | Object | List of load balancer objects with the IP ACL group applied         |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | Load balancer ID                                     |
| ipacl_group.tenant_id                     | Body | String | Tenant ID                                         |
| ipacl_group.action                        | Body | Enum   | Control action of IP ACL group<br>One of `ALLOW`, `DENY` |
| ipacl_group.id                            | Body | UUID   | IP ACL group ID                                    |
| ipacl_group.name                          | Body | String | IP ACL group name                                    |

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

Creates a new IP ACL group.

```
POST /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### Request

| Name                    | Type   | Format   | Required | Description                                              |
| ----------------------- | ------ | ------ | ---- | ------------------------------------------------- |
| tokenId                 | Header | String | O    | Token ID                                           |
| ipacl_group             | Body   | Object | O    | IP ACL group object                                  |
| ipacl_group.description | Body   | String | -    | IP ACL group description                                  |
| ipacl_group.action      | Body   | Enum   | O    | Control action of IP ACL group<br>One of `ALLOW`, `DENY` |
| ipacl_group.name        | Body   | String | -    | IP ACL group name                                    |
| ipacl_group.ipacl_targets | Body | Object | - | `IP ACL target object. When entering a value, the target is also created. |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (if ipacl_targets object has been added) | IP ACL target CIDR<br>Enter a single IP address, or IP range in CIDR format |
| ipacl_group.ipacl_targets.description | Body | String | - | IP ACL target description |

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

| Name                                      | Type | Format   | Description                                              |
| ----------------------------------------- | ---- | ------ | ------------------------------------------------- |
| ipacl_group                               | Body | Object | IP ACL group object                                  |
| ipacl_group.ipacl_target_count            | Body | String | Number of targets included in IP ACL group                    |
| ipacl_group.description                   | Body | String | IP ACL group description                                  |
| ipacl_group.loadbalancers                 | Body | String | List of load balancer objects with the IP ACL group applied         |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | Load balancer ID                                     |
| ipacl_group.tenant_id                     | Body | String | Tenant ID                                         |
| ipacl_group.action                        | Body | Enum   | Control action of IP ACL group<br>One of `ALLOW`, `DENY` |
| ipacl_group.id                            | Body | UUID   | IP ACL group ID                                    |
| ipacl_group.name                          | Body | String | IP ACL group name                                    |

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

Modifies an existing IP ACL group.
ipacl_group.action cannot be changed.
This API may be used to replace the list of sub-IP ACL targets globally.
However, all existing targets belonging to the IP ACL group will be deleted and replaced with the entered target list.
The cidr_address of the entered target must not be duplicated.

```
PUT /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### Request

| Name             | Type   | Format   | Required | Description             |
| ---------------- | ------ | ------ | ---- | ---------------- |
| tokenId          | Header | String | O    | Token ID          |
| ipaclGroupId     | URL    | UUID   | O    | IP ACL group ID   |
| ipacl_group      | Body   | String | O    | IP ACL group object |
| ipacl_group.name | Body   | String | -    | IP ACL group name   |
| ipacl_group.description | Body | String | - | IP ACL Group Description |
| ipacl_group.ipacl_targets | Body | Object | - | `IP ACL target object. When entering a value, the target is also created. |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (if ipacl_targets object has been added) | IP ACL target CIDR<br>Enter a single IP address, or IP range in CIDR format |
| ipacl_group.ipacl_targets.description | Body | String | - | IP ACL target description |


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

| Name                                      | Type | Format   | Description                                              |
| ----------------------------------------- | ---- | ------ | ------------------------------------------------- |
| ipacl_group                               | Body | Object | IP ACL group object                                  |
| ipacl_group.ipacl_target_count            | Body | String | Number of targets included in IP ACL group                    |
| ipacl_group.description                   | Body | String | IP ACL group description                                  |
| ipacl_group.loadbalancers                 | Body | String | List of load balancer objects with the IP ACL group applied         |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | Load balancer ID                                     |
| ipacl_group.tenant_id                     | Body | String | Tenant ID                                         |
| ipacl_group.action                        | Body | Enum   | Control action of IP ACL group<br>One of `ALLOW`, `DENY` |
| ipacl_group.id                            | Body | UUID   | IP ACL group ID                                    |
| ipacl_group.name                          | Body | String | IP ACL group name                                    |

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

Deletes the specified IP ACL Group.

```
DELETE /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

When an IP ACL group is deleted, all sub-IP ACL targets are also deleted.
Rules associated with this IP ACL group are deleted from all load balancers that use the IP ACL group being deleted.

#### Request

This API does not require a request body.

| Name         | Type   | Format   | Required | Description           |
| ------------ | ------ | ------ | ---- | -------------- |
| tokenId      | Header | String | O    | Token ID        |
| ipaclGroupId | URL    | UUID   | O    | IP ACL group ID |

#### Response

This API does not return a response body.

- - -


### Apply IP ACL Group to Load Balancer

Applies an IP ACL group to a load balancer.
The IP ACL target rules included in the group are applied to the load balancer to which the IP ACL group is applied.
Multiple groups can be applied to a load balancer. However, the actions of all groups must be the same.
All IP ACL groups previously applied to the load balancer are deleted and the entered group list are applied.

```
PUT /v2.0/lbaas/loadbalancers/{lb_id}/bind_ipacl_groups
X-auth-Token: {tokenId}
```

#### Request

| Name                                | Type   | Format   | Required | Description                               |
| ----------------------------------- | ------ | ------ | ---- | ---------------------------------- |
| tokenId                             | Header | String | O    | Token ID                            |
| lb_id                               | URL    | UUID   | O    | Load balancer ID                      |
| ipacl_groups_binding                | Body   | Object | O    | IP ACL binding object                 |
| ipacl_groups_binding.ipacl_group_id | Body   | UUID   | O    | IP ACL group ID to be applied to a load balancer |

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
| Name            | Type | Format | Description  |
| --------------- | ---- | ---- | -------------- |
| loadbalancer_id | Body | UUID | Load balancer ID  |
| ipacl_group_id  | Body | UUID | IP ACL group ID |

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

### List IP ACL Targets

Returns a list of IP ACL targets.

```
GET /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name           | Type   | Format   | Required | Description                                                       |
| -------------- | ------ | ------ | ---- | ---------------------------------------------------------- |
| tokenId        | Header | String | O    | Token ID                                                    |
| id             | Query  | String | -    | IP ACL target ID                                             |
| cidr_address   | Query  | String | -    | IP ACL target CIDR<br>Single IP Address or IP range in CIDR format |
| ipacl_group_id | Query  | String | -    | IP ACL group ID                                             |
| description    | Query  | String | -    | IP ACL group description                                           |

#### Response

| Name                         | Type | Format   | Description                       |
| ---------------------------- | ---- | ------ | -------------------------- |
| ipacl_targets                | Body | Array  | IP ACL target information object list |
| ipacl_targets.ipacl_group_id | Body | UUID   | IP ACL group ID             |
| ipacl_targets.tenant_id      | Body | String | Tenant ID                  |
| ipacl_targets.cidr_address   | Body | String | IP ACL target CIDR           |
| ipacl_targets.description    | Body | String | IP ACL target description           |
| ipacl_targets.id             | Body | UUID   | IP ACL target ID             |

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

### Get IP ACL Target

Returns the specified IP ACL target information.

```
GET /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name          | Type   | Format   | Required | Description           |
| ------------- | ------ | ------ | ---- | -------------- |
| tokenId       | Header | String | O    | Token ID        |
| ipaclTargetId | URL    | UUID   | O    | IP ACL target ID |

#### Response

| Name                        | Type | Format   | Description                                                       |
| --------------------------- | ---- | ------ | ---------------------------------------------------------- |
| ipacl_target                | Body | Array  | IP ACL target information object                                       |
| ipacl_target.ipacl_group_id | Body | UUID   | IP ACL group ID                                             |
| ipacl_target.tenant_id      | Body | String | Tenant ID                                                  |
| ipacl_target.cidr_address   | Body | String | IP ACL target CIDR<br>Single IP Address or IP range in CIDR format |
| ipacl_target.description    | Body | String | IP ACL target description                                           |
| ipacl_target.id             | Body | UUID   | IP ACL target ID                                             |

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

Creates IP ACL target.

```
POST /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### Request

| Name                        | Type   | Format   | Required | Description                                                       |
| --------------------------- | ------ | ------ | ---- | ---------------------------------------------------------- |
| tokenId                     | Header | String | O    | Token ID                                                    |
| ipacl_target                | Body   | Object | O    | IP ACL target information object                                       |
| ipacl_target.ipacl_group_id | Body   | UUID   | O    | IP ACL group ID                                             |
| ipacl_target.cidr_address   | Body   | String | O    | IP ACL target CIDR<br>Single IP Address or IP range in CIDR format |
| ipacl_target.description    | Body   | String | -    | IP ACL target description                                           |

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

| Name                        | Type | Format   | Description                                                       |
| --------------------------- | ---- | ------ | ---------------------------------------------------------- |
| ipacl_target                | Body | Object | IP ACL target information object                                       |
| ipacl_target.ipacl_group_id | Body | UUID   | IP ACL group ID                                             |
| ipacl_target.tenant_id      | Body | String | Tenant ID                                                  |
| ipacl_target.cidr_address   | Body | String | IP ACL target CIDR<br>Single IP Address or IP range in CIDR format |
| ipacl_target.description    | Body | String | IP ACL target description                                           |
| ipacl_target.id             | Body | UUID   | IP ACL target ID                                             |

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

Modifies an existing IP ACL target.
Only description can be changed.

```
PUT /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### Request

| Name                     | Type   | Format   | Required | Description                  |
| ------------------------ | ------ | ------ | ---- | --------------------- |
| tokenId                  | Header | String | O    | Token ID               |
| ipaclTargetId            | URL    | UUID   | O    | IP ACL target ID        |
| ipacl_target             | Body   | Object | O    | IP ACL target information object  |
| ipacl_target.description | Body   | String | -    | IP ACL target description      |

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

| Name                        | Type | Format   | Description                                                       |
| --------------------------- | ---- | ------ | ---------------------------------------------------------- |
| ipacl_target                | Body | Object | IP ACL target information object                                       |
| ipacl_target.ipacl_group_id | Body | UUID   | IP ACL group ID                                             |
| ipacl_target.tenant_id      | Body | String | Tenant ID                                                  |
| ipacl_target.cidr_address   | Body | String | IP ACL target CIDR<br>Single IP Address or IP range in CIDR format |
| ipacl_target.description    | Body | String | IP ACL target description                                           |
| ipacl_target.id             | Body | UUID   | IP ACL target ID                                             |

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

Deletes the specified load balancer.

```
DELETE /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name          | Type   | Format   | Required | Description           |
| ------------- | ------ | ------ | ---- | -------------- |
| tokenId       | Header | String | O    | Token ID        |
| ipaclTargetId | URL    | UUID   | O    | IP ACL target ID |

#### Response

This API does not return a response body.

- - -

