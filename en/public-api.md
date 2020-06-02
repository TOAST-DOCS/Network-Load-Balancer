## Network > Load Balancer > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

로드 밸런서, 리스너, 풀, 헬스 모니터, 멤버 API는 `network` 타입 엔드포인트를 이용합니다. 시크릿, 시크릿 컨테이너 API는 `key-manager` 타입 엔드포인트를 이용해 호출합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| network | 한국(판교) 리전<br>일본 리전 | https://kr1-api-network.infrastructure.cloud.toast.com<br>https://jp1-api-network.infrastructure.cloud.toast.com |
| key-manager | 한국(판교) 리전<br>일본 리전 | https://kr1-api-key-manager.infrastructure.cloud.toast.com<br>https://jp1-api-key-manager.infrastructure.cloud.toast.com |


API 응답에 가이드에 명시되지 않은 필드가 노출될 수 있습니다. 이런 필드는 TOAST 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

## 로드 밸런서

### 로드 밸런서 목록 보기

```
GET /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 로드 밸런서 ID |
| name | Query | String | - | 조회할 로드 밸런서 이름 |
| provisioning_status | Query | Enum | - | 조회할 로드 밸런서의 프로비저닝 상태 |
| description | Query | String | - | 조회할 로드 밸런서의 설명 |
| vip_address | Query | String | - | 조회할 로드 밸런서의 IP |
| vip_port_id | Query | UUID | - | 조회할 로드 밸런서의 포트 ID |
| vip_subnet_id | Query | UUID | - | 조회할 로드 밸런서의 서브넷 ID |
| operating_status | Query | Enum | - | 조회할 로드 밸런서의 운영 상태 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancers | Body | Array | 로드 밸런서 정보 객체 목록 |
| loadbalancers.description | Body | String | 로드 밸런서 설명 |
| loadbalancers.provisioning_status | Body | Enum | 로드 밸런서 프로비저닝 상태 |
| loadbalancers.tenant_id | Body | String | 테넌트 ID |
| loadbalancers.provider | Body | String | 로드 밸런서 프로바이더(공급자) |
| loadbalancers.name | Body | String | 로드 밸런서 이름 |
| loadbalancers.listeners | Body | Object | 로드 밸런서 리스너 객체 목록 |
| loadbalancers.listeners.id | Body | UUID | 리스너 ID |
| loadbalancers.vip_address | Body | String | 로드 밸런서 IP |
| loadbalancers.vip_port_id | Body | UUID | 로드 밸런서 포트 ID |
| loadbalancers.vip_subnet_id | Body | UUID | 로드 밸런서 서브넷 ID |
| loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| loadbalancers.operating_status | Body | Enum | 로드 밸런서 운영 상태 |
| loadbalancers.admin_state_up | Body | Boolean | 로드 밸런서 관리자 제어 상태 |

<details><summary>예시</summary>
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


### 로드 밸런서 보기

```
GET /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드 밸런서 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드 밸런서 정보 객체 |
| loadbalancer.description | Body | String | 로드 밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드 밸런서 프로비저닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드 밸런서 프로바이더(공급자) |
| loadbalancer.name | Body | String | 로드 밸런서 이름 |
| loadbalancer.listeners | Body | Object | 로드 밸런서 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드 밸런서 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드 밸런서 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드 밸런서 서브넷 ID |
| loadbalancer.id | Body | UUID | 로드 밸런서 ID |
| loadbalancer.operating_status | Body | Enum | 로드 밸런서 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드 밸런서 관리자 제어 상태 |


<details><summary>예시</summary>
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
### 로드 밸런서 생성하기

```
POST /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancer | Body | Object | - | 로드 밸런서 정보 객체 |
| loadbalancer.name | Body | String | - | 로드 밸런서 이름 |
| loadbalancer.description | Body | String | - | 로드 밸런서 설명 |
| loadbalancer.vip_subnet_id | Body | UUID | O | 로드 밸런서의 서브넷 ID |
| loadbalancer.vip_address | Body | String | - | 로드 밸런서의 IP |
| loadbalancer.admin_state_up | Body | Boolean | - | 로드 밸런서 관리자 제어 상태로 생략하면 `true`로 설정됨 |




<details><summary>예시</summary>

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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드 밸런서 정보 객체 |
| loadbalancer.description | Body | String | 로드 밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드 밸런서 프로비저닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드 밸런서 프로바이더(공급자) 이름 |
| loadbalancer.name | Body | String | 로드 밸런서 이름 |
| loadbalancer.listeners | Body | Object | 로드 밸런서 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드 밸런서 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드 밸런서 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드 밸런서 서브넷 ID |
| loadbalancer.id | Body | UUID | 로드 밸런서 ID |
| loadbalancer.operating_status | Body | Enum | 로드 밸런서 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드 밸런서 관리자 제어 상태 |


<details><summary>예시</summary>

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
### 로드 밸런서 수정하기

```
PUT /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드 밸런서 ID |
| loadbalancer | Body | Object | O | 로드 밸런서 정보 객체 |
| loadbalancer.name | Body | String | - | 로드 밸런서 이름 |
| loadbalancer.description | Body | String | - | 로드 밸런서 설명 |
| loadbalancer.admin_state_up | Body | Boolean | - | 로드 밸런서의 관리자 제어 상태 |

