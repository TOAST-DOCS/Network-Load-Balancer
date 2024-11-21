## Network > Load Balancer > API v2ガイド

APIを使用するにはAPIエンドポイントとトークンなどが必要です。[API使用準備](/Compute/Compute/ja/identity-api/)を参照してAPIの使用に必要な情報を準備します。

ロードバランサー、リスナー、プール、ヘルスモニター、メンバーAPIは`network`タイプエンドポイントを利用します。シークレット、シークレットコンテナAPIは`key-manager`タイプエンドポイントを利用して呼び出します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| network | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>日本(東京)リージョン<br>米国(カリフォルニア)リージョン | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com<br>https://us1-api-network-infrastructure.nhncloudservice.com |
| key-manager | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>日本(東京)リージョン<br>米国(カリフォルニア)リージョン | https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-network-infrastructure.nhncloudservice.com |


APIレスポンスにガイドに明示されていないフィールドが表示される場合があります。このようなフィールドはNHN Cloud内部用で使用され、事前の告知なく変更される場合があるため使用しません。

## ロードバランサー

### ロードバランサーリスト表示

```
GET /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するロードバランサーのID |
| name | Query | String | - | 照会するロードバランサー名 |
| provisioning_status | Query | Enum | - | 照会するロードバランサーのプロビジョニングの状態 |
| description | Query | String | - | 照会するロードバランサーの説明 |
| vip_address | Query | String | - | 照会するロードバランサーのIP |
| vip_port_id | Query | UUID | - | 照会するロードバランサーのポートID |
| vip_subnet_id | Query | UUID | - | 照会するロードバランサーのサブネットID |
| operating_status | Query | Enum | - | 照会するロードバランサーの運用状態 |
| loadbalancer_type | Query | String | - | 照会するロードバランサーのタイプ<br>`shared`/`dedicated`/`physical_basic`/`physical_dedicated`のいずれか |


#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| loadbalancers | Body | Array | ロードバランサー情報オブジェクトリスト |
| loadbalancers.description | Body | String | ロードバランサーの説明 |
| loadbalancers.provisioning_status | Body | Enum | ロードバランサープロビジョニング状態 |
| loadbalancers.tenant_id | Body | String | テナントID |
| loadbalancers.provider | Body | String | ロードバランサープロバイダー |
| loadbalancers.name | Body | String | ロードバランサーの名前 |
| loadbalancers.listeners | Body | Object | ロードバランサーリスナーオブジェクトリスト |
| loadbalancers.listeners.id | Body | UUID | リスナーID |
| loadbalancers.pools | Body | Object | ロードバランサーのプールオブジェクトリスト |
| loadbalancers.pools.id | Body | UUID | プールID |
| loadbalancers.vip_address | Body | String | ロードバランサーのIP |
| loadbalancers.vip_port_id | Body | UUID | ロードバランサーのポートID |
| loadbalancers.vip_subnet_id | Body | UUID | ロードバランサーのサブネットID |
| loadbalancers.id | Body | UUID | ロードバランサーのID |
| loadbalancers.operating_status | Body | Enum | ロードバランサーの運用状態 |
| loadbalancers.admin_state_up | Body | Boolean | ロードバランサーの管理者制御状態 |
| loadbalancers.ipacl_groups | Body | Object | ロードバランサーに適用されたIP ACLグループオブジェクト |
| loadbalancers.ipacl_groups.ipacl_group_id | Body | UUID | IP ACLグループID |
| loadbalancers.ipacl_group_action | Body | String | ロードバランサーに適用されたIP ACLグループのaction<br>`null`/`DENY`/`ALLOW`のいずれか |
| loadbalancers.loadbalancer_type | Body | String | ロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |

<details><summary>例</summary>

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
### ロードバランサー表示

```
GET /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| loadbalancerId | URL | UUID | O | ロードバランサーのID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| loadbalancer | Body | Object | ロードバランサー情報オブジェクト |
| loadbalancer.description | Body | String | ロードバランサーの説明 |
| loadbalancer.provisioning_status | Body | Enum | ロードバランサーのプロビジョニング状態 |
| loadbalancer.tenant_id | Body | String | テナントID |
| loadbalancer.provider | Body | String | ロードバランサーのプロバイダー |
| loadbalancer.name | Body | String | ロードバランサーの名前 |
| loadbalancer.listeners | Body | Object | ロードバランサーのリスナーオブジェクトリスト |
| loadbalancer.listeners.id | Body | UUID | リスナーID |
| loadbalancers.pools | Body | Object | ロードバランサーのプールオブジェクトリスト |
| loadbalancers.pools.id | Body | UUID | プールID |
| loadbalancer.vip_address | Body | String | ロードバランサーのIP |
| loadbalancer.vip_port_id | Body | UUID | ロードバランサーのポートID |
| loadbalancer.vip_subnet_id | Body | UUID | ロードバランサーのサブネットID |
| loadbalancer.id | Body | UUID | ロードバランサーのID |
| loadbalancer.operating_status | Body | Enum | ロードバランサーの運用状態 |
| loadbalancer.admin_state_up | Body | Boolean | ロードバランサーの管理者制御状態 |
| loadbalancer.ipacl_groups | Body | Object | ロードバランサーに適用されたIP ACLグループオブジェクト |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACLグループID |
| loadbalancer.ipacl_group_action | Body | String | ロードバランサーに適用されたIP ACLグループのaction<br>`null`/`DENY`/`ALLOW`のいずれか |
| loadbalancer.loadbalancer_type | Body | String | ロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |


<details><summary>例</summary>

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
### ロードバランサーを作成する

```
POST /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| loadbalancer | Body | Object | - | ロードバランサーの情報オブジェクト |
| loadbalancer.name | Body | String | - | ロードバランサー名の前 |
| loadbalancer.description | Body | String | - | ロードバランサーの説明 |
| loadbalancer.vip_subnet_id | Body | UUID | O | ロードバランサーのサブネットID |
| loadbalancer.vip_address | Body | String | - | ロードバランサーのIP |
| loadbalancer.admin_state_up | Body | Boolean | - | ロードバランサーの管理者制御状態。省略すると`true`に設定される。 |
| loadbalancer.loadbalancer_type | Body | String | - | ロードバランサーのタイプとして`shared`/`dedicated`を使用可能<br> 省略した場合は`shared`に設定される |

<details><summary>例</summary>

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| loadbalancer | Body | Object | ロードバランサー情報オブジェクト |
| loadbalancer.description | Body | String | ロードバランサーの説明 |
| loadbalancer.provisioning_status | Body | Enum | ロードバランサーのプロビジョニング状態 |
| loadbalancer.tenant_id | Body | String | テナントID |
| loadbalancer.provider | Body | String | ロードバランサーのプロバイダー名 |
| loadbalancer.name | Body | String | ロードバランサーの名前 |
| loadbalancer.listeners | Body | Object | ロードバランサーのリスナーオブジェクトリスト |
| loadbalancer.listeners.id | Body | UUID | リスナーID |
| loadbalancers.pools | Body | Object | ロードバランサーのプールオブジェクトリスト |
| loadbalancers.pools.id | Body | UUID | プールID |
| loadbalancer.vip_address | Body | String | ロードバランサーのIP |
| loadbalancer.vip_port_id | Body | UUID | ロードバランサーのポートID |
| loadbalancer.vip_subnet_id | Body | UUID | ロードバランサーのサブネットID |
| loadbalancer.id | Body | UUID | ロードバランサーのID |
| loadbalancer.operating_status | Body | Enum | ロードバランサーの運用状態 |
| loadbalancer.admin_state_up | Body | Boolean | ロードバランサーの管理者制御状態 |
| loadbalancer.ipacl_groups | Body | Object | ロードバランサーに適用されたIP ACLグループオブジェクト |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACLグループID |
| loadbalancer.ipacl_group_action | Body | String | ロードバランサーに適用されたIP ACLグループのaction<br>`null`/`DENY`/`ALLOW`のいずれか |
| loadbalancer.loadbalancer_type | Body | String | ロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |


<details><summary>例</summary>

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
### ロードバランサーを修正する

```
PUT /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| loadbalancerId | URL | UUID | O | ロードバランサーのID |
| loadbalancer | Body | Object | O | ロードバランサーの情報オブジェクト |
| loadbalancer.name | Body | String | - | ロードバランサーの名前 |
| loadbalancer.description | Body | String | - | ロードバランサーの説明 |
| loadbalancer.admin_state_up | Body | Boolean | - | ロードバランサーの管理者制御状態 |

