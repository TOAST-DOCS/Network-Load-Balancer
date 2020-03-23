## Network > Load Balancer > API 가이드

## 로드밸런서
### 로드밸런서 목록 보기

로드밸런서 목록을 반환합니다.

```
GET /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 로드밸런서 ID |
| name | Query | String | - | 조회할 로드밸런서 이름 |
| provisioning_status | Query | Enum | - | 조회할 로드밸런서의 프로비져닝 상태 |
| description | Query | String | - | 조회할 로드밸런서의 설명 |
| loadbalancer_type| Query | Enum | - | 조회할 로드밸런서의 타입<br> `shared`, `dedicated` 중 하나 |
| workflow_status | Query | String | - | 조회할 로드밸런서의 작업상태 |
| vip_address | Query | String | - | 조회할 로드밸런서의 IP |
| vip_port_id | Query | UUID | - | 조회할 로드밸런서의 포트 ID |
| vip_subnet_id | Query | UUID | - | 조회할 로드밸런서의 서브넷 ID |
| operating_status | Query | UUID | - | 조회할 로드밸런서의 운영 상태 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancers | Body | Array | 로드밸런서 정보 객체 목록 |
| loadbalancers.ipacl_group_action | Body | Enum | 로드밸런서의 IP ACL 동작 방식<br>`DENY`, `ALLOW` 중 하나  |
| loadbalancers.description | Body | String | 로드밸런서 설명 |
| loadbalancers.provisioning_status | Body | Enum | 로드밸런서의 프로비져닝 상태 |
| loadbalancers.tenant_id | Body | String | 테넌트 ID |
| loadbalancers.provider | Body | String | 로드밸런서의 프로바이더 명 |
| loadbalancers.ipacl_groups | Body | String | 로드밸런서에 적용된 IP ACL 그룹 객체 목록 |
| loadbalancers.ipacl_groups.ipacl_group_id | Body | String | IP ACL 그룹 ID |
| loadbalancers.name | Body | String | 로드밸런서 이름 |
| loadbalancers.loadbalancer_type | Body | Enum | 로드밸런서 타입<br> `shared`, `dedicated` 중 하나 |
| loadbalancers.listeners | Body | Object | 로드밸런서의 리스너 객체 목록 |
| loadbalancers.listeners.id | Body | UUID | 리스너 ID |
| loadbalancers.vip_address | Body | String | 로드밸런서의 IP |
| loadbalancers.vip_port_id | Body | UUID | 로드밸런서의 포트 ID |
| loadbalancers.vip_subnet_id | Body | UUID | 로드밸런서의 서브넷 ID |
| loadbalancers.workflow_status | Body | String | 로드밸런서 작업 상태 |
| loadbalancers.id | Body | UUID | 로드밸런서 ID |
| loadbalancers.operating_status | Body | String | 로드밸런서의 운영 상태 |
| loadbalancers.admin_state_up | Body | Boolean | 로드밸런서의 관리자 제어 상태 |

<details><summary>예시</summary>
<p>

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

</p>
</details>


### 로드밸런서 보기
지정한 로드밸런서 정보를 반환합니다.
```
GET /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드밸런서 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드밸런서 정보 객체 |
| loadbalancer.ipacl_group_action | Body | Enum | 로드밸런서의 IP ACL 동작 방식<br>`DENY`, `ALLOW` 중 하나  |
| loadbalancer.description | Body | String | 로드밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드밸런서의 프로비져닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드밸런서의 프로바이더 명 |
| loadbalancer.ipacl_groups | Body | String | 로드밸런서에 적용된 IP ACL 그룹 객체 목록 |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | String | IP ACL 그룹 ID |
| loadbalancer.name | Body | String | 로드밸런서 이름 |
| loadbalancer.loadbalancer_type | Body | Enum | 로드밸런서 타입<br> `shared`, `dedicated` 중 하나 |
| loadbalancer.listeners | Body | Object | 로드밸런서의 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드밸런서의 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드밸런서의 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드밸런서의 서브넷 ID |
| loadbalancer.workflow_status | Body | String | 로드밸런서 작업 상태 |
| loadbalancer.id | Body | UUID | 로드밸런서 ID |
| loadbalancer.operating_status | Body | String | 로드밸런서의 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드밸런서의 관리자 제어 상태 |


<details><summary>예시</summary>
<p>

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
</p>
</details>