<details><summary>예시</summary>

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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드 밸런서 정보 객체 |
| loadbalancer.description | Body | String | 로드 밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드 밸런서 프로비저닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드 밸런서 프로바이더(공급자) 이름 |
| loadbalancer.name | Body | String | 로드 밸런서 이름 |
| loadbalancer.listeners | Body | Object | 로드 밸런서 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드 밸런서 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드 밸런서 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드 밸런서 서브넷 ID |
| loadbalancer.id | Body | UUID | 로드 밸런서 ID |
| loadbalancer.operating_status | Body | Enum | 로드 밸런서 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드 밸런서 관리자 제어 상태 |


<details><summary>예시</summary>

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
### 로드 밸런서 삭제하기

```
DELETE /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드 밸런서 ID |


#### 응답
이 API는 응답 본문을 반환하지 않습니다.





















## 리스너
### 리스너 목록 보기

```
GET /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| default_pool_id | Query | UUID | - | 리스너에 등록된 풀 ID |
| protocol | Query | Enum | - | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| description | Query | String | - | 리스너 설명 |
| name | Query | String | - | 리스너 이름 |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| connection_limit | Query | Integer | - | 리스너의 connection limit |
| keepalive_timeout | Query | Integer | - | 리스너의 keepalive timeout |
| protocol_port | Query | Integer | - | 리스너 포트 번호 |
| id | Query | UUID | - | 리스너 ID |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listeners | Body | Array | 리스너 정보 객체 목록 |
| listeners.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listeners.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listeners.description | Body | String | 리스너 설명 |
| listeners.name | Body | String | 리스너 이름 |
| listeners.loadbalancers | Body | Array | 리스너가 등록된 로드 밸런서 객 목록 |
| listeners.loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| listeners.tenant_id | Body | String | 테넌트 ID |
| listeners.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| listeners.connection_limit | Body | Integer | 리스너의 connection limit |
| listeners.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listeners.default_tls_container_ref | Body | String| key-manager에 등록된 TLS 인증서 경로 |
| listeners.sni_container_refs | Body | Array | key-manager에 등록된 SNI 인증서 경로 목록 |
| listeners.protocol_port | Body | Integer | 리스너 포트 |
| listeners.id | Body | String| 리스너 ID |


<details><summary>예시</summary>
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


### 리스너 보기

```
GET /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listenerId | URL | UUID | O | 리스너 ID |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listener | Body | Object | 리스너 정보 객체 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listener.description | Body | String | 리스너 설명 |
| listener.name | Body | String | 리스너 이름 |
| listener.loadbalancers | Body | Array | 리스너가 등록된 로드 밸런서 객체 목록 |
| listener.loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| listener.tenant_id | Body | String | 테넌트 ID |
| listener.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String| key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | key-manager에 등록된 SNI 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | UUID | 리스너 ID |


<details><summary>예시</summary>
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
### 리스너 생성하기

```
POST /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listener | Body | Object | O | 리스너 정보 객체 |
| listener.protocol | Body | Enum | O | 리스너 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listener.description | Body | String | - | 리스너 설명 |
| listener.name | Body | String | - | 리스너 이름 |
| listener.loadbalancer_id | Body | UUID | O | 로드 밸런서 ID |
| listener.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| listener.connection_limit | Body |  Integer | - | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | - | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String | - | key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | - | key-manager에 등록된 SNI 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | O | 리스너 포트 |


<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listener | Body | Object | 리스너 정보 객체 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listener.description | Body | String | 리스너 설명 |
| listener.name | Body | String | 리스너 이름 |
| listener.loadbalancers | Body | Array | 리스너가 등록된 로드 밸런서 객체 목록 |
| listener.loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| listener.tenant_id | Body | String | 테넌트 ID |
| listener.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String | key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | key-manager에 등록된 SNI 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | UUID | 리스너 ID |