<details><summary>例</summary>

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| loadbalancer | Body | Object | ロードバランサーの情報オブジェクト |
| loadbalancer.description | Body | String | ロードバランサーの説明 |
| loadbalancer.provisioning_status | Body | Enum | ロードバランサーのプロビジョニング状態 |
| loadbalancer.tenant_id | Body | String | テナントID |
| loadbalancer.provider | Body | String | ロードバランサーのプロバイダー名 |
| loadbalancer.name | Body | String | ロードバランサーの名前 |
| loadbalancer.listeners | Body | Object | ロードバランサーのリスナーオブジェクトリスト |
| loadbalancer.listeners.id | Body | UUID | リスナーID |
| loadbalancers.pools | Body | Object | ロードバランサーのプールオブジェクトリスト |
| loadbalancers.pools.id | Body | UUID | プールID |
| loadbalancer.vip_address | Body | String | ロードバランサーのIP |
| loadbalancer.vip_port_id | Body | UUID | ロードバランサーのポートID |
| loadbalancer.vip_subnet_id | Body | UUID | ロードバランサーのサブネットID |
| loadbalancer.id | Body | UUID | ロードバランサーのID |
| loadbalancer.operating_status | Body | Enum | ロードバランサーの運用状態 |
| loadbalancer.admin_state_up | Body | Boolean | ロードバランサーの管理者制御状態 |
| loadbalancer.ipacl_groups | Body | Object | ロードバランサーに適用されたIP ACLグループオブジェクト |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACLグループID |
| loadbalancer.ipacl_group_action | Body | String | ロードバランサーに適用されたIP ACLグループのaction<br>`null`/`DENY`/`ALLOW`のいずれか |
| loadbalancer.loadbalancer_type | Body | String | ロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |

<details><summary>例</summary>

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
    "admin_state_up": true
    "ipacl_groups": []
  }
}
```
</details>


---
### ロードバランサーを削除する

```
DELETE /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| loadbalancerId | URL | UUID | O | ロードバランサーのID |


#### レスポンス
このAPIはレスポンス本文を返しません。





















## リスナー
### リスナーリスト表示

```
GET /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| default_pool_id | Query | UUID | - | リスナーに登録された基本メンバーグループ(プール) ID |
| protocol | Query | Enum | - | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のうちいずれか1つ |
| description | Query | String | - | リスナーの説明 |
| name | Query | String | - | リスナーの名前 |
| admin_state_up | Query | Boolean | - | 管理者制御状態 |
| connection_limit | Query | Integer | - | リスナーのconnection limit |
| keepalive_timeout | Query | Integer | - | リスナーのkeepalive timeout |
| protocol_port | Query | Integer | - | リスナーのポート番号 |
| id | Query | UUID | - | リスナーID |


#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| listeners | Body | Array | リスナー情報オブジェクトリスト |
| listeners.default_pool_id | Body | UUID | リスナーに登録された基本メンバーグループ(プール) ID |
| listeners.protocol | Body | Enum | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のうちいずれか1つ |
| listeners.description | Body | String | リスナーの説明 |
| listeners.name | Body | String | リスナーの名前 |
| listeners.loadbalancers | Body | Array | リスナーが登録されたロードバランサーのリスト |
| listeners.loadbalancers.id | Body | UUID | ロードバランサーのID |
| listeners.tenant_id | Body | String | テナントID |
| listeners.admin_state_up | Body | Boolean | 管理者制御状態 |
| listeners.connection_limit | Body | Integer | リスナーのconnection limit |
| listeners.keepalive_timeout | Body | Integer | リスナーのkeepalive timeout |
| listeners.default_tls_container_ref | Body | String| key-managerに登録されたtls証明書のパス |
| listeners.sni_container_refs | Body | Array | key-managerに登録されたsni証明書のパスリスト |
| listeners.protocol_port | Body | Integer | リスナーポート |
| listeners.id | Body | String| リスナーID |


<details><summary>例</summary>
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


### リスナー表示

```
GET /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID | 


#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| listener | Body | Object | リスナー情報オブジェクト |
| listener.default_pool_id | Body | UUID | リスナーに登録された基本メンバーグループ(プール) ID |
| listener.protocol | Body | Enum | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のうちいずれか1つ |
| listener.description | Body | String | リスナーの説明 |
| listener.name | Body | String | リスナーの名前 |
| listener.loadbalancers | Body | Array | リスナーが登録されたロードバランサーのオブジェクトリスト |
| listener.loadbalancers.id | Body | UUID | ロードバランサーのID |
| listener.tenant_id | Body | String | テナントID |
| listener.admin_state_up | Body | Boolean | 管理者制御状態 |
| listener.connection_limit | Body | Integer | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | リスナーのkeepalive timeout |
| listener.default_tls_container_ref | Body | String| key-managerに登録されたtls証明書のパス |
| listener.sni_container_refs | Body | Array | key-managerに登録されたsni証明書のパスリスト |
| listener.protocol_port | Body | Integer | リスナーポート |
| listener.id | Body | UUID | リスナーID |


<details><summary>例</summary>
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
### リスナーを作成する

```
POST /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listener | Body | Object | O | リスナー情報オブジェクト |
| listener.protocol | Body | Enum | O | リスナープロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のうちいずれか1つ |
| listener.description | Body | String | - | リスナーの説明 |
| listener.name | Body | String | - | リスナーの名前 |
| listener.default_pool_id | Body | UUID | - | リスナーに登録された基本メンバーグループ(プール) ID<br>指定しない場合は`使用しない`で作成 |
| listener.loadbalancer_id | Body | UUID | O | ロードバランサーのID |
| listener.admin_state_up | Body | Boolean | - | 管理者制御状態 |
| listener.connection_limit | Body |  Integer | - | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | - | リスナーのkeepalive timeout |
| listener.default_tls_container_ref | Body | String | - | key-managerに登録されたtls証明書のパス |
| listener.sni_container_refs | Body | Array | - | key-managerに登録されたsni証明書のパスリスト |
| listener.protocol_port | Body | Integer | O | リスナーポート |


<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| listener | Body | Object | リスナー情報オブジェクト |
| listener.default_pool_id | Body | UUID | リスナーに登録された基本メンバーグループ(プール) ID |
| listener.protocol | Body | Enum | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のうちいずれか1つ |
| listener.description | Body | String | リスナーの説明 |
| listener.name | Body | String | リスナーの名前 |
| listener.loadbalancers | Body | Array | リスナーが登録されたロードバランサーのオブジェクトリスト |
| listener.loadbalancers.id | Body | UUID | ロードバランサーのID |
| listener.tenant_id | Body | String | テナントID |
| listener.admin_state_up | Body | Boolean | 管理者制御状態 |
| listener.connection_limit | Body | Integer | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | リスナーのkeepalive timeout |
| listener.default_tls_container_ref | Body | String | key-managerに登録されたtls証明書のパス |
| listener.sni_container_refs | Body | Array | key-managerに登録されたsni証明書のパスリスト |
| listener.protocol_port | Body | Integer | リスナーポート |
| listener.id | Body | UUID | リスナーID |


<details><summary>例</summary>
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
### リスナーを修正する

```
PUT /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |
| listener | Body | Object | O | リスナー情報オブジェクト |
| listener.description | Body | String | - | リスナーの説明 |
| listener.name | Body | String| - | リスナーの名前 |
| listener.default_pool_id | Body | UUID | - | リスナーに登録された基本メンバーグループ(プール) ID<br>該当値をnullに指定すると`使用しない`に変更 |
| listener.admin_state_up | Body | Boolean | - | 管理者制御状態 |
| listener.connection_limit | Body |  Integer | - | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | - | リスナーのkeepalive timeout |
| listener.default_tls_container_ref | Body | String | - | key-managerに登録されたtls証明書のパス |
| listener.sni_container_refs | Body | Array | - | key-managerに登録されたsni証明書のパスリスト |

<details><summary>例</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "description": "",
    "name": "",
    "admin_state_up": true,
    "default_pool_id": null,    
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "tls_version": "TLSv1.0",
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": []
  }
}
```
</p>
</details>

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| listener | Body | Object | リスナー情報オブジェクト |
| listener.default_pool_id | Body | UUID | リスナーに登録された基本メンバーグループ(プール) ID |
| listener.protocol | Body | Enum | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のうちいずれか1つ |
| listener.description | Body | String | リスナーの説明 |
| listener.name | Body | String | リスナーの名前 |
| listener.loadbalancers | Body | Array | リスナーが登録されたロードバランサーのオブジェクトリスト |
| listener.loadbalancers.id | Body | UUID | ロードバランサーのID |
| listener.tenant_id | Body | String | テナントID |
| listener.admin_state_up | Body | Boolean | 管理者制御状態 |
| listener.connection_limit | Body | Integer | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | リスナーのkeepalive timeout |
| listener.default_tls_container_ref | Body | String | key-managerに登録されたtls証明書のパス |
| listener.sni_container_refs | Body | Array | key-managerに登録されたsni証明書のパスリスト |
| listener.protocol_port | Body | Integer | リスナーポート |
| listener.id | Body | UUID | リスナーID |


<details><summary>例</summary>
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
### リスナーを削除する
指定したリスナーを削除します。
```
DELETE /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |

#### レスポンス

このAPIはレスポンス本文を返しません。













## プール
### プールリスト表示