---
### 로드밸런서 생성하기
새로운 로드밸런서를 생성합니다.
```
POST /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancer | Body | Object | - | 로드밸런서 정보 객체 |
| loadbalancer.name | Body | String | - | 로드밸런서 이름 |
| loadbalancer.description | Body | String | - | 로드밸런서 설명 |
| loadbalancer.vip_subnet_id | Body | UUID | O | 로드밸런서의 서브넷 ID |
| loadbalancer.vip_address | Body | String | - |로드밸런서의 IP |
| loadbalancer.admin_state_up | Body | Boolean | - |  로드밸런서의 관리자 제어 상태 <br> default=`true`|
| loadbalancer.loadbalancer_type | Body | Enum | - | 로드밸런서 타입<br> `shared`, `dedicated` 중 하나<br>default=`shared` |


<details><summary>예시</summary>
<p>

```json
{
    "loadbalancer": {
        "name": "LB-1",
        "description": "",
        "vip_subnet_id": "dcb31578-1e16-407f-a117-a716795fabc4",
        "vip_address": "192.168.0.187",
        "admin_state_up": true,
        "loadbalancer_type": "shared",
    }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드밸런서 정보 객체 |
| loadbalancer.ipacl_group_action | Body | Enum | 로드밸런서의 IP ACL 동작 방식<br>`DENY`, `ALLOW` 중 하나  |
| loadbalancer.description | Body | String | 로드밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드밸런서의 프로비져닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드밸런서의 프로바이더 명 |
| loadbalancer.ipacl_groups | Body | String | 로드밸런서에 적용된 IP ACL 그룹 객체 목록 |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | String | IP ACL 그룹 ID |
| loadbalancer.name | Body | String | 로드밸런서 이름 |
| loadbalancer.loadbalancer_type | Body | Enum | 로드밸런서 타입<br> `shared`, `dedicated` 중 하나 |
| loadbalancer.listeners | Body | Object | 로드밸런서의 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드밸런서의 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드밸런서의 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드밸런서의 서브넷 ID |
| loadbalancer.workflow_status | Body | String | 로드밸런서 작업 상태 |
| loadbalancer.id | Body | UUID | 로드밸런서 ID |
| loadbalancer.operating_status | Body | String | 로드밸런서의 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드밸런서의 관리자 제어 상태 |


<details><summary>예시</summary>
<p>

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
</p>
</details>

---
### 로드밸런서 수정하기
기존 로드밸런서를 수정합니다.
```
PUT /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드밸런서 ID |
| loadbalancer | Body | Object | O | 로드밸런서 정보 객체 |
| loadbalancer.name | Body | String | - | 로드밸런서 이름 |
| loadbalancer.description | Body | String | - | 로드밸런서 설명 |
| loadbalancer.admin_state_up | Body | Boolean | - | 로드밸런서의 관리자 제어 상태 |

<details><summary>예시</summary>
<p>

```json
{
    "loadbalancer": {
        "name": "LB-1",
        "description": "",
        "admin_state_up": true,
    }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| loadbalancer | Body | Object | 로드밸런서 정보 객체 |
| loadbalancer.ipacl_group_action | Body | Enum | 로드밸런서의 IP ACL 동작 방식<br>`DENY`, `ALLOW` 중 하나  |
| loadbalancer.description | Body | String | 로드밸런서 설명 |
| loadbalancer.provisioning_status | Body | Enum | 로드밸런서의 프로비져닝 상태 |
| loadbalancer.tenant_id | Body | String | 테넌트 ID |
| loadbalancer.provider | Body | String | 로드밸런서의 프로바이더 명 |
| loadbalancer.ipacl_groups | Body | String | 로드밸런서에 적용된 IP ACL 그룹 객체 목록 |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | String | IP ACL 그룹 ID |
| loadbalancer.name | Body | String | 로드밸런서 이름 |
| loadbalancer.loadbalancer_type | Body | Enum | 로드밸런서 타입<br> `shared`, `dedicated` 중 하나 |
| loadbalancer.listeners | Body | Object | 로드밸런서의 리스너 객체 목록 |
| loadbalancer.listeners.id | Body | UUID | 리스너 ID |
| loadbalancer.vip_address | Body | String | 로드밸런서의 IP |
| loadbalancer.vip_port_id | Body | UUID | 로드밸런서의 포트 ID |
| loadbalancer.vip_subnet_id | Body | UUID | 로드밸런서의 서브넷 ID |
| loadbalancer.workflow_status | Body | String | 로드밸런서 작업 상태 |
| loadbalancer.id | Body | UUID | 로드밸런서 ID |
| loadbalancer.operating_status | Body | String | 로드밸런서의 운영 상태 |
| loadbalancer.admin_state_up | Body | Boolean | 로드밸런서의 관리자 제어 상태 |


<details><summary>예시</summary>
<p>

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
</p>
</details>


---
### 로드밸런서 삭제하기
지정한 로드밸런서를 삭제합니다.
```
DELETE /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| loadbalancerId | URL | UUID | O | 로드밸런서 ID |


#### 응답
이 API는 응답 본문을 반환하지 않습니다.














## IP ACL 그룹
### IP ACL 그룹 목록 보기

IP ACL 그룹 목록을 반환합니다.

```
GET /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | String | - | IP ACL 그룹 ID |
| name | Query | String | - | IP ACL 그룹 이름 |
| description | Query | String | - | IP ACL 그룹 설명 |
| action | Body | Enum | IP ACL 적용 방식<br>`ALLOW`, `DENY`중 하나|


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ipacl_groups | Body | Array | IP ACL 그룹 객체 목록 |
| ipacl_groups.ipacl_target_count | Body | String | IP ACL 그룹에 적용된 타깃 개수 |
| ipacl_groups.description | Body | String | IP ACL 그룹 설명 |
| ipacl_groups.loadbalancers | Body | String | IP ACL 그룹이 적용된 로드밸런서 객체 목록 |
| ipacl_groups.loadbalancers.id | Body | String | 로드밸런서 ID |
| ipacl_groups.tenant_id | Body | String | 테넌트 ID |
| ipacl_groups.action | Body | Enum | IP ACL 적용 방식<br>`ALLOW`, `DENY`중 하나|
| ipacl_groups.id | Body | UUID | IP ACL 그룹 ID |
| ipacl_groups.name | Body | String | IP ACL 그룹 명 |

<details><summary>예시</summary>
<p>

```json
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


### IP ACL 그룹 보기
지정한 IP ACL 그룹을 반환합니다.
```
GET /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| ipaclGroupId | Header | String | O | 토큰 ID |

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ipacl_group | Body | Object | IP ACL 그룹 객체 |
| ipacl_group.ipacl_target_count | Body | String | IP ACL 그룹에 적용된 타깃 개수 |
| ipacl_group.description | Body | String | IP ACL 그룹 설명 |
| ipacl_group.loadbalancers | Body | String | IP ACL 그룹이 적용된 로드밸런서 객체 목록 |
| ipacl_group.loadbalancers.id | Body | String | 로드밸런서 ID |
| ipacl_group.tenant_id | Body | String | 테넌트 ID |
| ipacl_group.action | Body | Enum | IP ACL 적용 방식<br>`ALLOW`, `DENY`중 하나|
| ipacl_group.id | Body | UUID | IP ACL 그룹 ID |
| ipacl_group.name | Body | String | IP ACL 그룹 명 |


<details><summary>예시</summary>
<p>

```json
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

---
### IP ACL 그룹 생성하기
새로운 IP ACL 그룹을 생성합니다.
```
POST /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| ipacl_group | Body | Object | IP ACL 그룹 객체 |
| ipacl_group.description | Body | String | IP ACL 그룹 설명 |
| ipacl_group.action | Body | Enum | O | IP ACL 적용 방식<br>`ALLOW`, `DENY`중 하나|
| ipacl_group.name | Body | String | IP ACL 그룹 명 |

<details><summary>예시</summary>
<p>

```json
{
  "ipacl_group": {
    "action": "ALLOW",
    "name": "example",
    "description": "description"
  }
}
```
</p>
</details>

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ipacl_group | Body | Object | IP ACL 그룹 객체 |
| ipacl_group.ipacl_target_count | Body | String | IP ACL 그룹에 적용된 타깃 개수 |
| ipacl_group.description | Body | String | IP ACL 그룹 설명 |
| ipacl_group.loadbalancers | Body | String | IP ACL 그룹이 적용된 로드밸런서 객체 목록 |
| ipacl_group.loadbalancers.id | Body | String | 로드밸런서 ID |
| ipacl_group.tenant_id | Body | String | 테넌트 ID |
| ipacl_group.action | Body | Enum | IP ACL 적용 방식<br>`ALLOW`, `DENY`중 하나|
| ipacl_group.id | Body | UUID | IP ACL 그룹 ID |
| ipacl_group.name | Body | String | IP ACL 그룹 명 |

<details><summary>예시</summary>
<p>

```json
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

---
### IP ACL 그룹 수정하기
기존 IP ACL 그룹을 수정합니다.
```
PUT /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```
#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| ipaclGroupId | URL | UUID | O | IP ACL 그룹 ID |
| ipacl_group | Body | String | O| IP ACL 그룹 객체 |
| ipacl_group.name | Body | String | - | IP ACL 그룹 명 |
| ipacl_group.description | Body | String | - | IP ACL 그룹 설명 |

<details><summary>예시</summary>
<p>

```json
{
  "ipacl_group": {
    "description": "description",
    "name": "example"
  }
}
```
</p>
</details>


#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ipacl_group | Body | Object | IP ACL 그룹 객체 |
| ipacl_group.ipacl_target_count | Body | String | IP ACL 그룹에 적용된 타깃 개수 |
| ipacl_group.description | Body | String | IP ACL 그룹 설명 |
| ipacl_group.loadbalancers | Body | String | IP ACL 그룹이 적용된 로드밸런서 객체 목록 |
| ipacl_group.loadbalancers.id | Body | String | 로드밸런서 ID |
| ipacl_group.tenant_id | Body | String | 테넌트 ID |
| ipacl_group.action | Body | Enum | IP ACL 적용 방식<br>`ALLOW`, `DENY`중 하나|
| ipacl_group.id | Body | UUID | IP ACL 그룹 ID |
| ipacl_group.name | Body | String | IP ACL 그룹 명 |


<details><summary>예시</summary>
<p>

```json
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

---
### IP ACL 그룹 삭제하기
지정한 IP ACL 그룹을 삭제합니다.
```
GET /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| ipaclGroupId | URL | UUID | O | IP ACL 그룹 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.























## IP ACL 타깃
### IP ACL 타깃 목록 보기

IP ACL 타깃 목록을 반환합니다.

```
GET /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | String | - | IP ACL 타깃 ID |
| cidr_address | Query | String | - | IP ACL 타킷 CIDR |
| description | Query | String | - | IP ACL 타깃 설명 |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ipacl_targets | Body | Array | IP ACL 타깃 정보 객체 목록 |
| ipacl_targets.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| ipacl_targets.tenant_id | Body | String | 테넌트 ID |
| ipacl_targets.cidr_address | Body | String | IP ACL 타깃 CIDR |
| ipacl_targets.description | Body | String | IP ACL 타깃 설명 |
| ipacl_targets.id | Body | UUID | IP ACL 타깃 ID |

<details><summary>예시</summary>
<p>

```json
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


### IP ACL 타깃 보기
지정한 IP ACL 타깃 정보를 반환합니다.
```
GET /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| ipaclTargetId | URL | UUID | O | IP ACL 타깃 ID | 

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ipacl_target | Body | Array | IP ACL 타깃 정보 객체 |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| ipacl_target.tenant_id | Body | String | 테넌트 ID |
| ipacl_target.cidr_address | Body | String | IP ACL 타깃 CIDR |
| ipacl_target.description | Body | String | IP ACL 타깃 설명 |
| ipacl_target.id | Body | UUID | IP ACL 타깃 ID |


<details><summary>예시</summary>
<p>

```json
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

---
### IP ACL 타깃 생성하기
새로운 로드밸런서를 생성합니다.
```
POST /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| ipacl_target | Body | Object | O | IP ACL 타깃 정보 객체 |
| ipacl_target.ipacl_group_id | Body | UUID | O | IP ACL 그룹 ID |
| ipacl_target.cidr_address | Body | String | O | IP ACL 타깃 CIDR |
| ipacl_target.description | Body | String | - | IP ACL 타깃 설명 |

<details><summary>예시</summary>
<p>

```json
{
  "ipacl_target": {
    "ipacl_group_id": "d240300b-53f2-4729-a6bb-b6f84f9be076",
    "cidr_address": "10.0.0.0/24",
    "description": "description",
  }
}
```
</p>
</details>

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ipacl_target | Body | Object | IP ACL 타깃 정보 객체 |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| ipacl_target.tenant_id | Body | String | 테넌트 ID |
| ipacl_target.cidr_address | Body | String | IP ACL 타깃 CIDR |
| ipacl_target.description | Body | String | IP ACL 타깃 설명 |
| ipacl_target.id | Body | UUID | IP ACL 타깃 ID |

<details><summary>예시</summary>
<p>

```json
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

---
### IP ACL 타깃 수정하기
기존 로드밸런서를 수정합니다.
```
GET /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| ipaclTargetId | URL | UUID | O | IP ACL 타깃 ID |
| ipacl_target | Body | Object | O | IP ACL 타깃 정보 객체 |
| ipacl_target.description | Body | String | - | IP ACL 타깃 설명 |

<details><summary>예시</summary>
<p>

```json
{
  "ipacl_target": {
    "description": "description"
  }
}
```
</p>
</details>

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ipacl_target | Body | Object | IP ACL 타깃 정보 객체 |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACL 그룹 ID |
| ipacl_target.tenant_id | Body | String | 테넌트 ID |
| ipacl_target.cidr_address | Body | String | IP ACL 타깃 CIDR |
| ipacl_target.description | Body | String | IP ACL 타깃 설명 |
| ipacl_target.id | Body | UUID | IP ACL 타깃 ID |

<details><summary>예시</summary>
<p>

```json
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

---
### IP ACL 타깃 삭제하기
지정한 로드밸런서를 삭제합니다.
```
DELETE /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| ipaclTargetId | URL | UUID | O | IP ACL 타깃 ID |


#### 응답

이 API는 응답 본문을 반환하지 않습니다.




















## 리스너
### 리스너 목록 보기

리스너 목록을 반환합니다.

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
| proxy_protocol | Body | Boolean | - | 프록시 프로토콜 사용여부 |
| description | Query | String | - | 리스너 설명  |
| name | Query | String| - | 리스너 명 |
| admin_state_up | Query | String | - | 관리자 제어 상태 |
| connection_limit | Query | Integer | - | 리스너의 connection limit |
| keepalive_timeout | Query | Integer | - | 리스너의 keepalive timeout |
| tls_version | Query | String | - | TERMINATED_HTTPS 프로토콜 사용시 사용할 TLS 버전 <br> 사용가능한 버전 목록: `TLSv1.2`, `TLSv1.1`, `TLSv1.0_2016`, `TLSv1.0`, `SSLv3` |
| protocol_port | Query | Integer | - | 리스너 포트 |
| id | Query | UUID | - | 리스너 ID |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listeners | Body | Array | 로드밸런서 정보 객체 목록 |
| listeners.proxy_protocol | Body | Boolean| 프록시 프로토콜 사용여부 <br> HTTPS 프로토콜에서만 사용 가능 |
| listeners.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listeners.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나|
| listeners.description | Body | String | 리스너 설명  |
| listeners.name | Body | String| 리스너 명 |
| listeners.loadbalancers | Body | String| 리스너가 등록된 로드밸런서 객체 |
| listeners.loadbalancers.id | Body | String| 로드밸런서 ID |
| listeners.tenant_id | Body | String| 테넌트 ID |
| listeners.admin_state_up | Body | String| 관리자 제어 상태 |
| listeners.connection_limit | Body | Integer | 리스너의 connection limit |
| listeners.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listeners.tls_version | Body | String| TERMINATED_HTTPS 프로토콜 사용시 사용할 TLS 버전 <br> 사용가능한 버전 목록: `TLSv1.2`, `TLSv1.1`, `TLSv1.0_2016`, `TLSv1.0`, `SSLv3` |
| listeners.default_tls_container_id | Body | String| (DEPRECATED) 기본인프라상품 key-manager의 tls 인증서 경로 |
| listeners.default_tls_container_ref | Body | String| 기본인프라상품 key-manager의 tls 인증서 경로 |
| listeners.sni_container_ids | Body | Array | (DEPRECATED) 기본인프라상품 key-manager의 sni 인증서 경로 목록 |
| listeners.sni_container_refs | Body | Array | 기본인프라상품 key-manager의 sni 인증서 경로 목록 |
| listeners.protocol_port | Body | Integer | 리스너 포트 |
| listeners.id | Body | String| 리스너 ID |
| listeners.cert_expire_date | Body | String | TERMINATED_HTTPS 사용시 등록된 인증서의 만료일 |


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
      "default_tls_container_id": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
      "default_tls_container_ref": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
지정한 리스너 정보를 반환합니다.
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
| listener | Body | Object | 로드밸런서 정보 객체 목록 |
| listener.proxy_protocol | Body | Boolean| 프록시 프로토콜 사용여부 <br> HTTPS 프로토콜에서만 사용 가능 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나|
| listener.description | Body | String | 리스너 설명  |
| listener.name | Body | String| 리스너 명 |
| listener.loadbalancers | Body | String| 리스너가 등록된 로드밸런서 객체 |
| listener.loadbalancers.id | Body | String| 로드밸런서 ID |
| listener.tenant_id | Body | String| 테넌트 ID |
| listener.admin_state_up | Body | String| 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.tls_version | Body | String| TERMINATED_HTTPS 프로토콜 사용시 사용할 TLS 버전 <br> 사용가능한 버전 목록: `TLSv1.2`, `TLSv1.1`, `TLSv1.0_2016`, `TLSv1.0`, `SSLv3` |
| listener.default_tls_container_id | Body | String| (DEPRECATED) 기본인프라상품 key-manager의 tls 인증서 경로 |
| listener.default_tls_container_ref | Body | String| 기본인프라상품 key-manager의 tls 인증서 경로 |
| listener.sni_container_ids | Body | Array| (DEPRECATED) 기본인프라상품 key-manager의 sni 인증서 경로 목록 |
| listener.sni_container_refs | Body | Array | 기본인프라상품 key-manager의 sni 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | String| 리스너 ID |
| listener.cert_expire_date | Body | String | TERMINATED_HTTPS 사용시 등록된 인증서의 만료일 |


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
    "default_tls_container_id": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
    "default_tls_container_ref": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
새로운 리스너를 생성합니다.
```
POST /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listener | Body | Object | O | 로드밸런서 정보 객체 목록 |
| listener.protocol | Body | Enum | O | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나|
| listener.proxy_protocol | Body | Boolean| - | 프록시 프로토콜 사용여부 <br> HTTPS 프로토콜에서만 사용 가능 |
| listener.description | Body | String | - | 리스너 설명  |
| listener.name | Body | String| - |리스너 명 |
| listener.loadbalancer_id | Body | String | O | 로드밸런서 ID |
| listener.admin_state_up | Body | String | - | 관리자 제어 상태 |
| listener.connection_limit | Body |  Integer | - | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | - | 리스너의 keepalive timeout |
| listener.tls_version | Body | String | - | TERMINATED_HTTPS 프로토콜 사용시 사용할 TLS 버전 <br> 사용가능한 버전 목록: `TLSv1.2`, `TLSv1.1`, `TLSv1.0_2016`, `TLSv1.0`, `SSLv3` |
| listener.default_tls_container_ref | Body | String | - | 기본인프라상품 key-manager의 tls 인증서 경로 |
| listener.sni_container_refs | Body | Array | - | 기본인프라상품 key-manager의 sni 인증서 경로 목록 |
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
    "default_tls_container_ref": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
| listener | Body | Object | 로드밸런서 정보 객체 목록 |
| listener.proxy_protocol | Body | Boolean| 프록시 프로토콜 사용여부 <br> HTTPS 프로토콜에서만 사용 가능 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나|
| listener.description | Body | String | 리스너 설명  |
| listener.name | Body | String| 리스너 명 |
| listener.loadbalancers | Body | String| 리스너가 등록된 로드밸런서 객체 |
| listener.loadbalancers.id | Body | String| 로드밸런서 ID |
| listener.tenant_id | Body | String| 테넌트 ID |
| listener.admin_state_up | Body | String| 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.tls_version | Body | String| TERMINATED_HTTPS 프로토콜 사용시 사용할 TLS 버전 <br> 사용가능한 버전 목록: `TLSv1.2`, `TLSv1.1`, `TLSv1.0_2016`, `TLSv1.0`, `SSLv3` |
| listener.default_tls_container_id | Body | String| (DEPRECATED) 기본인프라상품 key-manager의 tls 인증서 경로 |
| listener.default_tls_container_ref | Body | String| 기본인프라상품 key-manager의 tls 인증서 경로 |
| listener.sni_container_ids | Body | Array | (DEPRECATED) 기본인프라상품 key-manager의 sni 인증서 경로 목록|
| listener.sni_container_refs | Body | Array | 기본인프라상품 key-manager의 sni 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | String| 리스너 ID |
| listener.cert_expire_date | Body | String | TERMINATED_HTTPS 사용시 등록된 인증서의 만료일 |


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
    "default_tls_container_id": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
    "default_tls_container_ref": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
기존 리스너를 수정합니다.
```
PUT /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| listenerId | URL | UUID | O | 리스너 ID |
| listener | Body | Object | O | 로드밸런서 정보 객체  |
| listener.proxy_protocol | Body | Boolean| - | 프록시 프로토콜 사용여부 <br> HTTPS 프로토콜에서만 사용 가능 |
| listener.description | Body | String | - | 리스너 설명  |
| listener.name | Body | String| - |리스너 명 |
| listener.admin_state_up | Body | String | - | 관리자 제어 상태 |
| listener.connection_limit | Body |  Integer | - | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | - | 리스너의 keepalive timeout |
| listener.tls_version | Body | String | - | TERMINATED_HTTPS 프로토콜 사용시 사용할 TLS 버전 <br> 사용가능한 버전 목록: `TLSv1.2`, `TLSv1.1`, `TLSv1.0_2016`, `TLSv1.0`, `SSLv3` |
| listener.default_tls_container_ref | Body | String | - | 기본인프라상품 key-manager의 tls 인증서 경로 |
| listener.sni_container_refs | Body | Array | - | 기본인프라상품 key-manager의 sni 인증서 경로 목록 |

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
    "default_tls_container_ref": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
  }
}
```
</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| listener | Body | Object | 로드밸런서 정보 객체 목록 |
| listener.proxy_protocol | Body | Boolean| 프록시 프로토콜 사용여부 <br> HTTPS 프로토콜에서만 사용 가능 |
| listener.default_pool_id | Body | UUID | 리스너에 등록된 풀 ID |
| listener.protocol | Body | Enum | 리스너의 프로토콜<br>`TCP`, `HTTP`,`HTTPS`, `TERMINATED_HTTPS` 중 하나|
| listener.description | Body | String | 리스너 설명  |
| listener.name | Body | String| 리스너 명 |
| listener.loadbalancers | Body | String| 리스너가 등록된 로드밸런서 객체 |
| listener.loadbalancers.id | Body | String| 로드밸런서 ID |
| listener.tenant_id | Body | String| 테넌트 ID |
| listener.admin_state_up | Body | String| 관리자 제어 상태 |
| listener.connection_limit | Body | Integer | 리스너의 connection limit |
| listener.keepalive_timeout | Body | Integer | 리스너의 keepalive timeout |
| listener.tls_version | Body | String| TERMINATED_HTTPS 프로토콜 사용시 사용할 TLS 버전 <br> 사용가능한 버전 목록: `TLSv1.2`, `TLSv1.1`, `TLSv1.0_2016`, `TLSv1.0`, `SSLv3` |
| listener.default_tls_container_id | Body | String| (DEPRECATED) 기본인프라상품 key-manager의 tls 인증서 경로 |
| listener.default_tls_container_ref | Body | String| 기본인프라상품 key-manager의 tls 인증서 경로 |
| listener.sni_container_ids | Body | Array | (DEPRECATED) 기본인프라상품 key-manager의 sni 인증서 경로 목록 |
| listener.sni_container_refs | Body | Array | 기본인프라상품 key-manager의 sni 인증서 경로 목록 |
| listener.protocol_port | Body | Integer | 리스너 포트 |
| listener.id | Body | String| 리스너 ID |
| listener.cert_expire_date | Body | String | TERMINATED_HTTPS 사용시 등록된 인증서의 만료일 |


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
    "default_tls_container_id": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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
    "default_tls_container_ref": "https://key-manager-endpoint/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
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

풀 목록을 반환합니다.

```
GET /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | String | - | 풀 ID |
| name | Query | String | - | 풀 명 |
| lb_algorithm | Query | String | - | 풀의 로드밸런싱 방식  |
| protocol | Query | String | - | 멤버의 프로토콜 |
| admin_state_up | Query | String | - | 관리자 제어 상태 |
| member_port | Query | String | - | 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트 값 |
| healthmonitor_id | Query | String | - | 풀의 헬스모니터 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| pools | Body | Array | 풀 정보 객체 목록 |
| pools.lb_algorithm | Body | Enum | 풀의 로드밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_ID`중 하나 |
| pools.protocol | Body | String | 멤버의 프로토콜 |
| pools.description | Body | String | 풀 설명 |
| pools.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pools.tenant_id | Body | String | 테넌트 ID |
| pools.member_port | Body | String | 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트 값|
| pools.session_persistence | Body | Object | 풀의 세션지속성 객체 |
| pools.session_persistence.type | Body | Enum | 세션지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다.|
| pools.healthmonitor_id | Body | String | 헬스모니터 ID |
| pools.listeners | Body | Array | 풀이 등록된 리스너 객체 |
| pools.listeners.id | Body | String | 리스서 ID |
| pools.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pools.members.id | Body | String | 멤버 ID |
| pools.id | Body | String | 풀 ID |
| pools.name | Body | String | 풀 명 |

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
지정한 풀 정보를 반환합니다.
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
| pool | Body | Object | 풀 정보 객체 목록 |
| pool.lb_algorithm | Body | Enum | 풀의 로드밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_ID`중 하나 |
| pool.protocol | Body | String | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pool.member_port | Body | String | 멤버의 Port<br> 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트 값|
| pools.session_persistence | Body | Object | 풀의 세션지속성 객체 |
| pools.session_persistence.type | Body | Enum | 세션지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다.|
| pool.healthmonitor_id | Body | String | 헬스모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 |
| pool.listeners.id | Body | String | 리스서 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | String | 멤버 ID |
| pool.id | Body | String | 풀 ID |
| pool.name | Body | String | 풀 명 |

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
새로운 풀을 생성합니다.
```
POST /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| pool | Body | Object | O | 풀 정보 객체 목록 |
| pool.listener_id | Body | UUID | O | 풀이 등록될 리스너 ID |
| pool.lb_algorithm | Body | Enum | O | 풀의 로드밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_ID`중 하나 |
| pool.protocol | Body | String | O |멤버의 프로토콜 |
| pool.description | Body | String | - |  풀 설명 |
| pool.admin_state_up | Body | Booean | - | 관리자 제어 상태 |
| pool.member_port | Body | String | - | 멤버의 Port<br> 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트 값|
| pools.session_persistence | Body | Object | - | 풀의 세션지속성 객체 |
| pools.session_persistence.type | Body | Enum | - | 세션지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | - | 쿠키 이름 <br>세션지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다.|
| pool.name | Body | String | - | 풀 명 |



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
| pool | Body | Object | 풀 정보 객체 목록 |
| pool.lb_algorithm | Body | Enum | 풀의 로드밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_ID`중 하나 |
| pool.protocol | Body | String | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pool.member_port | Body | String | 멤버의 Port<br> 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트 값|
| pool.session_persistence | Body | Enum | 세션지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없으며, PROTOCOL에 따라 사용할 수 있는 다릅니다. |
| pool.healthmonitor_id | Body | String | 헬스모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 |
| pool.listeners.id | Body | String | 리스서 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | String | 멤버 ID |
| pool.id | Body | String | 풀 ID |
| pool.name | Body | String | 풀 명 |

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
기존 풀을 수정합니다.
```
PUT /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```


#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | UUID | O | 풀 ID |
| pool | Body | Object | O | 풀 정보 객체 목록 |
| pool.lb_algorithm | Body | Enum | O | 풀의 로드밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_ID`중 하나 |
| pool.description | Body | String | - |  풀 설명 |
| pool.admin_state_up | Body | Booean | - | 관리자 제어 상태 |
| pool.member_port | Body | String | - | 멤버의 Port<br> 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트 값|
| pools.session_persistence | Body | Object | - |풀의 세션지속성 객체 |
| pools.session_persistence.type | Body | Enum | - |세션지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | - | 쿠키 이름 <br>세션지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다.|
| pool.name | Body | String | - | 풀 명 |



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
| pool | Body | Object | 풀 정보 객체 목록 |
| pool.lb_algorithm | Body | Enum | 풀의 로드밸런싱 방식 <br> `ROUND_ROBIN`, `LEAST_CONNECTIONS`, `SOURCE_ID`중 하나 |
| pool.protocol | Body | String | 멤버의 프로토콜 |
| pool.description | Body | String | 풀 설명 |
| pool.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| pool.tenant_id | Body | String | 테넌트 ID |
| pool.member_port | Body | String | 멤버의 Port<br> 웹콘솔에서 멤버를 생성할 경우 지정되는 멤버의 포트 값|
| pools.session_persistence | Body | Object | 풀의 세션지속성 객체 |
| pools.session_persistence.type | Body | Enum | 세션지속성<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나<br> 로드밸런싱 방식이 SOURCE_IP인 경우 사용할 수 없습니다.<br>프로토콜이 HTTPS이거나 TCP인 경우 `HTTP_COOKIE`와 `APP_COOKIE`를 사용할 수 없습니다. |
| pools.session_persistence.cookie_name | Body | String | 쿠키 이름 <br>세션지속성 타입이 `APP_COOKIE`인 경우에만 사용 가능합니다.|
| pool.healthmonitor_id | Body | String | 헬스모니터 ID |
| pool.listeners | Body | Array | 풀이 등록된 리스너 객체 |
| pool.listeners.id | Body | String | 리스서 ID |
| pool.members | Body | Array | 풀에 등록된 멤버 객체 목록 |
| pool.members.id | Body | String | 멤버 ID |
| pool.id | Body | String | 풀 ID |
| pool.name | Body | String | 풀 명 |

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




