<details><summary>예시</summary>
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
### 리스너 수정하기

```
PUT /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listenerId | URL | UUID | O | 리스너 ID |
| listener | Body | Object | O | 리스너 정보 객체 |
| listener.description | Body | String | - | 리스너 설명 |
| listener.name | Body | String| - | 리스너 이름 |
| listener.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| listener.connection_limit | Body |  Integer | - | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | - | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String | - | key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | - | key-manager에 등록된 SNI 인증서 경로 목록 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listener | Body | Object | 리스너 정보 객체 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| listener.description | Body | String | 리스너 설명 |
| listener.name | Body | String | 리스너 이름 |
| listener.loadbalancers | Body | Array | 리스너가 등록된 로드 밸런서 객체 목록 |
| listener.loadbalancers.id | Body | UUID | 로드 밸런서 ID |
| listener.tenant_id | Body | String | 테넌트 ID |
| listener.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.default_tls_container_ref | Body | String | key-manager에 등록된 TLS 인증서 경로 |
| listener.sni_container_refs | Body | Array | key-manager에 등록된 SNI 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | UUID | 리스너 ID |


<details><summary>예시</summary>
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
### 리스너 삭제하기
지정한 리스너를 삭제합니다.
```
DELETE /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listenerId | URL | UUID | O | 리스너 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.













## 풀
### 풀 목록 보기

```
GET /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 풀 ID |
| name | Query | String | - | 풀 이름 |
| lb_algorithm | Query | Enum | - | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP`중 하나 |
| protocol | Query | Enum | - | 멤버의 프로토콜 |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| healthmonitor_id | Query | UUID | - | 풀의 헬스 모니터 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pools | Body | Array | 풀 정보 객체 목록 |
| pools.lb_algorithm | Body | Enum | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP`중 하나 |
| pools.protocol | Body | Enum | 멤버의 프로토콜 |
| pools.description | Body | String | 풀 설명 |
| pools.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pools.tenant_id | Body | String | 테넌트 ID |
| pools.session_persistence | Body | Object | 풀의 세션 지속성 객체 |
| pools.session_persistence.type | Body | Enum | 세션 지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드 밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다. |
| pools.healthmonitor_id | Body | String | 헬스 모니터 ID |
| pools.listeners | Body | Array | 풀이 등록된 리스너 객체 목록 |
| pools.listeners.id | Body | String | 리스너 ID |
| pools.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pools.members.id | Body | String | 멤버 ID |
| pools.id | Body | UUID | 풀 ID |
| pools.name | Body | String | 풀 이름 |

<details><summary>예시</summary>
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


### 풀 보기

```
GET /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pool | Body | Object | 풀 정보 객체 |
| pool.lb_algorithm | Body | Enum | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP`중 하나 |
| pool.protocol | Body | Enum | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pool.member_port | Body | Integer | 멤버의 포트<br> 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트값 |
| pools.session_persistence | Body | Object | 풀의 세션 지속성 객체 |
| pools.session_persistence.type | Body | Enum | 세션 지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드 밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다. |
| pool.healthmonitor_id | Body | UUID | 헬스 모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 목록 |
| pool.listeners.id | Body | UUID | 리스너 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | UUID | 멤버 ID |
| pool.id | Body | UUID | 풀 ID |
| pool.name | Body | String | 풀 이름 |

<details><summary>예시</summary>
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
### 풀 생성하기

```
POST /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| pool | Body | Object | O | 풀 정보 객체 |
| pool.listener_id | Body | UUID | O | 풀이 등록될 리스너 ID |
| pool.lb_algorithm | Body | Enum | O | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP`중 하나 |
| pool.protocol | Body | Enum | O | 멤버의 프로토콜 |
| pool.description | Body | String | - | 풀 설명 |
| pool.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| pool.member_port | Body | Integer | - | 멤버의 수신 포트<br>트래픽을 이 포트로 전달합니다.<br>기본 값은 -1 입니다. |
| pools.session_persistence | Body | Object | - | 풀의 세션 지속성 객체 |
| pools.session_persistence.type | Body | Enum | - | 세션 지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드 밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | - | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다. |
| pool.name | Body | String | - | 풀 이름 |



<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pool | Body | Object | 풀 정보 객체 |
| pool.lb_algorithm | Body | Enum | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP`중 하나 |
| pool.protocol | Body | Enum | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pool.session_persistence | Body | Enum | 세션 지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드 밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없으며, PROTOCOL에 따라 사용할 수 있는 다릅니다. |
| pool.healthmonitor_id | Body | String | 헬스 모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 목록 |
| pool.listeners.id | Body | UUID | 리스너 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | UUID | 멤버 ID |
| pool.id | Body | UUID | 풀 ID |
| pool.name | Body | String | 풀 이름 |