```
GET /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | プールID |
| name | Query | String | - | プール名 |
| lb_algorithm | Query | Enum | - | プールのロードバランシング方法<br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のうちいずれかつ |
| protocol | Query | Enum | - | メンバーのプロトコル |
| admin_state_up | Query | Boolean | - | 管理者制御状態 |
| healthmonitor_id | Query | UUID | - | プールのヘルスモニターID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| pools | Body | Array | プール情報オブジェクトリスト |
| pools.lb_algorithm | Body | Enum | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のうちいずれかつ |
| pools.protocol | Body | Enum | メンバーのプロトコル |
| pools.description | Body | String | プールの説明 |
| pools.admin_state_up | Body | Boolean | 管理者制御状態 |
| pools.tenant_id | Body | String | テナントID |
| pools.session_persistence | Body | Object | プールのセッション持続性オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション持続性<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のうち、いずれか1つ設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定した場合、接続されたリスナーのプロトコルが`HTTP`または`TERMINATED_HTTPS`に設定されていることを確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション持続性を`HTTP_COOKIE`、`APP_COOKIE`に設定してもロードバランサーはセッション持続性関連の動作を行いません。 |
| pools.session_persistence.cookie_name | Body | String | Cookie名<br>セッション持続性タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pools.healthmonitor_id | Body | String | ヘルスモニターID |
| pools.loadbalancers | Body | Array | プールが登録されたロードバランサーオブジェクトリスト |
| pools.loadbalancers.id | Body | UUID | ロードバランサーID |
| pools.listeners | Body | Array | プールが登録されたリスナーオブジェクトリスト |
| pools.listeners.id | Body | String | リスナーID |
| pools.members | Body | Array | プールに登録されたメンバーオブジェクトリスト |
| pools.members.id | Body | String | メンバーID |
| pools.id | Body | UUID | プールID |
| pools.name | Body | String | プール名 |

<details><summary>例</summary>
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


### プール表示

```
GET /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| pool | Body | Object | プール情報オブジェクト |
| pool.lb_algorithm | Body | Enum | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のうちいずれかつ |
| pool.protocol | Body | Enum | メンバーのプロトコル |
| pool.description | Body | String | プールの説明 |
| pool.admin_state_up | Body | Boolean | 管理者制御状態 |
| pool.tenant_id | Body | String | テナントID |
| pool.member_port | Body | Integer | メンバーのPort<br> Webコンソールでメンバーを作成する場合に指定されるメンバーのポート値 |
| pool.session_persistence | Body | Object | プールのセッション持続性オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション持続性<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のうち、いずれか1つ設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定した場合、接続されたリスナーのプロトコルが`HTTP`または`TERMINATED_HTTPS`に設定されていることを確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション持続性を`HTTP_COOKIE`、`APP_COOKIE`に設定してもロードバランサーはセッション持続性関連の動作を行いません。 |
| pool.session_persistence.cookie_name | Body | String | Cookie名<br>セッション持続性タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pool.healthmonitor_id | Body | UUID | ヘルスモニターID |
| pool.loadbalancers | Body | Array | プールが登録されたロードバランサーオブジェクトのリスト |
| pool.loadbalancers.id | Body | UUID | ロードバランサーID |
| pool.listeners | Body | Array | プールが登録されたリスナーオブジェクトリスト |
| pool.listeners.id | Body | UUID | リスナーID |
| pool.members | Body | Array | プールに登録されたメンバーオブジェクトリスト |
| pool.members.id | Body | UUID | メンバーID |
| pool.id | Body | UUID | プールID |
| pool.name | Body | String | プール名 |

<details><summary>例</summary>
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
### プールを作成する

```
POST /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| pool | Body | Object | O | プール情報オブジェクト |
| pool.loadbalancer_id | Body | UUID | - | プールが登録されるロードバランサーID。ロードバランサーIDまたはリスナーIDのどちらかは必須で入力する必要があります。 |
| pool.listener_id | Body | UUID | - | プールが登録されるリスナーID、ロードバランサーIDかリスナーIDのどちらかは必須で入力する必要があります。 |
| pool.lb_algorithm | Body | Enum | O | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のうちいずれかつ |
| pool.protocol | Body | Enum | O | メンバーのプロトコル |
| pool.description | Body | String | - | プールの説明 |
| pool.admin_state_up | Body | Boolean | - | 管理者制御状態 |
| pool.member_port | Body | Integer | - | メンバーの受信ポート<br>トラフィックをこのポートへ伝達します。<br>基本値は -1です。 |
| pool.session_persistence | Body | Object | - | プールのセッション持続性オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション持続性<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のうち、いずれか1つ設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定した場合、接続されたリスナーのプロトコルが`HTTP`または`TERMINATED_HTTPS`に設定されていることを確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション持続性を`HTTP_COOKIE`、`APP_COOKIE`に設定してもロードバランサーはセッション持続性関連の動作を行いません。 |
| pools.session_persistence.cookie_name | Body | String | Cookie名<br>セッション持続性タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pool.name | Body | String | - | プール名 |



<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| pool | Body | Object | プール情報オブジェクト |
| pool.lb_algorithm | Body | Enum | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のうちいずれかつ |
| pool.protocol | Body | Enum | メンバーのプロトコル |
| pool.description | Body | String | プールの説明 |
| pool.admin_state_up | Body | Boolean | 管理者制御状態 |
| pool.tenant_id | Body | String | テナントID |
| pool.session_persistence | Body | Object | - | プールのセッション持続性オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション持続性<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のうち、いずれか1つ設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定した場合、接続されたリスナーのプロトコルが`HTTP`または`TERMINATED_HTTPS`に設定されていることを確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション持続性を`HTTP_COOKIE`、`APP_COOKIE`に設定してもロードバランサーはセッション持続性関連の動作を行いません。 |
| pool.healthmonitor_id | Body | String | ヘルスモニターID |
| pool.loadbalancers | Body | Array | プールが登録されたロードバランサーオブジェクトのリスト |
| pool.loadbalancers.id | Body | UUID | ロードバランサーID |
| pool.listeners | Body | Array | プールが登録されたリスナーオブジェクトリスト |
| pool.listeners.id | Body | UUID | リスナーID |
| pool.members | Body | Array | プールに登録されたメンバーオブジェクトリスト |
| pool.members.id | Body | UUID | メンバーID |
| pool.id | Body | UUID | プールID |
| pool.name | Body | String | プール名 |

<details><summary>例</summary>
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
### プールを修正する

```
PUT /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```


#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | プールID |
| pool | Body | Object | O | プール情報オブジェクト |
| pool.lb_algorithm | Body | Enum | - | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のうちいずれかつ |
| pool.description | Body | String | - | プールの説明 |
| pool.admin_state_up | Body | Boolean | - | 管理者制御状態 |
| pool.session_persistence | Body | Object | - | プールのセッション持続性オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション持続性<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のうち、いずれか1つ設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定した場合、接続されたリスナーのプロトコルが`HTTP`または`TERMINATED_HTTPS`に設定されていることを確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション持続性を`HTTP_COOKIE`、`APP_COOKIE`に設定してもロードバランサーはセッション持続性関連の動作を行いません。 |
| pools.session_persistence.cookie_name | Body | String | Cookie名<br>セッション持続性タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pool.name | Body | String | - | プール名 |



<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| pool | Body | Object | プール情報オブジェクト |
| pool.lb_algorithm | Body | Enum | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のうちいずれかつ |
| pool.protocol | Body | Enum | メンバーのプロトコル |
| pool.description | Body | String | プールの説明 |
| pool.admin_state_up | Body | Boolean | 管理者制御状態 |
| pool.tenant_id | Body | String | テナントID |
| pools.session_persistence | Body | Object | プールのセッション持続性オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション持続性<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のうち、いずれか1つ設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定した場合、接続されたリスナーのプロトコルが`HTTP`または`TERMINATED_HTTPS`に設定されていることを確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション持続性を`HTTP_COOKIE`、`APP_COOKIE`に設定してもロードバランサーはセッション持続性関連の動作を行いません。 |
| pools.session_persistence.cookie_name | Body | String | Cookie名<br>セッション持続性タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pool.healthmonitor_id | Body | UUID | ヘルスモニターID |
| pool.loadbalancers | Body | Array | プールが登録されているロードバランサーオブジェクトの一覧｜｜ pool.loadbalancers.id
| pool.loadbalancers.id | Body | UUID | ロードバランサーID |
| pool.listeners | Body | Array | プールが登録されたリスナーオブジェクトリスト |
| pool.listeners.id | Body | UUID | リスナーID |
| pool.members | Body | Array | プールに登録されたメンバーオブジェクトリスト |
| pool.members.id | Body | UUID | メンバーID |
| pool.id | Body | UUID | プールID |
| pool.name | Body | String | プール名 |

<details><summary>例</summary>
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
### プールを削除する
指定したプールを削除します。
```
DELETE /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | プールID |

#### レスポンス

このAPIはレスポンス本文を返しません。




























## ヘルスモニター
### ヘルスモニターリスト表示