## 헬스모니터
### 헬스모니터 목록 보기

헬스모니터 목록을 반환합니다.

```
GET /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | String | - | 토큰 ID |
| admin_state_up | Query | Booelan | - | 관리자 제어 상태 |
| health_check_port | Query | Integer | - | 상태 확인 포트 <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다.|
| delay | Query | Integer | - | 상태 확인 주기 |
| expected_codes | Query | String | - | 상태 확인시 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능합니다.<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| max_retries | Query | Integer | - | 최대 재시도 횟수 |
| http_method | Query | String | - | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| timeout | Query | Integer | - | 상태 확인 응답 대기 시간 |
| url_path | Query | String | - | 상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| type | Query | Enum | - | 상태 확인 타입 |



#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitors | Body | Array | 헬스모니터 정보 객체 목록 |
| healthmonitors.admin_state_up | Body | Booelan | 관리자 제어 상태 |
| healthmonitors.health_check_port | Body | Integer | 상태 확인 포트 <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다.|
| healthmonitors.delay | Body | Integer | 상태 확인 주기 |
| healthmonitors.expected_codes | Body | String | 상태 확인시 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능합니다.<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitors.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitors.http_method | Body | String | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitors.timeout | Body | Integer | 상태 확인 응답 대기 시간 |
| healthmonitors.pools | Body | Array | 헬스모니터가 연결된 풀 객체 |
| healthmonitors.pools.id | Body | String | 풀 ID |
| healthmonitors.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitors.type | Body | Enum | 상태 확인 타입 |
| healthmonitors.id | Body | UUID | 헬스모니터 ID |


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


### 헬스모니터 보기
지정한 헬스모니터 정보를 반환합니다.
```
GET /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthMonitorId | URL | UUID | O | 헬스모니터 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| healthmonitor | Body | Object | 헬스모니터 정보 객체 목록 |
| healthmonitor.admin_state_up | Body | Booelan | 관리자 제어 상태 |
| healthmonitor.health_check_port | Body | Integer | 상태 확인 포트 <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다.|
| healthmonitor.delay | Body | Integer | 상태 확인 주기 |
| healthmonitor.expected_codes | Body | String | 상태 확인시 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능합니다.<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | String | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간 |
| healthmonitor.pools | Body | Array | 헬스모니터가 연결된 풀 객체 |
| healthmonitor.pools.id | Body | String | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.type | Body | Enum | 상태 확인 타입 |
| healthmonitor.id | Body | UUID |  헬스모니터 ID |


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
### 헬스모니터 생성하기
새로운 헬스모니터를 생성합니다.
```
POST /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthmonitor | Body | Object | O | 헬스모니터 정보 객체 목록 |
| healthmonitor.pool_id | Body | UUID | O |  |
| healthmonitor.admin_state_up | Body | Booelan | - | 관리자 제어 상태 |
| healthmonitor.health_check_port | Body | Integer | - | 상태 확인 포트 <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다.|
| healthmonitor.delay | Body | Integer | O | 상태 확인 주기 |
| healthmonitor.expected_codes | Body | String | - | 상태 확인시 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능합니다.<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.max_retries | Body | Integer | O | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | String | - | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.timeout | Body | Integer | O | 상태 확인 응답 대기 시간 |
| healthmonitor.url_path | Body | String | - |상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.type | Body | Enum  | O | 상태 확인 타입 |




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
| healthmonitor | Body | Object | 헬스모니터 정보 객체 목록 |
| healthmonitor.admin_state_up | Body | Booelan | 관리자 제어 상태 |
| healthmonitor.health_check_port | Body | Integer | 상태 확인 포트 <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다.|
| healthmonitor.delay | Body | Integer | 상태 확인 주기 |
| healthmonitor.expected_codes | Body | String | 상태 확인시 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능합니다.<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | String | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간 |
| healthmonitor.pools | Body | Array | 헬스모니터가 연결된 풀 객체 |
| healthmonitor.pools.id | Body | String | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.type | Body | Enum | 상태 확인 타입 |
| healthmonitor.id | Body | UUID |  헬스모니터 ID |


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
### 헬스모니터 수정하기
기존 헬스모니터를 수정합니다.
```
PUT /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthmonitorId | URL | UUID | O | 헬스모니터 ID |
| healthmonitor | Body | Object | O | 헬스모니터 정보 객체 목록 |
| healthmonitor.admin_state_up | Body | Booelan | - | 관리자 제어 상태 |
| healthmonitor.health_check_port | Body |  Integer | - | 상태 확인 포트 <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다.|
| healthmonitor.delay | Body | Integer | - | 상태 확인 주기 |
| healthmonitor.expected_codes | Body | String | - | 상태 확인시 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능합니다.<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.max_retries | Body | Integer | - | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | String | - | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.timeout | Body | Integer | - | 상태 확인 응답 대기 시간 |
| healthmonitor.url_path | Body | String | - |상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |

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
| healthmonitor | Body | Object | 헬스모니터 정보 객체 목록 |
| healthmonitor.admin_state_up | Body | Booelan | 관리자 제어 상태 |
| healthmonitor.health_check_port | Body | Integer | 상태 확인 포트 <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다.|
| healthmonitor.delay | Body | Integer | 상태 확인 주기 |
| healthmonitor.expected_codes | Body | String | 상태 확인시 멤버의 HTTP 응답 코드 <br> 단일값(200), 목록(201,202), 또는 범위(201-204)로 사용 가능합니다.<br>상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.max_retries | Body | Integer | 최대 재시도 횟수 |
| healthmonitor.http_method | Body | String | 상태 확인에 사용할 HTTP Method <br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.timeout | Body | Integer | 상태 확인 응답 대기 시간 |
| healthmonitor.pools | Body | Array | 헬스모니터가 연결된 풀 객체 |
| healthmonitor.pools.id | Body | String | 풀 ID |
| healthmonitor.url_path | Body | String | 상태 확인 요청 URL<br> 상태 확인 타입이 `HTTP`, `HTTPS`일 경우에만 사용합니다. |
| healthmonitor.type | Body | Enum | 상태 확인 타입 |
| healthmonitor.id | Body | UUID |  헬스모니터 ID |


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
### 헬스모니터 삭제하기
지정한 헬스모니터를 삭제합니다.
```
DELETE /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| healthMonitorId | URL | UUID | O | 헬스모니터 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.






























## 멤버
### 멤버 목록 보기

멤버 목록을 반환합니다.

```
GET /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | String | - | 멤버가 속한 Pool ID |
| id | Query | UUID | - | 멤버 ID |
| weight | Query | Integer | - | 멤버 가중치 |
| admin_state_up | Query | Boolean | - | 관리자 제어 상태 |
| subnet_id | Query | UUID | - | 멤버의 서브넷 ID |
| tenant_id | Query | String | - | 테넌트 ID |
| address | Query | String | - | 멤버의 IP 주소 |
| protocol_port | Query | String | - | 멤버의 포트 |
| operating_status | Query | String | - | 멤버의 운영 상태 |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| members | Body | Array | 멤버 정보 객체 목록 |
| members.weight | Body | Integer | 멤버 가중치 |
| members.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| members.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| members.tenant_id | Body | String | 테넌트 ID |
| members.address | Body | String | 멤버의 IP 주소 |
| members.protocol_port | Body | String | 멤버의 포트 |
| members.id | Body | UUID | 멤버 ID |
| members.operating_status | Body | String | 멤버의 운영 상태 |

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
지정한 멤버 정보를 반환합니다.
```
GET /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | String | - | 멤버가 속한 Pool ID |
| memberId | URL | String | - | 멤버 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| member | Body | Object | 멤버 정보 객체 |
| member.weight | Body | Integer | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | 관리자 제어 상태 |
| member.subnet_id | Body | UUID | 멤버의 서브넷 ID |
| member.tenant_id | Body | String | 테넌트 ID |
| member.address | Body | String | 멤버의 IP 주소 |
| member.protocol_port | Body | String | 멤버의 포트 |
| member.id | Body | UUID | 멤버 ID |
| member.operating_status | Body | String | 멤버의 운영 상태 |

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
새로운 멤버를 생성합니다.
```
POST /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | String | O | 멤버가 속한 Pool ID |
| member | Body | Object | O | 멤버 정보 객체 |
| member.weight | Body | Integer | - | 멤버 가중치 |
| member.admin_state_up | Body | Boolean | -| 관리자 제어 상태 |
| member.subnet_id | Body | UUID | O | 멤버의 서브넷 ID |
| member.address | Body | String | O | 멤버의 IP 주소 |
| member.protocol_port | Body | String | O | 멤버의 포트 |


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
| member.protocol_port | Body | String | 멤버의 포트 |
| member.id | Body | String | 멤버 ID |
| member.operating_status | Body | String | 멤버의 운영 상태 |

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
기존 멤버를 수정합니다.
```
PUT /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | String | O | 멤버가 속한 Pool ID |
| memberId | URL | String | O | 멤버 ID |
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
| member.protocol_port | Body | String | 멤버의 포트 |
| member.id | Body | UUID | 멤버 ID |
| member.operating_status | Body | String | 멤버의 운영 상태 |

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
지정한 멤버를 삭제합니다.
```
DELETE /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| poolId | URL | String | O | 멤버가 속한 Pool ID |
| memberId | URL | String | O | 멤버 ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