<details><summary>예시</summary>
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
### 풀 수정하기

```
PUT /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```


#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 풀 ID |
| pool | Body | Object | O | 풀 정보 객체 |
| pool.lb_algorithm | Body | Enum | - | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP`중 하나 |
| pool.description | Body | String | - |  풀 설명 |
| pool.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| pools.session_persistence | Body | Object | - | 풀의 세션 지속성 객체 |
| pools.session_persistence.type | Body | Enum | - | 세션 지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드 밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | - | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다. |
| pool.name | Body | String | - | 풀 이름 |



<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pool | Body | Object | 풀 정보 객체 |
| pool.lb_algorithm | Body | Enum | 풀의 로드 밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_IP`중 하나 |
| pool.protocol | Body | Enum | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pools.session_persistence | Body | Object | 풀의 세션 지속성 객체 |
| pools.session_persistence.type | Body | Enum | 세션 지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br>로드 밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션 지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다. |
| pool.healthmonitor_id | Body | UUID | 헬스 모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 목록 |
| pool.listeners.id | Body | UUID | 리스너 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | UUID | 멤버 ID |
| pool.id | Body | UUID | 풀 ID |
| pool.name | Body | String | 풀 이름 |

<details><summary>예시</summary>
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
### 풀 삭제하기
지정한 풀을 삭제합니다.
```
DELETE /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 풀 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.




























## 헬스 모니터
### 헬스 모니터 목록 보기

```
GET /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 헬스 모니터 ID |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| delay | Query | Integer | - | 상태 확인 간격(초) |
| expected_codes | Query | String | - | 정상 상태로 간주할 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| max_retries | Query | Integer | - | 최대 재시도 횟수 |
| http_method | Query | Enum | - | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| timeout | Query | Integer | - | 상태 확인 응답 대기 시간(초) |
| url_path | Query | String | - | 상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| type | Query | Enum | - | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |



#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitors | Body | Array | 헬스 모니터 정보 객체 목록 |
| healthmonitors.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| healthmonitors.delay | Body | Integer | 상태 확인 간격(초) |
| healthmonitors.expected_codes | Body | String | 정상 상태로 간주할 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitors.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitors.http_method | Body | Enum | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitors.timeout | Body | Integer | 상태 확인 응답 대기 시간(초) |
| healthmonitors.pools | Body | Array | 헬스 모니터가 연결된 풀 객체 목록 |
| healthmonitors.pools.id | Body | UUID | 풀 ID |
| healthmonitors.url_path | Body | String | 상태 확인 요청 URL<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitors.type | Body | Enum | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitors.id | Body | UUID | 헬스 모니터 ID |


<details><summary>예시</summary>
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


### 헬스 모니터 보기

```
GET /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthMonitorId | URL | UUID | O | 헬스 모니터 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitor | Body | Object | 헬스 모니터 정보 객체 |
| healthmonitor.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| healthmonitor.delay | Body | Integer | 상태 확인 간격(초) |
| healthmonitors.expected_codes | Body | String | 정상 상태로 간주할 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간(초) |
| healthmonitor.pools | Body | Array | 헬스 모니터가 연결된 풀 객체 목록 |
| healthmonitor.pools.id | Body | UUID | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.type | Body | Enum | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitor.id | Body | UUID | 헬스 모니터 ID |


<details><summary>예시</summary>
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
### 헬스 모니터 생성하기

```
POST /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthmonitor | Body | Object | O | 헬스 모니터 정보 객체 |
| healthmonitor.pool_id | Body | UUID | O | 헬스 모니터가 연결될 풀 ID |
| healthmonitor.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| healthmonitor.health_check_port | Body | Integer | - | 헬스체크의 대상이 되는 멤버 포트 |
| healthmonitor.delay | Body | Integer | O | 상태 확인 간격(초) |
| healthmonitor.expected_codes | Body | String | - | 정상 상태로 간주할 멤버의 HTTP 응답 코드. 생략하면 200으로 설정됨.<br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.max_retries | Body | Integer | O | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | - | 상태 확인에 사용할 HTTP Method. 생략하면 `GET`이 사용됨. <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.timeout | Body | Integer | O | 상태 확인 응답 대기 시간(초) |
| healthmonitor.url_path | Body | String | - | 상태 확인 요청 URL. 생략하면 `/`가 설정됨. <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.type | Body | Enum  | O | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |




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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitor | Body | Object | 헬스 모니터 정보 객체 |
| healthmonitor.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| healthmonitor.delay | Body | Integer | 상태 확인 간격(초) |
| healthmonitor.expected_codes | Body | String | 정상 상태로 간주할 멤버의 HTTP 응답 코드. 생략하면 200으로 설정됨.<br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간(초) |
| healthmonitor.pools | Body | Array | 헬스 모니터가 연결된 풀 객체 목록 |
| healthmonitor.pools.id | Body | UUID | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.type | Body | Enum | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitor.id | Body | UUID | 헬스 모니터 ID |