```
GET /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | ヘルスモニターID |
| admin_state_up | Query | Boolean | - | 管理者制御状態 |
| delay | Query | Integer | - | ヘルスチェック間隔(秒) |
| expected_codes | Query | String | - | 正常状態とみなすメンバーのHTTPレスポンスコード <br>単一値(200)、リスト(201,202)、または範囲(201-204)を使用可能。<br>ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| max_retries | Query | Integer | - | 最大再試行回数 |
| http_method | Query | Enum | - | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| timeout | Query | Integer | - | ヘルスチェックレスポンス待機時間(秒) |
| url_path | Query | String | - | ヘルスチェックリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| type | Query | Enum | - | ヘルスチェックに使用するプロトコル。 `TCP`、`HTTP`、`HTTPS`のうちいずれか1つ |
| host_header | Query | String | - | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|


#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| healthmonitors | Body | Array | ヘルスモニター情報オブジェクトリスト |
| healthmonitors.admin_state_up | Body | Boolean | 管理者制御状態 |
| healthmonitors.delay | Body | Integer | ヘルスチェック間隔(秒) |
| healthmonitors.expected_codes | Body | String | 正常状態と見なすメンバーのHTTPレスポンスコード <br>単一値(200)、リスト(201,202)、または範囲(201-204)で使用可能。<br>ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitors.max_retries | Body | Integer | 最大再試行回数 |
| healthmonitors.http_method | Body | Enum | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitors.timeout | Body | Integer | ヘルスチェックレスポンス待機時間(秒) |
| healthmonitors.pools | Body | Array | ヘルスモニターが接続されたプールオブジェクトリスト |
| healthmonitors.pools.id | Body | UUID | プールID |
| healthmonitors.url_path | Body | String | ヘルスチェックリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitors.type | Body | Enum | ヘルスチェックに使用するプロトコル。 `TCP`、`HTTP`、`HTTPS`のうちいずれか1つ |
| healthmonitors.id | Body | UUID | ヘルスモニターID |

| healthmonitors.host_header | Body | String | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|


<details><summary>例</summary>
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


### ヘルスモニター表示

```
GET /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| healthMonitorId | URL | UUID | O | ヘルスモニターID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| healthmonitor | Body | Object | ヘルスモニター情報オブジェクト |
| healthmonitor.admin_state_up | Body | Boolean | 管理者制御状態 |
| healthmonitor.delay | Body | Integer | ヘルスチェック間隔(秒) |
| healthmonitors.expected_codes | Body | String | 正常状態とみなすメンバーのHTTPレスポンスコード <br>単一値(200)、リスト(201,202)、または範囲(201-204)で使用可能。<br>ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.max_retries | Body | Integer | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.timeout | Body | Integer | ヘルスチェックレスポンス待機時間(秒) |
| healthmonitor.pools | Body | Array | ヘルスモニターが接続されたプールオブジェクトリスト |
| healthmonitor.pools.id | Body | UUID | プールID |
| healthmonitor.url_path | Body | String | ヘルスチェックリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.type | Body | Enum | ヘルスチェックに使用するプロトコル。 `TCP`、`HTTP`、`HTTPS`のうちいずれか1つ |
| healthmonitor.id | Body | UUID | ヘルスモニターID |
| healthmonitors.host_header | Body | String | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|



<details><summary>例</summary>
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
### ヘルスモニターを作成する

```
POST /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| healthmonitor | Body | Object | O | ヘルスモニター情報オブジェクト |
| healthmonitor.pool_id | Body | UUID | O | ヘルスモニターが接続されるプールID |
| healthmonitor.admin_state_up | Body | Boolean | - | 管理者制御状態 |
| healthmonitor.health_check_port | Body | Integer | - | ヘルスチェックの対象となるメンバーポート |
| healthmonitor.delay | Body | Integer | O | ヘルスチェック間隔(秒) |
| healthmonitor.expected_codes | Body | String | - | 正常状態とみなすメンバーのHTTPレスポンスコード。省略すると200に設定される。<br>単一値(200)、リスト(201,202)、または範囲(201-204)で使用可能。<br>ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.max_retries | Body | Integer | O | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | - | ヘルスチェックに使用するHTTP Method. 省略すると`GET`が使用される。<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.timeout | Body | Integer | O | ヘルスチェックレスポンス待機時間(秒) |
| healthmonitor.url_path | Body | String | - | ヘルスチェックリクエストURL。省略すると`/`が設定される。<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.type | Body | Enum  | O | ヘルスチェックに使用するプロトコル。 `TCP`、`HTTP`、`HTTPS`のうちいずれか1つ |
| healthmonitors.host_header | Body | String | - | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|





<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| healthmonitor | Body | Object | ヘルスモニター情報オブジェクト |
| healthmonitor.admin_state_up | Body | Boolean | 管理者制御状態 |
| healthmonitor.delay | Body | Integer | ヘルスチェック間隔(秒) |
| healthmonitor.expected_codes | Body | String | 正常状態とみなすメンバーのHTTPレスポンスコード。省略すると200に設定される。<br> 単一値(200)、リスト(201,202)、または範囲(201-204)を使用可能。<br>ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.max_retries | Body | Integer | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.timeout | Body | Integer | ヘルスチェックレスポンス待機時間(秒) |
| healthmonitor.pools | Body | Array | ヘルスモニターが接続されたプールオブジェクトリスト |
| healthmonitor.pools.id | Body | UUID | プールID |
| healthmonitor.url_path | Body | String | ヘルスチェックリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.type | Body | Enum | ヘルスチェックに使用するプロトコル。 `TCP`、`HTTP`、`HTTPS`のうちいずれか1つ |
| healthmonitor.id | Body | UUID | ヘルスモニターID |


<details><summary>例</summary>
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
### ヘルスモニターを修正する

```
PUT /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| healthmonitorId | URL | UUID | O | ヘルスモニターID |
| healthmonitor | Body | Object | O | ヘルスモニター情報オブジェクト |
| healthmonitor.admin_state_up | Body | Boolean | - | 管理者制御状態 |
| healthmonitor.delay | Body | Integer | - | ヘルスチェック間隔(秒) |
| healthmonitor.expected_codes | Body | String | - | 正常状態とみなすメンバーのHTTPレスポンスコード。<br> 単一値(200)、リスト(201,202)、または範囲(201-204)を使用可能。<br>ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.max_retries | Body | Integer | - | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | - | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.timeout | Body | Integer | - | ヘルスチェックレスポンス待機時間(秒) |
| healthmonitor.url_path | Body | String | - | ヘルスチェックリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitors.host_header | Body | String | - | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|


<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| healthmonitor | Body | Object | ヘルスモニター情報オブジェクト |
| healthmonitor.admin_state_up | Body | Boolean | 管理者制御状態 |
| healthmonitor.delay | Body | Integer | ヘルスチェック間隔(秒) |
| healthmonitor.expected_codes | Body | String | 正常状態とみなすメンバーのHTTPレスポンスコード。<br> 単一値(200)、リスト(201,202)、または範囲(201-204)を使用可能。<br>ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.max_retries | Body | Integer | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.timeout | Body | Integer | ヘルスチェックレスポンス待機時間(秒) |
| healthmonitor.pools | Body | Array | ヘルスモニターが接続されたプールオブジェクトリスト |
| healthmonitor.pools.id | Body | UUID | プールID |
| healthmonitor.url_path | Body | String | ヘルスチェックリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitor.type | Body | Enum | ヘルスチェックに使用するプロトコル。 `TCP`、`HTTP`、`HTTPS`のうちいずれか1つ |
| healthmonitor.id | Body | UUID | ヘルスモニターID |
| healthmonitors.host_header | Body | String | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|


<details><summary>例</summary>
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
### ヘルスモニターを削除する

```
DELETE /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| healthMonitorId | URL | UUID | O | ヘルスモニターID |

#### レスポンス

このAPIはレスポンス本文を返しません。






























## メンバー
### メンバーリスト表示

```
GET /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属しているPool ID |
| id | Query | UUID | - | メンバーID |
| weight | Query | Integer | - | メンバーの重み |
| admin_state_up | Query | Boolean | - | 管理者制御状態 |
| subnet_id | Query | UUID | - | メンバーのサブネットID |
| tenant_id | Query | String | - | テナントID |
| address | Query | String | - | メンバーのIPアドレス |
| protocol_port | Query | Integer | - | メンバーのポート |
| operating_status | Query | Enum | - | メンバーの運用状態 |


#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| members | Body | Array | メンバー情報オブジェクトリスト |
| members.weight | Body | Integer | メンバーの重み |
| members.admin_state_up | Body | Boolean | 管理者制御状態 |
| members.subnet_id | Body | UUID | メンバーのサブネットID |
| members.tenant_id | Body | String | テナントID |
| members.address | Body | String | メンバーのIPアドレス |
| members.protocol_port | Body | Integer | メンバーのポート |
| members.id | Body | UUID | メンバーID |
| members.operating_status | Body | Enum | メンバーの運用状態 |

<details><summary>例</summary>
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


### メンバー表示

```
GET /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属しているPool ID |
| memberId | URL | UUID | O | メンバーID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| member | Body | Object | メンバー情報オブジェクト |
| member.weight | Body | Integer | メンバーの重み |
| member.admin_state_up | Body | Boolean | 管理者制御状態 |
| member.subnet_id | Body | UUID | メンバーのサブネットID |
| member.tenant_id | Body | String | テナントID |
| member.address | Body | String | メンバーのIPアドレス |
| member.protocol_port | Body | Integer | メンバーのポート |
| member.id | Body | UUID | メンバーID |
| member.operating_status | Body | Enum | メンバーの運用状態 |

<details><summary>例</summary>
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
### メンバーを作成する

```
POST /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属しているPool ID |
| member | Body | Object | O | メンバー情報オブジェクト |
| member.weight | Body | Integer | - | メンバーの重み |
| member.admin_state_up | Body | Boolean | -| 管理者制御状態 |
| member.subnet_id | Body | UUID | O | メンバーのサブネットID |
| member.address | Body | String | O | メンバーのIPアドレス |
| member.protocol_port | Body | Integer | O | メンバーのポート |