<details><summary>예시</summary>
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
### 헬스 모니터 수정하기

```
PUT /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthmonitorId | URL | UUID | O | 헬스 모니터 ID |
| healthmonitor | Body | Object | O | 헬스 모니터 정보 객체 |
| healthmonitor.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |
| healthmonitor.delay | Body | Integer | - | 상태 확인 간격(초) |
| healthmonitor.expected_codes | Body | String | - | 정상 상태로 간주할 멤버의 HTTP 응답 코드<br>단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.max_retries | Body | Integer | - | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | - | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.timeout | Body | Integer | - | 상태 확인 응답 대기 시간(초) |
| healthmonitor.url_path | Body | String | - | 상태 확인 요청 URL<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitor | Body | Object | 헬스 모니터 정보 객체 |
| healthmonitor.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| healthmonitor.delay | Body | Integer | 상태 확인 간격(초) |
| healthmonitor.expected_codes | Body | String | 정상 상태로 간주할 멤버의 HTTP 응답 코드<br>단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | Enum | 상태 확인에 사용할 HTTP Method <br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간(초) |
| healthmonitor.pools | Body | Array | 헬스 모니터가 연결된 풀 객체 목록 |
| healthmonitor.pools.id | Body | UUID | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용 |
| healthmonitor.type | Body | Enum | 상태 확인에 사용할 프로토콜. `TCP`, `HTTP`, `HTTPS` 중 하나 |
| healthmonitor.id | Body | UUID | 헬스 모니터 ID |


<details><summary>예시</summary>
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
### 헬스 모니터 삭제하기

```
DELETE /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthMonitorId | URL | UUID | O | 헬스 모니터 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.






























## 멤버
### 멤버 목록 보기

```
GET /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| id | Query | UUID | - | 멤버 ID |
| weight | Query | Integer | - | 멤버 가중치 |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| subnet_id | Query | UUID | - | 멤버의 서브넷 ID |
| tenant_id | Query | String | - | 테넌트 ID |
| address | Query | String | - | 멤버의 IP 주소 |
| protocol_port | Query | Integer | - | 멤버의 포트 |
| operating_status | Query | Enum | - | 멤버의 운영 상태 |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| members | Body | Array | 멤버 정보 객체 목록 |
| members.weight | Body | Integer | 멤버 가중치 |
| members.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| members.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| members.tenant_id | Body | String | 테넌트 ID |
| members.address | Body | String | 멤버의 IP 주소 |
| members.protocol_port | Body | Integer | 멤버의 포트 |
| members.id | Body | UUID | 멤버 ID |
| members.operating_status | Body | Enum | 멤버의 운영 상태 |

<details><summary>예시</summary>
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


### 멤버 보기

```
GET /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| memberId | URL | UUID | O | 멤버 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| member | Body | Object | 멤버 정보 객체 |
| member.weight | Body | Integer | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| member.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| member.tenant_id | Body | String | 테넌트 ID |
| member.address | Body | String | 멤버의 IP 주소 |
| member.protocol_port | Body | Integer | 멤버의 포트 |
| member.id | Body | UUID | 멤버 ID |
| member.operating_status | Body | Enum | 멤버의 운영 상태 |

<details><summary>예시</summary>
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
### 멤버 생성하기

```
POST /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| member | Body | Object | O | 멤버 정보 객체 |
| member.weight | Body | Integer | - | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | -| 관리자 제어 상태 |
| member.subnet_id | Body | UUID | O | 멤버의 서브넷 ID |
| member.address | Body | String | O | 멤버의 IP 주소 |
| member.protocol_port | Body | Integer | O | 멤버의 포트 |


<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| member | Body | Object | 멤버 정보 객체 |
| member.weight | Body | Integer | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| member.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| member.tenant_id | Body | String | 테넌트 ID |
| member.address | Body | String | 멤버의 IP 주소 |
| member.protocol_port | Body | Integer | 멤버의 포트 |
| member.id | Body | UUID | 멤버 ID |
| member.operating_status | Body | Enum | 멤버의 운영 상태 |

<details><summary>예시</summary>
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
### 멤버 수정하기

```
PUT /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| memberId | URL | UUID | O | 멤버 ID |
| member | Body | Object | O | 멤버 정보 객체 |
| member.weight | Body | Integer | - | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | - | 관리자 제어 상태 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| member | Body | Object | 멤버 정보 객체 |
| member.weight | Body | Integer | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| member.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| member.tenant_id | Body | String | 테넌트 ID |
| member.address | Body | String | 멤버의 IP 주소 |
| member.protocol_port | Body | Integer | 멤버의 포트 |
| member.id | Body | UUID | 멤버 ID |
| member.operating_status | Body | Enum | 멤버의 운영 상태 |

<details><summary>예시</summary>
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
### 멤버 삭제하기

```
DELETE /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 멤버가 속한 풀 ID |
| memberId | URL | UUID | O | 멤버 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

























## 시크릿

시크릿 API는 `key-manager` 타입 앤드포인트를 이용하여 호출합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| key-manager | 한국(판교) 리전<br>일본 리전 | https://kr1-api-key-manager.infrastructure.cloud.toast.com<br>https://jp1-api-key-manager.infrastructure.cloud.toast.com |

API 응답에 가이드에 명시되지 않은 필드가 노출될 수 있습니다. 이런 필드는 TOAST 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.


### 시크릿 목록 보기

시크릿 목록을 반환합니다.

```
GET /v1/secrets
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| offset | Query | Integer | - | 응답 목록의 오프셋, 기본값: 0 |
| limit | Query | Integer| - | 응답 목록에 노출할 최대 갯수, 기본값: 10 |
| name | Query | String | - | 시크릿 이름 |
| alg | Query | String | - | 시크릿 알고리즘 |
| mode | Query | String| - | 블록 암호 운용 방식 |
| bits | Query | Integer| - | 암호화 키 길이 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| secrets | Body | Array | 시크릿 객체 목록 |
| secrets.secret_ref | Body | String | 시크릿 주소<br>`<barbican endpoint>/v1/secrets/<secret id>` 형식 |
| secrets.secret_type | Body | Enum | 시크릿 타입 <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque`중 하나 |
| secrets.status | Body | Enum | 시크릿 상태 |
| secrets.content_types | Body | Array | 시크릿 페이로드의 콘텐츠 타입 목록 |
| secrets.content_types.default | Body | String | 콘텐츠 타입 기본값 |
| secrets.creator_id | Body | String | 시크릿을 생성한 사용자 ID |
| secrets.mode | Body | String | 블록 암호 운용 방식. 사용자 입력 메타데이터 |
| secrets.algorithm | Body | String | 암호화 알고리즘. 사용자 입력 메타데이터 |
| secrets.bit_length | Body | Integer | 암호화 키 길이. 사용자 입력 메타데이터 |
| secrets.expiration | Body | Datetime | 만료일. 사용자 입력 메타데이터 <br>`YYYY-MM-DDThh:mm:ss`<br> 만료일이 지난 시크릿은 자동으로 삭제 처리됨 |
| secrets.name| Body | String | 시크릿 이름 |
| secrets.created | Body | Datetime | 생성 시간 <br> `YYYY-MM-DDThh:mm:ss` |
| secrets.updated | Body | Datetime | 수정 시간 <br> `YYYY-MM-DDThh:mm:ss` |
| total | Body | Integer | 요청 쿼리의 총 시크릿 개수 |
| next | Body | String | 현재 조회된 목록의 다음 목록 URL |
| previous | Body | String | 현재 조회된 목록의 이전 목록 URL |

<details><summary>예시</summary>
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


### 시크릿 보기
지정한 시크릿 정보를 반환합니다.
```
GET /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| secretId | URL | UUID | O | 시크릿 ID |

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| secret | Body | Object | 시크릿 객체 |
| secret.secret_ref | Body | String | 시크릿 주소<br>`<barbican endpoint>/v1/secrets/<secret id>` 형식 |
| secret.secret_type | Body | Enum | 시크릿 타입 <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque`중 하나 |
| secret.status | Body | Enum | 시크릿 상태 |
| secret.content_types | Body | Array | 시크릿 페이로드의 콘텐츠 타입 목록 |
| secret.content_types.default | Body | String | 콘텐츠 타입 기본값 |
| secret.creator_id | Body | String | 시크릿을 생성한 사용자 ID |
| secret.mode | Body | String | 블록 암호 운용 방식. 사용자 입력 메타데이터 |
| secret.algorithm | Body | String | 암호화 알고리즘. 사용자 입력 메타데이터 |
| secret.bit_length | Body | Integer | 암호화 키 길이. 사용자 입력 메타데이터 |
| secret.expiration | Body | Datetime | 만료일. 사용자 입력 메타데이터 <br>`YYYY-MM-DDThh:mm:ss`<br> 만료일이 지난 시크릿은 자동으로 삭제됨 |
| secret.name| Body | String | 시크릿 이름 |
| secret.created | Body | Datetime | 생성 시간 <br> `YYYY-MM-DDThh:mm:ss` |
| secret.updated | Body | Datetime | 수정 시간 <br> `YYYY-MM-DDThh:mm:ss` |

<details><summary>예시</summary>
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
### 시크릿 생성하기
새로운 시크릿을 생성합니다.
```
POST /v1/secrets
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| name | Body | String | - | 시크릿 이름 |
| expiration | Body | Datetime | - | 만료일. ISO8601형식으로 요청 |
| algorithm | Body | String | - | 암호화 알고리즘 |
| bit_length | Body | String | - | 암호화 키 길이|
| mode | Body | String | - | 블록 암호 운용 방식 |
| payload | Body | String | - | 암호화 키 페이로드 |
| payload_content_type | Body | String | - | 암호화 키 페이로드 콘텐츠 타입<br> payload를 입력할 시 필수로 입력해야 함 <br>지원하는 콘텐츠 타입 목록: `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` |
| payload_content_encoding | Body | Enum | - | 암호화 키 페이로드 인코딩 방식 <br>payload_content_type이 text/plain이 아닌 경우 필수로 입력해야 함<br> `base64` 만 지원 |
| secret_type | Body | Enum | - | 시크릿 타입 <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque`중 하나 |



<details><summary>예시</summary>
메타데이터만 생성
```json
{
    "name": "example key",
    "expiration": "2025-12-31T00:00:00.000000Z",
    "algorithm": "example-algorithm",
    "bit_length": 256,
    "mode": "example-mode"
}
```

text로 페이로드 전송
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

base64로 페이로드 전송
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

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| secret_ref | Body | String | 시크릿 주소<br>`<barbican endpoint>/v1/secrets/<secret id>` 형식 |

<details><summary>예시</summary>
<p>

```json
{
    "secret_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/secrets/9b2dcb7b-51fe-4408-a2bb-23da731758a6"
}
```
</p>
</details>

---
### 시크릿 수정하기
기존에 메타데이터만 입력한 시크릿의 페이로드 데이터를 입력합니다.
```
PUT /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
Content-Type: {ConetentType}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| secretId | URL | UUID | O | 시크릿 ID |
| ContentType| Header | Enum | O | `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` 중 하나<br> 생략시 `text/plain` 으로 설정됨 |
| payload | Body | String | O | 암호화 키 페이로드 |