<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| member | Body | Object | メンバー情報オブジェクト |
| member.weight | Body | Integer | メンバーの重み |
| member.admin_state_up | Body | Boolean | 管理者制御状態 |
| member.subnet_id | Body | UUID | メンバーのサブネットID |
| member.tenant_id | Body | String | テナントID |
| member.address | Body | String | メンバーのIPアドレス |
| member.protocol_port | Body | Integer | メンバーのポート |
| member.id | Body | UUID | メンバーID |
| member.operating_status | Body | Enum | メンバーの運用状態 |

<details><summary>例</summary>
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
### メンバーを修正する

```
PUT /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属しているPool ID |
| memberId | URL | UUID | O | メンバーID |
| member | Body | Object | O | メンバー情報オブジェクト |
| member.weight | Body | Integer | - | メンバーの重み |
| member.admin_state_up | Body | Boolean | - | 管理者制御状態 |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| member | Body | Object | メンバー情報オブジェクト |
| member.weight | Body | Integer | メンバーの重み |
| member.admin_state_up | Body | Boolean | 管理者制御状態 |
| member.subnet_id | Body | UUID | メンバーのサブネットID |
| member.tenant_id | Body | String | テナントID |
| member.address | Body | String | メンバーのIPアドレス |
| member.protocol_port | Body | Integer | メンバーのポート |
| member.id | Body | UUID | メンバーID |
| member.operating_status | Body | Enum | メンバーの運用状態 |

<details><summary>例</summary>
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
### メンバーを削除する

```
DELETE /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属しているPool ID |
| memberId | URL | UUID | O | メンバーID |

#### レスポンス

このAPIはレスポンス本文を返しません。


















## L7ポリシー

### L7ポリシーリスト表示

```
GET /v2.0/lbaas/l7policies
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するL7ポリシーID |
| name | Query | String | - | 照会するL7ポリシー名 |
| description | Query | String | - | 照会するL7ポリシーの説明 |
| listener_id | Query | UUID | - | 照会するL7ポリシーのリスナーID |
| action | Query | Enum | - | 照会するL7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| redirect_pool_id | Query | UUID | - | 照会するL7ポリシーのリダイレクトプールID<br>アクションが`REDIRECT_TO_POOL`の場合にのみ適用 |
| redirect_url | Query | String | - | 照会するL7ポリシーのリダイレクトURL<br>アクションが`REDIRECT_TO_URL`の場合にのみ適用 |
| position | Query | Integer | - | 照会するL7ポリシーの優先順位 |


#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| l7policies | Body | Array | L7ポリシーオブジェクトリスト |
| l7policies.description | Body | String | L7ポリシーの説明 |
| l7policies.tenant_id | Body | String | テナントID |
| l7policies.listener_id | Body | UUID | L7ポリシーのリスナーID |
| l7policies.name | Body | String | L7ポリシー名 |
| l7policies.rules | Body | Object | L7ポリシールールオブジェクトリスト |
| l7policies.rules.id | Body | UUID | L7ルールID |
| l7policies.id | Body | UUID | L7ポリシーID |
| l7policies.admin_state_up | Body | Boolean | L7ポリシー管理者制御状態 |
| l7policies.action | Body | Enum | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policies.redirect_pool_id | Body | UUID | L7ポリシーのリダイレクトプールID<br>アクションが`REDIRECT_TO_POOL`の場合にのみ適用 |
| l7policies.redirect_url | Body | String | L7ポリシーのリダイレクトURL<br>アクションが`REDIRECT_TO_URL`の場合にのみ適用 |
| l7policies.position | Body | Integer | L7ポリシーの優先順位 |

<details><summary>例</summary>

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
### L7ポリシー表示

```
GET /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| l7policy | Body | Object | L7ポリシーオブジェクト |
| l7policy.description | Body | String | L7ポリシー説明 |
| l7policy.tenant_id | Body | String | テナントID |
| l7policy.listener_id | Body | UUID | L7ポリシーのリスナーID |
| l7policy.name | Body | String | L7ポリシー名 |
| l7policy.rules | Body | Object | L7ポリシールールオブジェクトリスト |
| l7policy.rules.id | Body | UUID | L7ルールID |
| l7policy.id | Body | UUID | L7ポリシーID |
| l7policy.admin_state_up | Body | Boolean | L7ポリシー管理者制御状態 |
| l7policy.action | Body | Enum | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policy.redirect_pool_id | Body | UUID | L7ポリシーのリダイレクトプールID<br>アクションが`REDIRECT_TO_POOL`の場合にのみ適用 |
| l7policy.redirect_url | Body | String | L7ポリシーのリダイレクトURL<br>アクションが`REDIRECT_TO_URL`の場合にのみ適用 |
| l7policy.position | Body | Integer | L7ポリシーの優先順位 |


<details><summary>例</summary>

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
### L7ポリシーを作成する

```
POST /v2.0/lbaas/l7policies
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policy | Body | Object | - | L7ポリシーオブジェクト |
| l7policy.description | Body | String | - | L7ポリシー説明 |
| l7policy.listener_id | Body | UUID | O | L7ポリシーのリスナーID |
| l7policy.name | Body | String | - | L7ポリシー名 |
| l7policy.admin_state_up | Body | Boolean | - | L7ポリシー管理者制御状態。省略した場合、`true`に設定 |
| l7policy.action | Body | Enum | O | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policy.redirect_pool_id | Body | UUID | - | L7ポリシーのリダイレクトプールID<br>アクションが`REDIRECT_TO_POOL`の場合必須 |
| l7policy.redirect_url | Body | String | - | L7ポリシーのリダイレクトURL<br>アクションが`REDIRECT_TO_URL`の場合必須 |
| l7policy.position | Body | Integer | - | L7ポリシーの優先順位。省略した場合、最後の順位に設定 |



<details><summary>例</summary>

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| l7policy | Body | Object | L7ポリシーオブジェクト |
| l7policy.description | Body | String | L7ポリシー説明 |
| l7policy.tenant_id | Body | String | テナントID |
| l7policy.listener_id | Body | UUID | L7ポリシーのリスナーID |
| l7policy.name | Body | String | L7ポリシー名 |
| l7policy.rules | Body | Object | L7ポリシールールオブジェクトのリスト
| l7policy.rules.id | Body | UUID | L7ルールID | String
| l7policy.id | Body | UUID | L7ポリシーID |
| l7policy.admin_state_up | Body | Boolean | L7ポリシー管理者制御状態 |
| l7policy.action | Body | Enum | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれかを指定します。
| l7policy.redirect_pool_id | Body | UUID | L7ポリシーのリダイレクトプールID<br> アクションが`REDIRECT_TO_POOL`の場合のみ適用されます。
| l7policy.redirect_url | Body | String | L7ポリシーのリダイレクトURL<br>アクションが`REDIRECT_TO_URL`の場合のみ適用します。
| l7policy.position | Body | Integer | L7ポリシーの優先順位 |


<details><summary>例</summary>

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
### L7ポリシーを修正する

```
PUT /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| l7policy | Body | Object | O | L7ポリシーオブジェクト |
| l7policy.name | Body | String | - | L7ポリシー名 |
| l7policy.description | Body | String | - | L7ポリシー説明 |
| l7policy.admin_state_up | Body | Boolean | - | L7ポリシーの管理者制御状態 |
| l7policy.action | Body | Enum | - | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policy.redirect_pool_id | Body | UUID | - | L7ポリシーのリダイレクトプールID<br>アクションが`REDIRECT_TO_POOL`の場合、必須です。
| l7policy.redirect_url | Body | String | - | L7ポリシーのリダイレクトURL<br>アクションが`REDIRECT_TO_URL`の場合は必須です。
| l7policy.position | Body | Integer | - | L7ポリシーの優先順位 |

<details><summary>例</summary>

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| l7policy | Body | Object | L7ポリシーオブジェクト |
| l7policy.description | Body | String | L7ポリシー説明 |
| l7policy.tenant_id | Body | String | テナントID |
| l7policy.listener_id | Body | UUID | L7ポリシーのリスナーID |
| l7policy.name | Body | String | L7ポリシー名 |
| l7policy.rules | Body | Object | L7ポリシールールオブジェクトのリスト
| l7policy.rules.id | Body | UUID | L7ルールID | String
| l7policy.id | Body | UUID | L7ポリシーID |
| l7policy.admin_state_up | Body | Boolean | L7ポリシー管理者制御状態 |
| l7policy.action | Body | Enum | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれかを指定します。
| l7policy.redirect_pool_id | Body | UUID | L7ポリシーのリダイレクトプールID<br> アクションが`REDIRECT_TO_POOL`の場合のみ適用されます。
| l7policy.redirect_url | Body | String | L7ポリシーのリダイレクトURL<br>アクションが`REDIRECT_TO_URL`の場合のみ適用します。
| l7policy.position | Body | Integer | L7ポリシーの優先順位 |


<details><summary>例</summary>

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
### L7ポリシーを削除する

```
DELETE /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |


#### レスポンス
このAPIはレスポンス本文を返しません。
















## L7ルール

### L7ルールリスト表示

```
GET /v2.0/lbaas/l7policies/{l7policyId}/rules
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ルールが属するL7ポリシーID |
| id | Query | UUID | - | 照会するL7ルールID |
| type | Query | Enum | - | 照会するL7ルールのタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| compare_type | Query | Enum | - | 照会するL7ルールの比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| rules | Body | Array | L7ルールオブジェクトリスト |
| rules.tenant_id | Body | String | テナントID |
| rules.id | Body | UUID | L7ルールID |
| rules.admin_state_up | Body | Boolean | L7ルール管理者制御状態 |
| rules.invert | Body | Boolean | マッチング結果に対するinvert設定 |
| rules.key | Body | String | L7ルールマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合にのみ適用 |
| rules.value | Body | String | L7ルールマッチング時に使用される値 |
| rules.type | Query | Enum | L7ルールタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rules.compare_type | Query | Enum | L7ルール比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |

<details><summary>例</summary>

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
### L7ルール表示

```
GET /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| l7ruleId | URL | UUID | O | L7ルールID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| rule | Body | Object | L7ルールオブジェクト |
| rule.tenant_id | Body | String | テナントID |
| rule.id | Body | UUID | L7ルールID |
| rule.admin_state_up | Body | Boolean | L7ルール管理者制御状態 |
| rule.invert | Body | Boolean | マッチング結果に対するinvert設定 |
| rule.key | Body | String | L7ルールマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合にのみ適用 |
| rule.value | Body | String | L7ルールマッチング時に使用される値 |
| rule.type | Query | Enum | L7ルールタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rule.compare_type | Query | Enum | L7ルール比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


<details><summary>例</summary>

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
### L7ルールを作成する

```
POST /v2.0/lbaas/l7policies/{l7policyId}/rules
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| rule | Body | Object | O | L7ルールオブジェクト |
| rule.admin_state_up | Body | Boolean | - | L7ルール管理者制御状態 |
| rule.invert | Body | Boolean | - | マッチング結果に対するinvert設定。省略した場合、`true`に設定される |
| rule.key | Body | String | - | L7ルールマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合は必須 |
| rule.value | Body | String | O | L7ルールマッチング時に使用される値 |
| rule.type | Query | Enum | O | L7ルールタイプ <br> `COOKIE`/ `FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rule.compare_type | Query | Enum | O | L7ルール比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


<details><summary>例</summary>

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| rule｜Body｜Object｜L7ルールオブジェクト｜ | rule.tenant_id｜Body｜String｜Tenant ID
| rule.tenant_id | Body | String | テナントID |
| rule.id | Body | UUID | L7ルールID | String
rule.admin_state_up | Body | Boolean | L7ルール管理者制御状態｜ | rule.invert | Body | UUID
rule.invert | Body | Boolean | マッチング結果のinvert設定｜ | rule.key | Body | String
rule.key | Body | String | L7ルールマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合のみ適用 | | rule.value | Body | String | L7ルール管理者の制御状態
| rule.value | Body | String | L7ルールマッチング時に使用される値｜｜ rule.type | Query | L7ルールマッチング時に使用される値｜｜ rule.key
| rule.type | Query | Enum | L7ルールタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか｜｜ | rule.compare_type | Query
| rule.compare_type | Query | Enum | L7ルール比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか｜ <BR> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか


<details><summary>例</summary>

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
### L7ルールを修正する

```
PUT /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
l7ruleId | URL | UUID | O | L7ルールID | O | L7ルールID
rule | Body | Object | O | L7ルールオブジェクト｜ O | L7ルールオブジェクト｜ | rule.admin_state_up
| rule.admin_state_up | Body | Boolean | - | L7ルール管理者制御状態｜ - | L7ルール管理者制御状態｜ O
| rule.invert | Body | Boolean | - | マッチング結果に対するinvert設定 |
| rule.key | Body | String | - | L7ルールマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合にのみ適用 |
| rule.value | Body | String | - | L7ルールマッチング時に使用される値 |
| rule.type | Query | Enum | - | L7ルールタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rule.compare_type | Query | Enum | - |L7ルール比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


<details><summary>例</summary>

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| rule｜Body｜Object｜L7ルールオブジェクト｜ | rule.tenant_id｜Body｜String｜Tenant ID
| rule.tenant_id | Body | String | テナントID |
| rule.id | Body | UUID | L7ルールID | String
rule.admin_state_up | Body | Boolean | L7ルール管理者制御状態｜ | rule.invert | Body | UUID
rule.invert | Body | Boolean | マッチング結果のinvert設定｜ | rule.key | Body | String
rule.key | Body | String | L7ルールマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合のみ適用 | | rule.value | Body | String | L7ルール管理者の制御状態
| rule.value | Body | String | L7ルールマッチング時に使用される値｜｜ rule.type | Query | L7ルールマッチング時に使用される値｜｜ rule.key
| rule.type | Query | Enum | L7ルールタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか｜｜ | rule.compare_type | Query
| rule.compare_type | Query | Enum | L7ルール比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか｜ <BR> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか


<details><summary>例</summary>

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
### L7ルールを削除する

```
DELETE /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| l7ruleId | URL | UUID | O | L7ルールID | L7ルールID


#### レスポンス
このAPIはレスポンス本文を返しません。





















## シークレット

シークレットAPIは`key-manager`タイプエンドポイントを利用して呼び出します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| key-manager | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>日本リージョン<br>米国リージョン |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-key-manager-infrastructure.nhncloudservice.com |

APIレスポンスには、ガイドに明示されていないフィールドが表示される場合があります。これらのフィールドはNHN Cloud内部用で使用され、事前の告知なく変更される場合があるため使用しません。


### シークレットリスト表示

シークレットリストを返します。

```
GET /v1/secrets
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| offset | Query | Integer | - | レスポンスリストのプリセット、デフォルト値：0|
| limit | Query | Integer| - | レスポンスリストに表示する最大数、デフォルト値：10 |
| name | Query | String | - | シークレット名 |
| alg | Query | String | - | シークレットアルゴリズム |
| mode | Query | String| - | ブロック暗号運用方式 |
| bits | Query | Integer| - | 暗号化キーの長さ |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| secrets | Body | Array | シークレットオブジェクトリスト |
| secrets.secret_ref | Body | String | シークレットアドレス<br>`<barbican endpoint>/v1/secrets/<secret id>`形式 |
| secrets.secret_type | Body | Enum | シークレットタイプ <br> `symmetric`、`public`、`private`、`passphrase`、`certificate`、`opaque`のいずれか1つ |
| secrets.status | Body | Enum | シークレットの状態 |
| secrets.content_types | Body | Array | シークレットペイロードのコンテンツタイプリスト |
| secrets.content_types.default | Body | String | コンテンツタイプデフォルト値 |
| secrets.creator_id | Body | String | シークレットを作成したユーザーID |
| secrets.mode | Body | String | ブロック暗号運用方式。ユーザー入力メタデータ |
| secrets.algorithm | Body | String | 暗号化アルゴリズム。ユーザー入力メタデータ |
| secrets.bit_length | Body | Integer | 暗号化キーの長さ。ユーザー入力メタデータ |
| secrets.expiration | Body | Datetime | 有効期限。ユーザー入力メタデータ <br>`YYYY-MM-DDThh:mm:ss`<br> 有効期限が過ぎたシークレットは自動的に削除処理される |
| secrets.name| Body | String | シークレット名 |
| secrets.created | Body | Datetime | 作成日時 <br> `YYYY-MM-DDThh:mm:ss` |
| secrets.updated | Body | Datetime | 修正日時 <br> `YYYY-MM-DDThh:mm:ss` |
| total | Body | Integer | リクエストクエリーの総シークレット数 |
| next | Body | String | 現在照会されたリストの次のリストURL |
| previous | Body | String | 現在照会されたリストの以前のリストURL |

<details><summary>例</summary>
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


### シークレット表示
指定したシークレット情報を返します。
```
GET /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| secretId | URL | UUID | O | シークレットID | 

#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| secret | Body | Object | シークレットオブジェクト |
| secret.secret_ref | Body | String | シークレットアドレス<br>`<barbican endpoint>/v1/secrets/<secret id>`形式 |
| secret.secret_type | Body | Enum | シークレットタイプ <br> `symmetric`、`public`、`private`、`passphrase`、`certificate`、`opaque`のうちいずれか1つ |
| secret.status | Body | Enum | シークレットの状態 |
| secret.content_types | Body | Array | シークレットペイロードのコンテンツタイプリスト |
| secret.content_types.default | Body | String | コンテンツタイプのデフォルト値 |
| secret.creator_id | Body | String | シークレットを作成したユーザーID |
| secret.mode | Body | String | ブロック暗号運用方式。ユーザー入力メタデータ |
| secret.algorithm | Body | String | 暗号化アルゴリズム。ユーザー入力メタデータ |
| secret.bit_length | Body | Integer | 暗号化キーの長さ。ユーザー入力メタデータ |
| secret.expiration | Body | Datetime | 有効期限。ユーザー入力メタデータ <br>`YYYY-MM-DDThh:mm:ss`<br> 有効期限が過ぎたシークレットは自動的に削除処理される |
| secret.name| Body | String | シークレット名 |
| secret.created | Body | Datetime | 作成日時 <br> `YYYY-MM-DDThh:mm:ss` |
| secret.updated | Body | Datetime | 修正日時 <br> `YYYY-MM-DDThh:mm:ss` |

<details><summary>例</summary>
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
### シークレットを作成する
新しいシークレットを作成します。
```
POST /v1/secrets
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| name | Body | String | - | シークレット名 |
| expiration | Body | Datetime | - | 有効期限。ISO8601フォーマットでリクエスト |
| algorithm | Body | String | - | 暗号化アルゴリズム |
| bit_length | Body | String | - | 暗号化キーの長さ|
| mode | Body | String | - | ブロック暗号運用方式 |
| payload | Body | String | - | 暗号化キーペイロード |
| payload_content_type | Body | String | - | 暗号化キーペイロードコンテンツタイプ<br> payloadを入力する時、必須入力<br>サポートするコンテンツタイプリスト: `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` |
| payload_content_encoding | Body | Enum | - | 暗号化キーペイロードエンコーディング方式 <br>payload_content_typeがtext/plainではない場合、必須入力。<br> `base64`のみサポート |
| secret_type | Body | Enum | - | シークレットタイプ <br> `symmetric`、`public`、`private`、`passphrase`、`certificate`、`opaque`のうちいずれか1つ |