<details><summary>예시</summary>
```
{
	"payload": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANQE .... nyxm\n-----END PRIVATE KEY-----\n"
}
```
</details>

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

---
### 시크릿 삭제하기
지정한 시크릿을 삭제합니다.
```
DELETE /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| secretId | URL | UUID | O | 시크릿 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.






































## 시크릿 컨테이너

시크릿 컨테이너 API는 `key-manager` 타입 앤드포인트를 이용하여 호출합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| key-manager | 한국(판교) 리전<br>일본 리전 | https://kr1-api-key-manager.infrastructure.cloud.toast.com<br>https://jp1-api-key-manager.infrastructure.cloud.toast.com |

API 응답에 가이드에 명시되지 않은 필드가 노출될 수 있습니다. 이런 필드는 TOAST 내부 용도로 사용되며 사전 공지없이 변경될 수 있으므로 사용하지 않습니다.


### 시크릿 컨테이너 목록 보기

시크릿 컨테이너 목록을 반환합니다.

```
GET /v1/containers
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| offset | Query | Integer | - | 응답 목록의 오프셋, 기본값: 0 |
| limit | Query | Integer | - | 응답 목록에 노출할 최대 갯수, 기본값: 10 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| containers | Body | Array | 컨테이너 객체 목록 |
| containers.status | Body | Enum | 컨테이너 상태 |
| containers.updated | Body | Datetime | 수정 시간 `YYYY-MM-DDThh:mm:ss` |
| containers.name | Body | String | 컨테이너 이름 |
| containers.consumers | Body | Array | 컨슈머 목록 |
| containers.consumers.URL | Body | String | 컨슈머 URL |
| containers.consumers.name | Body | String | 컨슈머 이름 |
| containers.created | Body | Datetime | 생성 시간  `YYYY-MM-DDThh:mm:ss`|
| containers.container_ref | Body | String | 컨테이너 주소 |
| containers.creator_id | Body | String | 컨테이너를 생성한 사용자 ID |
| containers.secret_refs | Body | Array | 시크릿 목록 |
| containers.secret_refs.secret_ref | Body | String | 시크릿 주소 |
| containers.secret_refs.name | Body | String | 컨테이너가 지정한 시크릿 이름<br> 컨테이너 타입이 `certificate`인 경우: `certificate`, `private_key`, `private_key_passphrase`, `intermediates`로 지정<br> 컨테이너 타입이 `rsa`인 경우: `private_key`, `private_key_passphrase`, `public_key`로 지정 |
| containers.type | Body | Enum | 컨테이너 타입<br> `generic`, `rsa`, `certificate` 중 하나|
| total | Body | Integer | 요청 쿼리의 시크릿 컨테이너의 총 갯수 |
| next | Body | String | 현재 조회된 목록의 다음 목록 URL |
| previous | Body | String | 현재 조회된 목록의 이전 목록 URL |