<details><summary>例</summary>
メタデータのみ作成
```json
{
    "name": "example key",
    "expiration": "2025-12-31T00:00:00.000000Z",
    "algorithm": "example-algorithm",
    "bit_length": 256,
    "mode": "example-mode"
}
```

textでペイロード転送
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

base64でペイロード転送
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

#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| secret_ref | Body | String | シークレットアドレス<br>`<barbican endpoint>/v1/secrets/<secret id>`形式 |

<details><summary>例</summary>
<p>

```json
{
    "secret_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/secrets/9b2dcb7b-51fe-4408-a2bb-23da731758a6"
}
```
</p>
</details>

---
### シークレットを修正する
既にメタデータのみ入力したシークレットのペイロードデータを入力します。
```
PUT /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
Content-Type: {ConetentType}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| secretId | URL | UUID | O | シークレットID | 
| ContentType| Header | Enum | O | `text/plain`、`application/octet-stream`、`application/pkcs8`、`application/pkix-cert`のうちいずれか1つ<br> 省略時は`text/plain`に設定される |
| payload | Body | String | O | 暗号化キーペイロード |

<details><summary>例</summary>
```
{
	"payload": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANQE .... nyxm\n-----END PRIVATE KEY-----\n"
}
```
</details>

#### レスポンス

このAPIはレスポンス本文を返しません。

---
### シークレットを削除する
指定したシークレットを削除します。
```
DELETE /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| secretId | URL | UUID | O | シークレットID | 

#### レスポンス

このAPIはレスポンス本文を返しません。






































## シークレットコンテナ

シークレットコンテナAPIは`key-manager`タイプエンドポイントを利用して呼び出します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| key-manager | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>日本リージョン<br>米国リージョン |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com<br>https://us1-api-key-manager-infrastructure.nhncloudservice.com |

APIレスポンスには、ガイドに明示されていないフィールドが表示される場合があります。これらのフィールドはNHN Cloud内部用で使用され、事前の告知なく変更される場合があるため使用しません。


### シークレットコンテナリスト表示

シークレットコンテナリストを返します。

```
GET /v1/containers
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| offset | Query | Integer | - | レスポンスリストのプリセット、デフォルト値：0|
| limit | Query | Integer | - | レスポンスリストに表示する最大数、デフォルト値：10 |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| containers | Body | Array | コンテナオブジェクトリスト |
| containers.status | Body | Enum | コンテナの状態 |
| containers.updated | Body | Datetime | 修正日時`YYYY-MM-DDThh:mm:ss` |
| containers.name | Body | String | コンテナ名 |
| containers.consumers | Body | Array | コンシューマーリスト |
| containers.consumers.URL | Body | String | コンシューマーURL |
| containers.consumers.name | Body | String | コンシューマー名 |
| containers.created | Body | Datetime | 作成日時`YYYY-MM-DDThh:mm:ss`|
| containers.container_ref | Body | String | コンテナアドレス |
| containers.creator_id | Body | String | コンテナを作成したユーザーID |
| containers.secret_refs | Body | Array | シークレットリスト |
| containers.secret_refs.secret_ref | Body | String | シークレットアドレス |
| containers.secret_refs.name | Body | String | コンテナが指定したシークレット名<br> コンテナタイプが`certificate`の場合：`certificate`、`private_key`、`private_key_passphrase`、`intermediates`に指定<br> コンテナタイプが`rsa`の場合: `private_key`、`private_key_passphrase`、`public_key`に指定 |
| containers.type | Body | Enum | コンテナタイプ<br> `generic`、`rsa`、`certificate`のうちいずれか1つ|
| total | Body | Integer | リクエストクエリーのシークレットコンテナの総数 |
| next | Body | String | 現在照会されたリストの次のリストURL |
| previous | Body | String | 現在照会されたリストの前のリストURL |



<details><summary>例</summary>
<p>

```json
{
  "total": 10,
  "previous": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers?limit=1&offset=0",
  "next": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers?limit=1&offset=2",
  "containers": [
    {
      "status": "ACTIVE",
      "updated": "2019-12-17T08:50:39",
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
      "type": "certificate"
    }
  ]
}


```
</p>
</details>


### シークレットコンテナ表示
指定したシークレットコンテナ情報を返します。
```
GET /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| containerId | URL | UUID | O | シークレットコンテナID |

#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| status | Body | Enum | コンテナの状態 |
| updated | Body | Datetime | 修正日時`YYYY-MM-DDThh:mm:ss` |
| name | Body | String | コンテナ名 |
| consumers | Body | Array | コンシューマーリスト |
| consumers.URL | Body | String | コンシューマーURL |
| consumers.name | Body | String | コンシューマー名 |
| created | Body | Datetime | 作成日時`YYYY-MM-DDThh:mm:ss`|
| container_ref | Body | String | コンテナアドレス |
| creator_id | Body | String | コンテナを作成したユーザーID |
| secret_refs | Body | Array | コンテナに登録したシークレットリスト |
| secret_refs.secret_ref | Body | String | シークレットアドレス |
| secret_refs.name | Body | String| コンテナが指定したシークレット名<br> コンテナタイプが`certificate`の場合：`certificate`、`private_key`、`private_key_passphrase`、`intermediates`に指定<br> コンテナタイプが`rsa`の場合：`private_key`、`private_key_passphrase`、`public_key`に指定 |
| type | Body | Enum | コンテナタイプ<br> `generic`、`rsa`、`certificate`のうちいずれか1つ |


<details><summary>例</summary>

```json
{
    "status": "ACTIVE",
    "updated": "2019-12-17T08:50:39",
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
    "type": "certificate"
}
```
</details>

---
### シークレットコンテナを作成する
新しいシークレットコンテナ作成します。
```
POST /v1/containers
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| type | Body | Enum | O | コンテナタイプ<br> `generic`、`rsa`、`certificate`のうちいずれか1つ |
| name | Body | String | - | コンテナ名 |
| secret_refs | Body | Array | - | コンテナに登録するシークレットリスト |
| secret_refs.secret_ref | Body | String | - | シークレットアドレス |
| secret_refs.name | Body | String | - | コンテナが指定したシークレット名<br> コンテナタイプが`certificate`の場合：`certificate`、`private_key`、`private_key_passphrase`、`intermediates`に指定<br> コンテナタイプが`rsa`の場合：`private_key`、`private_key_passphrase`、`public_key`に指定 |


<details><summary>例</summary>
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

#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| container_ref | Body | String | シークレットコンテナアドレス |

<details><summary>例</summary>
<p>

```json
{
    "container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/ea2e90fc-1ba2-412b-b7a0-61da4402bf58"
}
```
</p>
</details>


---
### シークレットコンテナを削除する
指定したシークレットコンテナ削除します。
```
DELETE /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| containerId | URL | UUID | シークレットコンテナID |


#### レスポンス

このAPIはレスポンス本文を返しません。


## IP ACLグループ

### IP ACLグループリスト表示

IP ACLグループリストを返します。

```
GET /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| id | Query | String | - | IP ACLグループID |
| name | Query | String | - | IP ACLグループ名 |
| description | Query | String | - | IP ACLグループ説明 |
| action | Body | Enum | IP ACLグループの制御動作<br>`ALLOW`、`DENY`のいずれか |  |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_groups | Body | Array | IP ACLグループオブジェクトリスト |
| ipacl_groups.ipacl_target_count | Body | String | IP ACLグループに含まれるターゲットの数 |
| ipacl_groups.description | Body | String | IP ACLグループ説明 |
| ipacl_groups.loadbalancers | Body | Object | IP ACLグループが適用されたロードバランサーオブジェクトリスト |
| ipacl_groups.loadbalancers.loadbalancer_id | Body | String | ロードバランサーID |
| ipacl_groups.tenant_id | Body | String | テナントID |
| ipacl_groups.action | Body | Enum | IPアクセス制御グループの制御動作<br>`ALLOW`、`DENY`のいずれか |
| ipacl_groups.id | Body | UUID | IP ACLグループID |
| ipacl_groups.name | Body | String | IP ACLグループ名 |

<details><summary>例</summary>
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

### IP ACLグループ表示

指定したIP ACLグループを返します。

```
GET /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclGroupId | Header | String | O | トークンID |
#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACLグループオブジェクト |
| ipacl_group.ipacl_target_count | Body | String | IP ACLグループに含まれるターゲットの数 |
| ipacl_group.description | Body | String | IP ACLグループ説明 |
| ipacl_group.loadbalancers | Body | Object | IP ACLグループが適用されたロードバランサーオブジェクトリスト |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | ロードバランサーID |
| ipacl_group.tenant_id | Body | String | テナントID |
| ipacl_group.action | Body | Enum | IP ACLグループの制御動作<br>`ALLOW`、`DENY`のいずれか |
| ipacl_group.id | Body | UUID | IP ACLグループID |
| ipacl_group.name | Body | String | IP ACLグループ名 |