<details><summary>예시</summary>
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


### 시크릿 컨테이너 보기
지정한 시크릿 컨테이너 정보를 반환합니다.
```
GET /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| containerId | URL | UUID | O | 시크릿 컨테이너 ID |

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| status | Body | Enum | 컨테이너 상태 |
| updated | Body | Datetime | 수정 시간 `YYYY-MM-DDThh:mm:ss` |
| name | Body | String | 컨테이너 이름 |
| consumers | Body | Array | 컨슈머 목록 |
| consumers.URL | Body | String | 컨슈머 URL |
| consumers.name | Body | String | 컨슈머 이름 |
| created | Body | Datetime | 생성 시간  `YYYY-MM-DDThh:mm:ss`|
| container_ref | Body | String | 컨테이너 주소 |
| creator_id | Body | String | 컨테이너를 생성한 사용자 ID |
| secret_refs | Body | Array | 컨테이너에 등록한 시크릿 목록 |
| secret_refs.secret_ref | Body | String | 시크릿 주소 |
| secret_refs.name | Body | String| 컨테이너가 지정한 시크릿 이름<br>컨테이너 타입이 `certificate`인 경우: `certificate`, `private_key`, `private_key_passphrase`, `intermediates`로 지정<br> 컨테이너 타입이 `rsa`인 경우: `private_key`, `private_key_passphrase`, `public_key`로 지정 |
| type | Body | Enum | 컨테이너 타입<br> `generic`, `rsa`, `certificate` 중 하나 |


<details><summary>예시</summary>

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
### 시크릿 컨테이너 생성하기
새로운 시크릿 컨테이너 생성합니다.
```
POST /v1/containers
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| type | Body | Enum | O | 컨테이너 타입<br> `generic`, `rsa`, `certificate` 중 하나 |
| name | Body | String | - | 컨테이너 이름 |
| secret_refs | Body | Array | - | 컨테이너에 등록할 시크릿 목록 |
| secret_refs.secret_ref | Body | String | - | 시크릿 주소 |
| secret_refs.name | Body | String | - | 컨테이너가 지정한 시크릿 이름<br> 컨테이너 타입이 `certificate`인 경우: `certificate`, `private_key`, `private_key_passphrase`, `intermediates`로 지정<br> 컨테이너 타입이 `rsa`인 경우: `private_key`, `private_key_passphrase`, `public_key`로 지정 |


<details><summary>예시</summary>
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

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| container_ref | Body | String | 시크릿 컨테이너 주소 |

<details><summary>예시</summary>
<p>

```json
{
    "container_ref": "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/ea2e90fc-1ba2-412b-b7a0-61da4402bf58"
}
```
</p>
</details>

---
### 시크릿 컨테이너 삭제하기
지정한 시크릿 컨테이너 삭제합니다.
```
DELETE /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| containerId | URL | UUID | 시크릿 컨테이너 ID |


#### 응답

이 API는 응답 본문을 반환하지 않습니다.