<details><summary>例</summary>
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

### IP ACLグループを作成する

新しいIP ACLグループを作成します。

```
POST /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipacl_group | Body | Object | O | IP ACLグループオブジェクト |
| ipacl_group.description | Body | String | -  | IP ACLグループ説明 |
| ipacl_group.action | Body | Enum | O | IP ACLグループの制御動作<br>`ALLOW`、`DENY`のいずれか |
| ipacl_group.name | Body | String | -  | IP ACLグループ名 |
| ipacl_group.ipacl_targets | Body | Object | - | IP ACLターゲットオブジェクト。値入力時、ターゲットも一緒に作成する |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (ipacl_targetsオブジェクトが追加された場合) | IP ACLターゲットCIDR<br>単独IPアドレス、またはCIDR形式のIP RANGE入力 |
| ipacl_group.ipacl_targets.descripion | Body | String | - | IP ACLターゲット説明 |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACLグループオブジェクト |
| ipacl_group.ipacl_target_count | Body | String | IP ACLグループに含まれるターゲットの数 |
| ipacl_group.description | Body | String | IP ACLグループ説明 |
| ipacl_group.loadbalancers | Body | String | IP ACLグループが適用されたロードバランサーオブジェクトリスト |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | ロードバランサーID |
| ipacl_group.tenant_id | Body | String | テナントID |
| ipacl_group.action | Body | Enum | IP ACLグループの制御動作<br>`ALLOW`、`DENY`のいずれか |
| ipacl_group.id | Body | UUID | IP ACLグループID |
| ipacl_group.name | Body | String | IP ACLグループ名 |

<details><summary>例</summary>
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

### IP ACLグループ修正

既存IP ACLグループを修正します。
ipacl_group.actionは変更できません。
下位IP ACLターゲットリストを全体的に変更する時にこのAPIを使用できます。 
ただし、IP ACLグループに属していたすべての既存のターゲットが削除され、入力したターゲットリストに代替されます。 
入力したターゲットのcidr_addressは重複してはいけません。

```
PUT /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclGroupId | URL | UUID | O | IP ACLグループID |
| ipacl_group | Body | String | O | IP ACLグループオブジェクト |
| ipacl_group.name | Body | String | - | IP ACLグループ名 |
| ipacl_group.description | Body | String | - | IP ACLグループ説明 |
| ipacl_group.ipacl_targets | Body | Object | - | IP ACLターゲットオブジェクト。値入力時、ターゲットも一緒に作成する | 
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (ipacl_targetsオブジェクトが追加された場合) | IP ACLターゲットCIDR<br>単独IPアドレス、またはCIDR形式のIP RANGE入力 |
| ipacl_group.ipacl_targets.descripion | Body | String | - | IP ACLターゲット説明 |


<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACLグループオブジェクト |
| ipacl_group.ipacl_target_count | Body | String | IP ACLグループに含まれるターゲットの数 |
| ipacl_group.description | Body | String | IP ACLグループ説明 |
| ipacl_group.loadbalancers | Body | String | IP ACLグループが適用されたロードバランサーオブジェクトリスト |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | ロードバランサーID |
| ipacl_group.tenant_id | Body | String | テナントID |
| ipacl_group.action | Body | Enum | IP ACLグループの制御動作<br>`ALLOW`、`DENY`のいずれか |
| ipacl_group.id | Body | UUID | IP ACLグループID |
| ipacl_group.name | Body | String | IP ACLグループ名 |

<details><summary>例</summary>
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

### IP ACLグループ削除

指定したIP ACLグループを削除します。

```
DELETE /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

IP ACLグループを削除すると、下位のIP ACLターゲットも全て削除されます。 
削除されるIP ACLグループを使用するすべてのロードバランサーからこのIP ACLグループに関連するルールが削除されます。

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclGroupId | URL | UUID | O | IP ACLグループID |

#### レスポンス

このAPIはレスポンス本文を返しません。

## IP ACLターゲット

### IP ACLターゲットリスト表示

IP ACLターゲットリストを返します。

```
GET /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| id | Query | String | - | IP ACLターゲットID |
| cidr_address | Query | String | - | IP ACLターゲットCIDR<br>単独IPアドレスまたはCIDR形式のIP RANGE |
| ipacl_group_id | Query | String | - | IP ACLグループid |
| description | Query | String | - | IP ACLグループ説明 |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_targets | Body | Array | IP ACLターゲット情報オブジェクトリスト |
| ipacl_targets.ipacl_group_id | Body | UUID | IP ACLグループID |
| ipacl_targets.tenant_id | Body | String | テナントID |
| ipacl_targets.cidr_address | Body | String | IP ACLターゲットCIDR |
| ipacl_targets.description | Body | String | IP ACLターゲット説明 |
| ipacl_targets.id | Body | UUID | IP ACLターゲットID |

<details><summary>例</summary>
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

### IP ACLターゲット表示

指定したIP ACLターゲット情報を返します。

```
GET /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclTargetId | URL | UUID | O | IP ACLターゲットID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_target | Body | Array | IP ACLターゲット情報オブジェクト |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACLグループID |
| ipacl_target.tenant_id | Body | String | テナントID |
| ipacl_target.cidr_address | Body | String | IP ACLターゲットCIDR<br>単独IPアドレスまたはCIDR形式のIP RANGE |
| ipacl_target.description | Body | String | IP ACLターゲット説明 |
| ipacl_target.id | Body | UUID | IP ACLターゲットID |

<details><summary>例</summary>
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

### IP ACLターゲット作成

IP ACLターゲットを作成します。

```
POST /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipacl_target | Body | Object | O | IP ACLターゲット情報オブジェクト |
| ipacl_target.ipacl_group_id | Body | UUID | O | IP ACLグループID |
| ipacl_target.cidr_address | Body | String | O | IP ACLターゲットCIDR<br>単独IPアドレスまたはCIDR形式のIP RANGE |
| ipacl_target.description | Body | String | - | IP ACLターゲット説明 |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_target | Body | Object | IP ACLターゲット情報オブジェクト |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACLグループID |
| ipacl_target.tenant_id | Body | String | テナントID |
| ipacl_target.cidr_address | Body | String | IP ACLターゲットCIDR<br>単独IPアドレスまたはCIDR形式のIP RANGE |
| ipacl_target.description | Body | String | IP ACLターゲット説明 |
| ipacl_target.id | Body | UUID | IP ACLターゲットID |

<details><summary>例</summary>
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

### IP ACLターゲット修正

既存IP ACLターゲットを変更します。
descriptionのみ変更できます。

```
PUT /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclTargetId | URL | UUID | O | IP ACLターゲットID |
| ipacl_target | Body | Object | O | IP ACLターゲット情報オブジェクト |
| ipacl_target.description | Body | String | - | IP ACLターゲット説明 |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_target | Body | Object | IP ACLターゲット情報オブジェクト |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACLグループID |
| ipacl_target.tenant_id | Body | String | テナントID |
| ipacl_target.cidr_address | Body | String | IP ACLターゲットCIDR<br>単独IPアドレスまたはCIDR形式のIP RANGE |
| ipacl_target.description | Body | String | IP ACLターゲット説明 |
| ipacl_target.id | Body | UUID | IP ACLターゲットID |

<details><summary>例</summary>
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

### IP ACLターゲット削除

指定したロードバランサーを削除します。

```
DELETE /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclTargetId | URL | UUID | O | IP ACLターゲットID |

#### レスポンス

このAPIはレスポンス本文を返しません。

- - -

### ロードバランサーにIP ACLグループ適用

ロードバランサーにIP ACLグループを適用します。
IP ACLグループを適用されたロードバランサーにはグループに含まれるIP ACLターゲットのルールが適用されます。
複数のグループをロードバランサーに適用できます。ただし、グループのactionはすべて同じにする必要があります。
既にロードバランサーに適用されていたIP ACLグループはすべて削除され、入力されたグループリストで再適用されます。 

```
PUT /v2.0/lbaas/loadbalancers/{lb_id}/bind_ipacl_groups
X-auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| lb_id | URL | UUID | O | ロードバランサーID |
| ipacl_groups_binding | Body | Object | O | IP ACLバインディングオブジェクト |
| ipacl_groups_binding.ipacl_group_id | Body | UUID | O | ロードバランサーに適用するIP ACLグループID |

<details><summary>例</summary>
<p>

``` json
{
  "ipacl_groups_binding": [
    {
      "ipacl_group_id": "{% response 'body', 'req_7219c88b7b36457fa3a078e0264c0618', '$.ipacl_groups[0].id' %}"
    },
		{
      "ipacl_group_id": "ef33c087-2dc9-4be6-a0d2-d24c9d84e66e"
    }
  ]
}
```

</p>
</details>

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| loadbalancer_id | Body | UUID | ロードバランサーID |
| ipacl_group_id | Body | UUID | IP ACLグループID |

<details><summary>例</summary>
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

- - -
