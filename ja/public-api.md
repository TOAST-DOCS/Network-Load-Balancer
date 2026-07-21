<!-- pre-align:aligned sig=fed248a1eb32 -->

<a id="network-load-balancer-api-v2-guide"></a>
## Network > Load Balancer > API v2 ガイド { #network-load-balancer-api-v2-guide }

NHN Cloud Networkサービスは、APIの呼び出し時に認証/認可のためにIaaSトークンを使用します。IaaSトークンは、NHN CloudのOpenStackベースのインフラサービス(IaaS)で使用する認証トークンです。IaaSトークンの発行及び使用に関する詳細は、[IaaSトークン](/nhncloud/ko/public-api/iaas-token)をご参照ください。

ロードバランサー、リスナー、プール、ヘルスモニター、メンバーの各APIは、`network`タイプのエンドポイントを利用します。シークレット、シークレットコンテナの各APIは、`key-manager`タイプのエンドポイントを利用して呼び出します。正確なエンドポイントは、トークン発行レスポンスの`serviceCatalog`を参照してください。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| network | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>韓国(クァンジュ)リージョン<br>日本(東京)リージョン | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://kr3-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com |
| key-manager | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>韓国(クァンジュ)リージョン<br>日本(東京)リージョン |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr3-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com |


APIレスポンスにガイドに明記されていないフィールドが表示される場合があります。このようなフィールドはNHN Cloudの内部用途で使用されており、事前の通知なしに変更される可能性があるため、使用しないでください。

<a id="load-balancer"></a>
## ロードバランサー { #load-balancer }

<a id="list-load-balancers"></a>
### ロードバランサー一覧の表示 { #list-load-balancers }

```
GET /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

<a id="list-load-balancers-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するロードバランサーのID |
| name | Query | String | - | 照会するロードバランサーの名前 |
| provisioning_status | Query | Enum | - | 照会するロードバランサーのプロビジョニングステータス |
| description | Query | String | - | 照会するロードバランサーの説明 |
| vip_address | Query | String | - | 照会するロードバランサーのIP |
| vip_port_id | Query | UUID | - | 照会するロードバランサーのポートID |
| vip_subnet_id | Query | UUID | - | 照会するロードバランサーのサブネットID |
| operating_status | Query | Enum | - | 照会するロードバランサーの運用ステータス |
| loadbalancer_type | Query | String | - | 照会するロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |


<a id="list-load-balancers-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| loadbalancers | Body | Array | ロードバランサー情報オブジェクトの一覧 |
| loadbalancers.description | Body | String | ロードバランサーの説明 |
| loadbalancers.provisioning_status | Body | Enum | ロードバランサーのプロビジョニングステータス |
| loadbalancers.tenant_id | Body | String | テナントID |
| loadbalancers.provider | Body | String | ロードバランサーのプロバイダー(ベンダー) |
| loadbalancers.name | Body | String | ロードバランサーの名前 |
| loadbalancers.listeners | Body | Object | ロードバランサーのリスナーオブジェクトの一覧 |
| loadbalancers.listeners.id | Body | UUID | リスナーID |
| loadbalancers.pools | Body | Object | ロードバランサーのプールオブジェクトの一覧 |
| loadbalancers.pools.id | Body | UUID | プールID |
| loadbalancers.vip_address | Body | String | ロードバランサーのIP |
| loadbalancers.vip_port_id | Body | UUID | ロードバランサーのポートID |
| loadbalancers.vip_subnet_id | Body | UUID | ロードバランサーのサブネットID |
| loadbalancers.id | Body | UUID | ロードバランサーID |
| loadbalancers.operating_status | Body | Enum | ロードバランサーの運用ステータス |
| loadbalancers.admin_state_up | Body | Boolean | ロードバランサーの管理者制御ステータス |
| loadbalancers.ipacl_groups | Body | Object | ロードバランサーに適用されたIP ACLグループオブジェクト |
| loadbalancers.ipacl_groups.ipacl_group_id | Body | UUID | IP ACLグループID |
| loadbalancers.ipacl_group_action | Body | String | ロードバランサーに適用されたIP ACLグループのアクション<br>`null`/`DENY`/`ALLOW`のいずれか |
| loadbalancers.loadbalancer_type | Body | String | ロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |
| loadbalancers.engine_version | Body | String | ロードバランサーエンジンバージョン<br>`v1`/`v2` のいずれか |

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
<a id="view-load-balancer"></a>
### ロードバランサーの表示 { #view-load-balancer }

```
GET /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

<a id="view-load-balancer-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| loadbalancerId | URL | UUID | O | ロードバランサーID |

<a id="view-load-balancer-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| loadbalancer | Body | Object | ロードバランサー情報オブジェクト |
| loadbalancer.description | Body | String | ロードバランサーの説明 |
| loadbalancer.provisioning_status | Body | Enum | ロードバランサーのプロビジョニングステータス |
| loadbalancer.tenant_id | Body | String | テナントID |
| loadbalancer.provider | Body | String | ロードバランサーのプロバイダー(ベンダー) |
| loadbalancer.name | Body | String | ロードバランサーの名前 |
| loadbalancer.listeners | Body | Object | ロードバランサーのリスナーオブジェクトの一覧 |
| loadbalancer.listeners.id | Body | UUID | リスナーID |
| loadbalancers.pools | Body | Object | ロードバランサーのプールオブジェクトの一覧 |
| loadbalancers.pools.id | Body | UUID | プールID |
| loadbalancer.vip_address | Body | String | ロードバランサーのIP |
| loadbalancer.vip_port_id | Body | UUID | ロードバランサーのポートID |
| loadbalancer.vip_subnet_id | Body | UUID | ロードバランサーのサブネットID |
| loadbalancer.id | Body | UUID | ロードバランサーID |
| loadbalancer.operating_status | Body | Enum | ロードバランサーの運用ステータス |
| loadbalancer.admin_state_up | Body | Boolean | ロードバランサーの管理者制御ステータス |
| loadbalancer.ipacl_groups | Body | Object | ロードバランサーに適用されたIP ACLグループオブジェクト |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACLグループID |
| loadbalancer.ipacl_group_action | Body | String | ロードバランサーに適用されたIP ACLグループのアクション<br>`null`/`DENY`/`ALLOW`のいずれか |
| loadbalancer.loadbalancer_type | Body | String | ロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |
| loadbalancer.engine_version | Body | String | ロードバランサーエンジンバージョン<br>`v1`/`v2` のいずれか |


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
<a id="create-load-balancer"></a>
### ロードバランサーの作成 { #create-load-balancer }

```
POST /v2.0/lbaas/loadbalancers
X-Auth-Token: {tokenId}
```

<a id="create-load-balancer-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| loadbalancer | Body | Object | - | ロードバランサー情報オブジェクト |
| loadbalancer.name | Body | String | - | ロードバランサーの名前 |
| loadbalancer.description | Body | String | - | ロードバランサーの説明 |
| loadbalancer.vip_subnet_id | Body | UUID | O | ロードバランサーのサブネットID |
| loadbalancer.vip_address | Body | String | - | ロードバランサーのIP |
| loadbalancer.admin_state_up | Body | Boolean | - | ロードバランサーの管理者制御ステータス。省略した場合は`true`に設定されます |
| loadbalancer.loadbalancer_type | Body | String | - | ロードバランサーのタイプ。`shared`/`dedicated`を使用可能<br> 省略した場合は`shared`に設定されます |

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

<a id="create-load-balancer-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| loadbalancer | Body | Object | ロードバランサー情報オブジェクト |
| loadbalancer.description | Body | String | ロードバランサーの説明 |
| loadbalancer.provisioning_status | Body | Enum | ロードバランサーのプロビジョニングステータス |
| loadbalancer.tenant_id | Body | String | テナントID |
| loadbalancer.provider | Body | String | ロードバランサーのプロバイダー(ベンダー)名 |
| loadbalancer.name | Body | String | ロードバランサーの名前 |
| loadbalancer.listeners | Body | Object | ロードバランサーのリスナーオブジェクトの一覧 |
| loadbalancer.listeners.id | Body | UUID | リスナーID |
| loadbalancers.pools | Body | Object | ロードバランサーのプールオブジェクトの一覧 |
| loadbalancers.pools.id | Body | UUID | プールID |
| loadbalancer.vip_address | Body | String | ロードバランサーのIP |
| loadbalancer.vip_port_id | Body | UUID | ロードバランサーのポートID |
| loadbalancer.vip_subnet_id | Body | UUID | ロードバランサーのサブネットID |
| loadbalancer.id | Body | UUID | ロードバランサーID |
| loadbalancer.operating_status | Body | Enum | ロードバランサーの運用ステータス |
| loadbalancer.admin_state_up | Body | Boolean | ロードバランサーの管理者制御ステータス |
| loadbalancer.ipacl_groups | Body | Object | ロードバランサーに適用されたIP ACLグループオブジェクト |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACLグループID |
| loadbalancer.ipacl_group_action | Body | String | ロードバランサーに適用されたIP ACLグループのアクション<br>`null`/`DENY`/`ALLOW`のいずれか |
| loadbalancer.loadbalancer_type | Body | String | ロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |
| loadbalancer.engine_version | Body | String | ロードバランサーエンジンバージョン<br>`v1`/`v2` のいずれか |


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
<a id="modify-load-balancer"></a>
### ロードバランサーの修正 { #modify-load-balancer }

```
PUT /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

<a id="modify-load-balancer-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| loadbalancerId | URL | UUID | O | ロードバランサーID |
| loadbalancer | Body | Object | O | ロードバランサー情報オブジェクト |
| loadbalancer.name | Body | String | - | ロードバランサーの名前 |
| loadbalancer.description | Body | String | - | ロードバランサーの説明 |
| loadbalancer.admin_state_up | Body | Boolean | - | ロードバランサーの管理者制御ステータス |
| loadbalancer.engine_version | Body | String | - | ロードバランサーエンジンのバージョン（`v1`/`v2`）<br>変更するとトラフィック処理の動作が変わる場合があります |

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

<a id="modify-load-balancer-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| loadbalancer | Body | Object | ロードバランサー情報オブジェクト |
| loadbalancer.description | Body | String | ロードバランサーの説明 |
| loadbalancer.provisioning_status | Body | Enum | ロードバランサーのプロビジョニングステータス |
| loadbalancer.tenant_id | Body | String | テナントID |
| loadbalancer.provider | Body | String | ロードバランサーのプロバイダー(ベンダー)名 |
| loadbalancer.name | Body | String | ロードバランサーの名前 |
| loadbalancer.listeners | Body | Object | ロードバランサーのリスナーオブジェクトの一覧 |
| loadbalancer.listeners.id | Body | UUID | リスナーID |
| loadbalancers.pools | Body | Object | ロードバランサーのプールオブジェクトの一覧 |
| loadbalancers.pools.id | Body | UUID | プールID |
| loadbalancer.vip_address | Body | String | ロードバランサーのIP |
| loadbalancer.vip_port_id | Body | UUID | ロードバランサーのポートID |
| loadbalancer.vip_subnet_id | Body | UUID | ロードバランサーのサブネットID |
| loadbalancer.id | Body | UUID | ロードバランサーID |
| loadbalancer.operating_status | Body | Enum | ロードバランサーの運用ステータス |
| loadbalancer.admin_state_up | Body | Boolean | ロードバランサーの管理者制御ステータス |
| loadbalancer.ipacl_groups | Body | Object | ロードバランサーに適用されたIP ACLグループオブジェクト |
| loadbalancer.ipacl_groups.ipacl_group_id | Body | UUID | IP ACLグループID |
| loadbalancer.ipacl_group_action | Body | String | ロードバランサーに適用されたIP ACLグループのアクション<br>`null`/`DENY`/`ALLOW`のいずれか |
| loadbalancer.loadbalancer_type | Body | String | ロードバランサーのタイプ<br>`shared`/`dedicated`のいずれか |
| loadbalancer.engine_version | Body | String | ロードバランサーエンジンバージョン<br>`v1`/`v2` のいずれか |


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
<a id="delete-load-balancer"></a>
### ロードバランサーの削除 { #delete-load-balancer }

```
DELETE /v2.0/lbaas/loadbalancers/{loadbalancerId}
X-Auth-Token: {tokenId}
```

<a id="delete-load-balancer-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| loadbalancerId | URL | UUID | O | ロードバランサーID |


<a id="delete-load-balancer-response"></a>
#### レスポンス
このAPIはレスポンスボディ(Body)を返却しません。

<a id="listener"></a>
## リスナー { #listener }
<a id="view-listener-list"></a>
### リスナー一覧の表示 { #view-listener-list }

```
GET /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

<a id="view-listener-list-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| default_pool_id | Query | UUID | - | リスナーに登録されたデフォルトメンバーグループ(プール)のID |
| protocol | Query | Enum | - | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のいずれか |
| protocol_version | Query | Enum | - | HTTP プロトコルバージョン<br>`HTTP/1`、`HTTP/2` のいずれか |
| description | Query | String | - | リスナーの説明 |
| name | Query | String | - | リスナーの名前 |
| admin_state_up | Query | Boolean | - | 管理者制御ステータス |
| connection_limit | Query | Integer | - | リスナーのconnection limit |
| keepalive_timeout | Query | Integer | - | リスナーのkeepalive timeout |
| protocol_port | Query | Integer | - | リスナーのポート番号 |
| id | Query | UUID | - | リスナーID |


<a id="view-listener-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| listeners | Body | Array | リスナー情報オブジェクトの一覧 |
| listeners.default_pool_id | Body | UUID | リスナーに登録されたデフォルトメンバーグループ(プール)のID |
| listeners.protocol | Body | Enum | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のいずれか |
| listeners.protocol_version | Body | Enum | HTTP プロトコルバージョン<br>`HTTP/1`、`HTTP/2` のいずれか |
| listeners.description | Body | String | リスナーの説明 |
| listeners.name | Body | String | リスナーの名前 |
| listeners.loadbalancers | Body | Array | リスナーが登録されたロードバランサーオブジェクトの一覧 |
| listeners.loadbalancers.id | Body | UUID | ロードバランサーID |
| listeners.tenant_id | Body | String | テナントID |
| listeners.admin_state_up | Body | Boolean | 管理者制御ステータス |
| listeners.connection_limit | Body | Integer | リスナーのconnection limit |
| listeners.keepalive_timeout | Body | Integer | リスナーのkeepalive timeout |
| listeners.default_tls_container_ref | Body | String| key-managerに登録されたTLS証明書のパス |
| listeners.sni_container_refs | Body | Array | key-managerに登録されたSNI証明書のパス一覧 |
| listeners.protocol_port | Body | Integer | リスナーポート |
| listeners.proxy_protocol | Body | Boolean | プロキシプロトコルのon/off<br>デフォルト値：`false` |
| listeners.block_invalid_http_request | Body | Boolean | 無効なHTTPリクエストブロックのon/off<br>デフォルト値：`true` |
| listeners.tls_version | Body | String | リスナーのTLSバージョン<br>`SSLv3`、`TLSv1.0`、`TLSv1.0_2016`、`TLSv1.1`、`TLSv1.2`、`TLSv1.3`のいずれか<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用 |
| listeners.ssl_policy_id | Body | UUID | リスナーに接続されたSSLポリシーID<br>接続されたSSLポリシーがない場合は`null`<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用 |
| listeners.keepalive_enable | Body | Boolean | keepalive有効化のon/off<br>デフォルト値：`true` |
| listeners.id | Body | String| リスナーID |


<details><summary>例</summary>
<p>

```json
{
  "listeners": [
    {
      "proxy_protocol": false,
      "block_invalid_http_request": true,
      "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
      "protocol": "TERMINATED_HTTPS",
      "protocol_version": "HTTP/2",
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
      "keepalive_enable": true,
      "tls_version": "TLSv1.0",
      "ssl_policy_id": null,
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


<a id="view-listener"></a>
### リスナーの表示 { #view-listener }

```
GET /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

<a id="view-listener-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |


<a id="view-listener-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| listener | Body | Object | リスナー情報オブジェクト |
| listener.default_pool_id | Body | UUID | リスナーに登録されたデフォルトメンバーグループ(プール)のID |
| listener.protocol | Body | Enum | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のいずれか |
| listener.protocol_version | Body | Enum | HTTP プロトコルバージョン<br>`HTTP/1`、`HTTP/2` のいずれか |
| listener.description | Body | String | リスナーの説明 |
| listener.name | Body | String | リスナーの名前 |
| listener.loadbalancers | Body | Array | リスナーが登録されたロードバランサーオブジェクトの一覧 |
| listener.loadbalancers.id | Body | UUID | ロードバランサーID |
| listener.tenant_id | Body | String | テナントID |
| listener.admin_state_up | Body | Boolean | 管理者制御ステータス |
| listener.connection_limit | Body | Integer | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | リスナーのkeepalive timeout |
| listener.enable_x_forwarded_proto | Body | Boolean | - | X-Forwarded-Proto/X-Forwarded-Portヘッダのon/off<br>デフォルト値：`true` |
| listener.enable_x_forwarded_port | Body | Boolean | - | X-Forwarded-Portヘッダのon/off<br>デフォルト値：`true` |
| listener.enable_x_forwarded_for | Body | Boolean | - | X-Forwarded-Forヘッダのon/off<br>デフォルト値：`true` |
| listener.default_tls_container_ref | Body | String| key-managerに登録されたTLS証明書のパス |
| listener.sni_container_refs | Body | Array | key-managerに登録されたSNI証明書のパス一覧 |
| listener.protocol_port | Body | Integer | リスナーポート |
| listener.proxy_protocol | Body | Boolean | プロキシプロトコルのon/off<br>デフォルト値：`false` |
| listener.block_invalid_http_request | Body | Boolean | 無効なHTTPリクエストブロックのon/off<br>デフォルト値：`true` |
| listener.tls_version | Body | String | リスナーのTLSバージョン<br>`SSLv3`、`TLSv1.0`、`TLSv1.0_2016`、`TLSv1.1`、`TLSv1.2`、`TLSv1.3`のいずれか<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用 |
| listener.ssl_policy_id | Body | UUID | リスナーに接続されたSSLポリシーID<br>接続されたSSLポリシーがない場合は`null`<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用 |
| listener.keepalive_enable | Body | Boolean | keepalive有効化のon/off<br>デフォルト値：`true` |
| listener.id | Body | UUID | リスナーID |


<details><summary>例</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "block_invalid_http_request": true,
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "protocol": "TERMINATED_HTTPS",
    "protocol_version": "HTTP/1",
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
    "keepalive_enable": true,
    "enable_x_forwarded_proto": true,
    "enable_x_forwarded_port": true,
    "enable_x_forwarded_for": true,
    "tls_version": "TLSv1.0",
    "ssl_policy_id": null,
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
<a id="create-listener"></a>
### リスナーの作成 { #create-listener }

```
POST /v2.0/lbaas/listeners
X-Auth-Token: {tokenId}
```

<a id="create-listener-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listener | Body | Object | O | リスナー情報オブジェクト |
| listener.protocol | Body | Enum | O | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のいずれか |
| listener.protocol_version | Body | Enum | - | HTTP プロトコルバージョン<br>`HTTP/1`、`HTTP/2` のいずれか |
| listener.description | Body | String | - | リスナーの説明 |
| listener.name | Body | String | - | リスナーの名前 |
| listener.default_pool_id | Body | UUID | - | リスナーに登録されたデフォルトメンバーグループ(プール)のID<br>指定しない場合は`使用しない`として作成 |
| listener.loadbalancer_id | Body | UUID | O | ロードバランサーID |
| listener.admin_state_up | Body | Boolean | - | 管理者制御ステータス |
| listener.connection_limit | Body |  Integer | - | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | - | リスナーのkeepalive timeout |
| listener.enable_x_forwarded_proto | Body | Boolean | - | X-Forwarded-Proto/X-Forwarded-Portヘッダのon/off<br>デフォルト値：`true` |
| listener.enable_x_forwarded_port | Body | Boolean | - | X-Forwarded-Portヘッダのon/off<br>デフォルト値：`true` |
| listener.enable_x_forwarded_for | Body | Boolean | - | X-Forwarded-Forヘッダのon/off<br>デフォルト値：`true` |
| listener.default_tls_container_ref | Body | String | - | key-managerに登録されたTLS証明書のパス |
| listener.sni_container_refs | Body | Array | - | key-managerに登録されたSNI証明書のパス一覧 |
| listener.protocol_port | Body | Integer | O | リスナーポート |
| listener.proxy_protocol | Body | Boolean | - | プロキシプロトコルのon/off<br>デフォルト値：`false` |
| listener.block_invalid_http_request | Body | Boolean | - | 無効なHTTPリクエストブロックのon/off<br>デフォルト値：`true` |
| listener.tls_version | Body | String | - | リスナーのTLSバージョン<br>`SSLv3`、`TLSv1.0`、`TLSv1.0_2016`、`TLSv1.1`、`TLSv1.2`、`TLSv1.3`のいずれか<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用<br>`ssl_policy_id`と共に指定する場合は、SSLポリシーの`min_tls_version`と一致する必要があります |
| listener.ssl_policy_id | Body | UUID | - | リスナーに接続するSSLポリシーID<br>デフォルト値：`null`<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用<br>詳細は[カスタムSSLポリシー](/Network/Load%20Balancer/ko/overview/#ssl)を参照してください |
| listener.keepalive_enable | Body | Boolean | - | keepalive有効化のon/off<br>デフォルト値：`true` |


<details><summary>例</summary>
<p>

```json
{
  "listener": {
    "protocol": "TERMINATED_HTTPS",
    "protocol_version": "HTTP/2",
    "proxy_protocol": false,
    "block_invalid_http_request": true,
    "description": "",
    "name": "",
    "loadbalancer_id":"7b4cef78-72b0-4c3c-9971-98763ef6284c",
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "enable_x_forwarded_proto": false,
    "enable_x_forwarded_port": false,
    "enable_x_forwarded_for": false,
    "tls_version": "TLSv1.2",
    "ssl_policy_id": "b5b3f6f2-6c29-4f3a-9a2e-3b2e6b2b5c0a",
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": [],
    "protocol_port": 443
  }
}
```
</p>
</details>

<a id="create-listener-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| listener | Body | Object | リスナー情報オブジェクト |
| listener.default_pool_id | Body | UUID | リスナーに登録されたデフォルトメンバーグループ(プール)のID |
| listener.protocol | Body | Enum | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のいずれか |
| listener.protocol_version | Body | Enum | HTTP プロトコルバージョン<br>`HTTP/1`、`HTTP/2` のいずれか |
| listener.description | Body | String | リスナーの説明 |
| listener.name | Body | String | リスナーの名前 |
| listener.loadbalancers | Body | Array | リスナーが登録されたロードバランサーオブジェクトの一覧 |
| listener.loadbalancers.id | Body | UUID | ロードバランサーID |
| listener.tenant_id | Body | String | テナントID |
| listener.admin_state_up | Body | Boolean | 管理者制御ステータス |
| listener.connection_limit | Body | Integer | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | リスナーのkeepalive timeout |
| listener.enable_x_forwarded_proto | Body | Boolean | - | X-Forwarded-Proto/X-Forwarded-Portヘッダのon/off<br>デフォルト値：`true` |
| listener.enable_x_forwarded_port | Body | Boolean | - | X-Forwarded-Portヘッダのon/off<br>デフォルト値：`true` |
| listener.enable_x_forwarded_for | Body | Boolean | - | X-Forwarded-Forヘッダのon/off<br>デフォルト値：`true` |
| listener.default_tls_container_ref | Body | String | key-managerに登録されたTLS証明書のパス |
| listener.sni_container_refs | Body | Array | key-managerに登録されたSNI証明書のパス一覧 |
| listener.protocol_port | Body | Integer | リスナーポート |
| listener.proxy_protocol | Body | Boolean | プロキシプロトコルのon/off<br>デフォルト値：`false` |
| listener.block_invalid_http_request | Body | Boolean | 無効なHTTPリクエストブロックのon/off<br>デフォルト値：`true` |
| listener.tls_version | Body | String | リスナーのTLSバージョン<br>`SSLv3`、`TLSv1.0`、`TLSv1.0_2016`、`TLSv1.1`、`TLSv1.2`、`TLSv1.3`のいずれか<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用 |
| listener.ssl_policy_id | Body | UUID | リスナーに接続されたSSLポリシーID<br>接続されたSSLポリシーがない場合は`null`<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用 |
| listener.keepalive_enable | Body | Boolean | keepalive有効化のon/off<br>デフォルト値：`true` |
| listener.id | Body | UUID | リスナーID |


<details><summary>例</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "block_invalid_http_request": true,
    "default_pool_id": "522a5681-fc4c-4b0b-85ec-bf7777c48a57",
    "protocol": "TERMINATED_HTTPS",
    "protocol_version": "HTTP/2",
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
    "keepalive_enable": true,
    "enable_x_forwarded_proto": false,
    "enable_x_forwarded_port": false,
    "enable_x_forwarded_for": false,
    "tls_version": "TLSv1.2",
    "ssl_policy_id": "b5b3f6f2-6c29-4f3a-9a2e-3b2e6b2b5c0a",
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
<a id="modify-listener"></a>
### リスナーの修正 { #modify-listener }

```
PUT /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

<a id="modify-listener-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |
| listener | Body | Object | O | リスナー情報オブジェクト |
| listener.description | Body | String | - | リスナーの説明 |
| listener.name | Body | String| - | リスナー名 |
| listener.default_pool_id | Body | UUID | - | リスナーに登録されたデフォルトメンバーグループ(プール)のID<br>該当の値をnullに指定すると`使用しない`に変更されます |
| listener.admin_state_up | Body | Boolean | - | 管理者制御ステータス |
| listener.connection_limit | Body |  Integer | - | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | - | リスナーのkeepalive timeout |
| listener.enable_x_forwarded_proto | Body | Boolean | - | X-Forwarded-Proto/X-Forwarded-Portヘッダのon/off<br>デフォルト値：`true` |
| listener.enable_x_forwarded_port | Body | Boolean | - | X-Forwarded-Portヘッダのon/off<br>デフォルト値：`true` |
| listener.enable_x_forwarded_for | Body | Boolean | - | X-Forwarded-Forヘッダのon/off<br>デフォルト値：`true` |
| listener.default_tls_container_ref | Body | String | - | key-managerに登録されたTLS証明書のパス |
| listener.sni_container_refs | Body | Array | - | key-managerに登録されたSNI証明書のパス一覧 |
| listener.proxy_protocol | Body | Boolean | - | プロキシプロトコルのon/off<br>デフォルト値：`false` |
| listener.block_invalid_http_request | Body | Boolean | - | 無効なHTTPリクエストブロックのon/off<br>デフォルト値：`true` |
| listener.tls_version | Body | String | - | リスナーのTLSバージョン<br>`SSLv3`、`TLSv1.0`、`TLSv1.0_2016`、`TLSv1.1`、`TLSv1.2`、`TLSv1.3`のいずれか<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用<br>`ssl_policy_id`と共に指定する場合は、SSLポリシーの`min_tls_version`と一致する必要があります |
| listener.ssl_policy_id | Body | UUID | - | リスナーに接続するSSLポリシーID<br>接続を解除する場合は`null`を送信<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用<br>詳細は[カスタムSSLポリシー](/Network/Load%20Balancer/ko/overview/#ssl)を参照してください |
| listener.keepalive_enable | Body | Boolean | - | keepalive有効化のon/off<br>デフォルト値：`true` |

<details><summary>例</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "block_invalid_http_request": true,
    "description": "",
    "name": "",
    "default_pool_id": null,
    "admin_state_up": true,
    "connection_limit": 2000,
    "keepalive_timeout": 300,
    "keepalive_enable": true,
    "enable_x_forwarded_proto": true,
    "enable_x_forwarded_port": true,
    "enable_x_forwarded_for": true,
    "tls_version": "TLSv1.2",
    "ssl_policy_id": "b5b3f6f2-6c29-4f3a-9a2e-3b2e6b2b5c0a",
    "default_tls_container_ref": "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/c8f4503c-1da5-4ec7-9456-51183bd4ad4e",
    "sni_container_refs": []
  }
}
```
</p>
</details>

<a id="modify-listener-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| listener | Body | Object | リスナー情報オブジェクト |
| listener.default_pool_id | Body | UUID | リスナーに登録されたデフォルトメンバーグループ(プール)のID |
| listener.protocol | Body | Enum | リスナーのプロトコル<br>`TCP`、`HTTP`、`HTTPS`、`TERMINATED_HTTPS`のいずれか |
| listener.protocol_version | Body | Enum | HTTP プロトコルバージョン<br>`HTTP/1`、`HTTP/2` のいずれか |
| listener.description | Body | String | リスナーの説明 |
| listener.name | Body | String | リスナーの名前 |
| listener.loadbalancers | Body | Array | リスナーが登録されたロードバランサーオブジェクトの一覧 |
| listener.loadbalancers.id | Body | UUID | ロードバランサーID |
| listener.tenant_id | Body | String | テナントID |
| listener.admin_state_up | Body | Boolean | 管理者制御ステータス |
| listener.connection_limit | Body | Integer | リスナーのconnection limit |
| listener.keepalive_timeout | Body | Integer | リスナーのkeepalive timeout |
| listener.enable_x_forwarded_proto | Body | Boolean | X-Forwarded-Proto/X-Forwarded-Protヘッダのon/off |
| listener.enable_x_forwarded_port | Body | Boolean | X-Forwarded-Portヘッダのon/off |
| listener.enable_x_forwarded_for | Body | Boolean | X-Forwarded-Forヘッダのon/off |
| listener.default_tls_container_ref | Body | String | key-managerに登録されたTLS証明書のパス |
| listener.sni_container_refs | Body | Array | key-managerに登録されたSNI証明書のパス一覧 |
| listener.protocol_port | Body | Integer | リスナーポート |
| listener.proxy_protocol | Body | Boolean | プロキシプロトコルのon/off<br>デフォルト値：`false` |
| listener.block_invalid_http_request | Body | Boolean | 無効なHTTPリクエストブロックのon/off<br>デフォルト値：`true` |
| listener.tls_version | Body | String | リスナーのTLSバージョン<br>`SSLv3`、`TLSv1.0`、`TLSv1.0_2016`、`TLSv1.1`、`TLSv1.2`、`TLSv1.3`のいずれか<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用 |
| listener.ssl_policy_id | Body | UUID | リスナーに接続されたSSLポリシーID<br>接続されたSSLポリシーがない場合は`null`<br>プロトコルが`TERMINATED_HTTPS`の場合にのみ適用 |
| listener.keepalive_enable | Body | Boolean | keepalive有効化のon/off<br>デフォルト値：`true` |
| listener.id | Body | UUID | リスナーID |


<details><summary>例</summary>
<p>

```json
{
  "listener": {
    "proxy_protocol": false,
    "block_invalid_http_request": true,
    "default_pool_id": null,
    "protocol": "TERMINATED_HTTPS",
    "protocol_version": "HTTP/2",
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
    "keepalive_enable": true,
    "enable_x_forwarded_proto": true,
    "enable_x_forwarded_port": true,
    "enable_x_forwarded_for": true,
    "tls_version": "TLSv1.2",
    "ssl_policy_id": "b5b3f6f2-6c29-4f3a-9a2e-3b2e6b2b5c0a",
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
<a id="delete-listener"></a>
### リスナーの削除 { #delete-listener }
指定したリスナーを削除します。
```
DELETE /v2.0/lbaas/listeners/{listenerId}
X-Auth-Token: {tokenId}
```

<a id="delete-listener-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |

<a id="delete-listener-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

---

<a id="create-custom-response"></a>
### カスタムレスポンスの作成 { #create-custom-response }

```
POST /v2.0/lbaas/listeners/{listenerId}/errorpages
X-Auth-Token: {tokenId}
```

<a id="create-custom-response-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |
| errorpage | Body | Object | O | カスタムレスポンス情報オブジェクト |
| errorpage.code | Body | Integer | O | エラーコード<br>`400`、`403`、`408`、`500`、`502`、`503`、`504`のいずれか |
| errorpage.content_type | Body | Enum | O | コンテンツタイプ<br>`application/javascript`、`application/json`、`text/css`、`text/html`、`text/plain`のいずれか |
| errorpage.body | Body | String | O | カスタムレスポンスボディ(1024文字以内) |

!!! tip "ポイント"
    同一のリスナーに重複するコードは作成できません。(例：504を複数作成する場合)

<details><summary>例</summary>
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

<a id="create-custom-response-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| errorpage | Body | Object | カスタムレスポンス情報オブジェクト |
| errorpage.id | Body | UUID | カスタムレスポンスID |
| errorpage.code | Body | Integer | エラーコード |
| errorpage.content_type | Body | Enum | コンテンツタイプ |
| errorpage.body | Body | String | カスタムレスポンスボディ |


<details><summary>例</summary>
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

<a id="modify-custom-response"></a>
### カスタムレスポンスの修正 { #modify-custom-response }

```
PUT /v2.0/lbaas/listeners/{listenerId}/errorpages/{errorpageId}
X-Auth-Token: {tokenId}
```

<a id="modify-custom-response-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |
| errorpageId | URL | UUID | O | カスタムレスポンスID |
| errorpage | Body | Object | O | カスタムレスポンス情報オブジェクト |
| errorpage.content_type | Body | Enum | O | コンテンツタイプ<br>`application/javascript`、`application/json`、`text/css`、`text/html`、`text/plain`のいずれか |
| errorpage.body | Body | String | O | カスタムレスポンスボディ(1024文字以内) |

!!! tip "ポイント"
    `code`は変更できません。

<details><summary>例</summary>
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

<a id="modify-custom-response-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| errorpage | Body | Object | カスタムレスポンス情報オブジェクト |
| errorpage.id | Body | UUID | カスタムレスポンスID |
| errorpage.code | Body | Integer | エラーコード |
| errorpage.content_type | Body | Enum | コンテンツタイプ |
| errorpage.body | Body | String | カスタムレスポンスボディ |


<details><summary>例</summary>
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

<a id="delete-custom-response"></a>
### カスタムレスポンスの削除 { #delete-custom-response }

```
DELETE /v2.0/lbaas/listeners/{listenerId}/errorpages/{errorpageId}
X-Auth-Token: {tokenId}
```

<a id="delete-custom-response-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |
| errorpageId | URL | UUID | O | カスタムレスポンスID |

<a id="delete-custom-response-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

---

<a id="view-custom-response"></a>
### カスタムレスポンスの表示 { #view-custom-response }

```
GET /v2.0/lbaas/listeners/{listenerId}/errorpages/{errorpageId}
X-Auth-Token: {tokenId}
```

<a id="view-custom-response-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |
| errorpageId | URL | UUID | O | カスタムレスポンスID |

<a id="view-custom-response-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| errorpage | Body | Object | カスタムレスポンス情報オブジェクト |
| errorpage.id | Body | UUID | カスタムレスポンスID |
| errorpage.code | Body | Integer | エラーコード |
| errorpage.content_type | Body | Enum | コンテンツタイプ |
| errorpage.body | Body | String | カスタムレスポンスボディ |


<details><summary>例</summary>
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

<a id="view-custom-response-list"></a>
### カスタムレスポンス一覧の表示 { #view-custom-response-list }

```
GET /v2.0/lbaas/listeners/{listenerId}/errorpages
X-Auth-Token: {tokenId}
```

<a id="view-custom-response-list-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| listenerId | URL | UUID | O | リスナーID |

<a id="view-custom-response-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| errorpages | Body | Array | カスタムレスポンス情報オブジェクトの一覧 |
| errorpages.id | Body | UUID | カスタムレスポンスID |
| errorpages.code | Body | Integer | エラーコード |
| errorpages.content_type | Body | Enum | コンテンツタイプ |
| errorpages.body | Body | String | カスタムレスポンスボディ |
| errorpages.tenant_id | Body | String | テナントID |

<details><summary>例</summary>
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

<a id="pool"></a>
## プール { #pool }
<a id="view-pool-list"></a>
### プール一覧表示 { #view-pool-list }

```
GET /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

<a id="view-pool-list-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | プールID |
| name | Query | String | - | プールの名前 |
| lb_algorithm | Query | Enum | - | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のいずれか |
| protocol | Query | Enum | - | メンバーのプロトコル |
| protocol_version | Query | Enum | - | メンバーのHTTPプロトコルバージョン |
| admin_state_up | Query | Boolean | - | 管理者制御ステータス |
| healthmonitor_id | Query | UUID | - | プールのヘルスモニターID |

<a id="view-pool-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| pools | Body | Array | プール情報オブジェクトの一覧 |
| pools.lb_algorithm | Body | Enum | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のいずれか |
| pools.protocol | Body | Enum | メンバーのプロトコル |
| pools.protocol_version | Body | Enum | メンバーの HTTP プロトコルバージョン |
| pools.description | Body | String | プールの説明 |
| pools.admin_state_up | Body | Boolean | 管理者制御ステータス |
| pools.tenant_id | Body | String | テナントID |
| pools.session_persistence | Body | Object | プールのセッション維持オブジェクト |
| pools.session_persistence.type | Body | Enum | セッション維持<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のいずれかに設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定する場合、接続されたリスナーのプロトコルを`HTTP`または`TERMINATED_HTTPS`に設定しているか確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション維持を`HTTP_COOKIE`、`APP_COOKIE`に設定しても、ロードバランサーはセッション維持に関連する動作を実行しません。 |
| pools.session_persistence.cookie_name | Body | String | Cookie名 <br>セッション維持タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pools.healthmonitor_id | Body | String | ヘルスモニターID |
| pools.loadbalancers | Body | Array | プールが登録されたロードバランサーオブジェクトの一覧 |
| pools.loadbalancers.id | Body | UUID | ロードバランサーID |
| pools.listeners | Body | Array | プールが登録されたリスナーオブジェクトの一覧 |
| pools.listeners.id | Body | String | リスナーID |
| pools.members | Body | Array | プールに登録されたメンバーオブジェクトの一覧 |
| pools.members.id | Body | String | メンバーID |
| pools.id | Body | UUID | プールID |
| pools.name | Body | String | プールの名前 |

<details><summary>例</summary>
<p>

```json
{
  "pools": [
    {
      "lb_algorithm": "ROUND_ROBIN",
      "protocol": "HTTP",
      "protocol_version": "HTTP/2",
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


<a id="view-pool"></a>
### プール表示 { #view-pool }

```
GET /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

<a id="view-pool-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |

<a id="view-pool-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| pool | Body | Object | プール情報オブジェクト |
| pool.lb_algorithm | Body | Enum | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のいずれか |
| pool.protocol | Body | Enum | メンバーのプロトコル |
| pool.protocol_version | Body | Enum | メンバーのHTTPプロトコルバージョン |
| pool.description | Body | String | プールの説明 |
| pool.admin_state_up | Body | Boolean | 管理者制御ステータス |
| pool.tenant_id | Body | String | テナントID |
| pool.member_port | Body | Integer | メンバーのポート<br> Webコンソールでメンバーを作成する場合に指定されるメンバーのポート値 |
| pool.session_persistence | Body | Object | プールのセッション維持オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション維持<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のいずれかに設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定する場合、接続されたリスナーのプロトコルを`HTTP`または`TERMINATED_HTTPS`に設定しているか確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション維持を`HTTP_COOKIE`、`APP_COOKIE`に設定しても、ロードバランサーはセッション維持に関連する動作を実行しません。 |
| pool.session_persistence.cookie_name | Body | String | Cookie名 <br>セッション維持タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pool.healthmonitor_id | Body | UUID | ヘルスモニターID |
| pool.loadbalancers | Body | Array | プールが登録されたロードバランサーオブジェクトの一覧 |
| pool.loadbalancers.id | Body | UUID | ロードバランサーID |
| pool.listeners | Body | Array | プールが登録されたリスナーオブジェクトの一覧 |
| pool.listeners.id | Body | UUID | リスナーID |
| pool.members | Body | Array | プールに登録されたメンバーオブジェクトの一覧 |
| pool.members.id | Body | UUID | メンバーID |
| pool.id | Body | UUID | プールID |
| pool.name | Body | String | プールの名前 |

<details><summary>例</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "protocol_version": "HTTP/1",
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
<a id="create-pool"></a>
### プール作成 { #create-pool }

```
POST /v2.0/lbaas/pools
X-Auth-Token: {tokenId}
```

<a id="create-pool-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| pool | Body | Object | O | プール情報オブジェクト |
| pool.loadbalancer_id | Body | UUID | - | プールが登録されるロードバランサーID。ロードバランサーIDまたはリスナーIDのいずれかは必須で入力する必要があります。 |
| pool.listener_id | Body | UUID | - | プールが登録されるリスナーID。ロードバランサーIDまたはリスナーIDのいずれかは必須で入力する必要があります。 |
| pool.lb_algorithm | Body | Enum | O | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のいずれか |
| pool.protocol | Body | Enum | O | メンバーのプロトコル |
| pool.protocol_version | Body | Enum | - | メンバーのHTTPプロトコルバージョン |
| pool.description | Body | String | - | プールの説明 |
| pool.admin_state_up | Body | Boolean | - | 管理者制御ステータス |
| pool.member_port | Body | Integer | - | メンバーの受信ポート<br>トラフィックをこのポートに転送します。<br>デフォルト値は-1です。 |
| pool.session_persistence | Body | Object | - | プールのセッション維持オブジェクト |
| pool.session_persistence.type | Body | Enum | - | セッション維持<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のいずれかに設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定する場合、接続されたリスナーのプロトコルを`HTTP`または`TERMINATED_HTTPS`に設定しているか確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション維持を`HTTP_COOKIE`、`APP_COOKIE`に設定しても、ロードバランサーはセッション維持に関連する動作を実行しません。 |
| pools.session_persistence.cookie_name | Body | String | - | Cookie名 <br>セッション維持タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pool.name | Body | String | - | プールの名前 |



<details><summary>例</summary>
<p>

```json
{
  "pool": {
    "listener_id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "protocol_version": "HTTP/1",
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

<a id="create-pool-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| pool | Body | Object | プール情報オブジェクト |
| pool.lb_algorithm | Body | Enum | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のいずれか |
| pool.protocol | Body | Enum | メンバーのプロトコル |
| pool.protocol_version | Body | Enum | メンバーの HTTP プロトコルバージョン |
| pool.description | Body | String | プールの説明 |
| pool.admin_state_up | Body | Boolean | 管理者制御ステータス |
| pool.tenant_id | Body | String | テナントID |
| pool.session_persistence | Body | Object | - | プールのセッション維持オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション維持<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のいずれかに設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定する場合、接続されたリスナーのプロトコルを`HTTP`または`TERMINATED_HTTPS`に設定しているか確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション維持を`HTTP_COOKIE`、`APP_COOKIE`に設定しても、ロードバランサーはセッション維持に関連する動作を実行しません。 |
| pool.healthmonitor_id | Body | String | ヘルスモニターID |
| pool.loadbalancers | Body | Array | プールが登録されたロードバランサーオブジェクトの一覧 |
| pool.loadbalancers.id | Body | UUID | ロードバランサーID |
| pool.listeners | Body | Array | プールが登録されたリスナーオブジェクトの一覧 |
| pool.listeners.id | Body | UUID | リスナーID |
| pool.members | Body | Array | プールに登録されたメンバーオブジェクトの一覧 |
| pool.members.id | Body | UUID | メンバーID |
| pool.id | Body | UUID | プールID |
| pool.name | Body | String | プールの名前 |

<details><summary>例</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "protocol_version": "HTTP/1",
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
<a id="modify-pool"></a>
### プール変更 { #modify-pool }

```
PUT /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```


<a id="modify-pool-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | プールID |
| pool | Body | Object | O | プール情報オブジェクト |
| pool.lb_algorithm | Body | Enum | - | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のいずれか |
| pool.description | Body | String | - | プールの説明 |
| pool.admin_state_up | Body | Boolean | - | 管理者制御ステータス |
| pool.session_persistence | Body | Object | - | プールのセッション維持オブジェクト |
| pool.session_persistence.type | Body | Enum | - | セッション維持<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のいずれかに設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定する場合、接続されたリスナーのプロトコルを`HTTP`または`TERMINATED_HTTPS`に設定しているか確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション維持を`HTTP_COOKIE`、`APP_COOKIE`に設定しても、ロードバランサーはセッション維持に関連する動作を実行しません。 |
| pools.session_persistence.cookie_name | Body | String | - | Cookie名 <br>セッション維持タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pool.name | Body | String | - | プールの名前 |



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

<a id="modify-pool-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| pool | Body | Object | プール情報オブジェクト |
| pool.lb_algorithm | Body | Enum | プールのロードバランシング方式 <br> `ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のいずれか |
| pool.protocol | Body | Enum | メンバーのプロトコル |
| pool.protocol_version | Body | Enum | メンバーのHTTPプロトコルバージョン |
| pool.description | Body | String | プールの説明 |
| pool.admin_state_up | Body | Boolean | 管理者制御ステータス |
| pool.tenant_id | Body | String | テナントID |
| pools.session_persistence | Body | Object | プールのセッション維持オブジェクト |
| pool.session_persistence.type | Body | Enum | セッション維持<br> `SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のいずれかに設定<br> `HTTP_COOKIE`、`APP_COOKIE`に設定する場合、接続されたリスナーのプロトコルを`HTTP`または`TERMINATED_HTTPS`に設定しているか確認することを推奨します。<br> リスナーのプロトコルを`TCP`または`HTTPS`に設定した場合、セッション維持を`HTTP_COOKIE`、`APP_COOKIE`に設定しても、ロードバランサーはセッション維持に関連する動作を実行しません。 |
| pools.session_persistence.cookie_name | Body | String | Cookie名 <br>セッション維持タイプが`APP_COOKIE`の場合にのみ設定値が適用されます。 |
| pool.healthmonitor_id | Body | UUID | ヘルスモニターID |
| pool.loadbalancers | Body | Array | プールが登録されたロードバランサーオブジェクトの一覧 |
| pool.loadbalancers.id | Body | UUID | ロードバランサーID |
| pool.listeners | Body | Array | プールが登録されたリスナーオブジェクトの一覧 |
| pool.listeners.id | Body | UUID | リスナーID |
| pool.members | Body | Array | プールに登録されたメンバーオブジェクトの一覧 |
| pool.members.id | Body | UUID | メンバーID |
| pool.id | Body | UUID | プールID |
| pool.name | Body | String | プールの名前 |

<details><summary>例</summary>
<p>

```json
{
  "pool": {
    "lb_algorithm": "ROUND_ROBIN",
    "protocol": "HTTP",
    "protocol_version": "HTTP/1",
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
<a id="delete-pool"></a>
### プール削除 { #delete-pool }
指定したプールを削除します。
```
DELETE /v2.0/lbaas/pools/{poolId}
X-Auth-Token: {tokenId}
```

<a id="delete-pool-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | プールID |

<a id="delete-pool-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

<a id="health-monitor"></a>
## ヘルスモニター { #health-monitor }
<a id="view-health-monitor-list"></a>
### ヘルスモニター一覧表示 { #view-health-monitor-list }

```
GET /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

<a id="view-health-monitor-list-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | ヘルスモニターID |
| admin_state_up | Query | Boolean | - | 管理者制御ステータス |
| delay | Query | Integer | - | ヘルスチェック間隔(秒) |
| expected_codes | Query | String | - | 正常状態と見なすメンバーのHTTPレスポンスコード <br> 単一値(200)、一覧(201,202)、または範囲(201-204)で使用可能<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| max_retries | Query | Integer | - | 最大再試行回数 |
| http_method | Query | Enum | - | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| timeout | Query | Integer | - | ヘルスチェックの応答待機時間(秒) |
| url_path | Query | String | - | ヘルスチェックのリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| type | Query | Enum | - | ヘルスチェックに使用するプロトコル。`TCP`、`HTTP`、`HTTPS`のいずれか |
| host_header | Query | String | - | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|



<a id="view-health-monitor-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| healthmonitors | Body | Array | ヘルスモニター情報オブジェクトの一覧 |
| healthmonitors.admin_state_up | Body | Boolean | 管理者制御ステータス |
| healthmonitors.delay | Body | Integer | ヘルスチェック間隔(秒) |
| healthmonitors.health_check_port | Body | Integer | ヘルスチェックの対象となるメンバーポート <br> * `member-port` または 0 の場合、各メンバーに指定されたポート番号を対象にヘルスチェックを実行します。 <br> * 正数の場合、各メンバーに指定されたポート番号に関係なく、入力されたポート番号でヘルスチェックを実行します。|
| healthmonitors.expected_codes | Body | String | 正常状態と見なすメンバーのHTTPレスポンスコード <br> 単一値(200)、一覧(201,202)、または範囲(201-204)で使用可能<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。 |
| healthmonitors.max_retries | Body | Integer | 最大再試行回数 |
| healthmonitors.http_method | Body | Enum | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitors.timeout | Body | Integer | ヘルスチェックの応答待機時間(秒) |
| healthmonitors.pools | Body | Array | ヘルスモニターが接続されたプールオブジェクトの一覧 |
| healthmonitors.pools.id | Body | UUID | プールID |
| healthmonitors.url_path | Body | String | ヘルスチェックのリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitors.type | Body | Enum | ヘルスチェックに使用するプロトコル。`TCP`、`HTTP`、`HTTPS`のいずれか |
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


<a id="view-health-monitor"></a>
### ヘルスモニター表示 { #view-health-monitor }

```
GET /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

<a id="view-health-monitor-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| healthMonitorId | URL | UUID | O | ヘルスモニターID |

<a id="view-health-monitor-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| healthmonitor | Body | Object | ヘルスモニター情報オブジェクト |
| healthmonitor.admin_state_up | Body | Boolean | 管理者制御ステータス |
| healthmonitor.delay | Body | Integer | ヘルスチェック間隔(秒) |
| healthmonitor.health_check_port | Body | Integer | ヘルスチェックの対象となるメンバーポート <br> * `member-port` または 0 の場合、各メンバーに指定されたポート番号を対象にヘルスチェックを実行します。 <br> * 正数の場合、各メンバーに指定されたポート番号に関係なく、入力されたポート番号でヘルスチェックを実行します。|
| healthmonitor.expected_codes | Body | String | 正常状態と見なすメンバーのHTTPレスポンスコード <br> 単一値(200)、一覧(201,202)、または範囲(201-204)で使用可能<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.max_retries | Body | Integer | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.timeout | Body | Integer | ヘルスチェックの応答待機時間(秒) |
| healthmonitor.pools | Body | Array | ヘルスモニターが接続されたプールオブジェクトの一覧 |
| healthmonitor.pools.id | Body | UUID | プールID |
| healthmonitor.url_path | Body | String | ヘルスチェックのリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.type | Body | Enum | ヘルスチェックに使用するプロトコル。`TCP`、`HTTP`、`HTTPS`のいずれか |
| healthmonitor.id | Body | UUID | ヘルスモニターID |
| healthmonitor.host_header | Body | String | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|



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
<a id="create-health-monitor"></a>
### ヘルスモニター作成 { #create-health-monitor }

```
POST /v2.0/lbaas/healthmonitors
X-Auth-Token: {tokenId}
```

<a id="create-health-monitor-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| healthmonitor | Body | Object | O | ヘルスモニター情報オブジェクト |
| healthmonitor.pool_id | Body | UUID | O | ヘルスモニターが接続されるプールID |
| healthmonitor.admin_state_up | Body | Boolean | - | 管理者制御ステータス |
| healthmonitor.health_check_port | Body | Integer | - | ヘルスチェックの対象となるメンバーポート <br> * `member-port` または 0 を指定すると、各メンバーに指定されたポート番号を対象にヘルスチェックを実行します。 <br> * 正数を入力すると、各メンバーに指定されたポート番号に関係なく、入力されたポート番号でヘルスチェックを実行します。|
| healthmonitor.delay | Body | Integer | O | ヘルスチェック間隔(秒) |
| healthmonitor.expected_codes | Body | String | - | 正常状態と見なすメンバーのHTTPレスポンスコード。省略した場合は200に設定されます。<br> 単一値(200)、一覧(201,202)、または範囲(201-204)で使用可能<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.max_retries | Body | Integer | O | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | - | ヘルスチェックに使用するHTTP Method。省略した場合は`GET`が使用されます。 <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.timeout | Body | Integer | O | ヘルスチェックの応答待機時間(秒) |
| healthmonitor.url_path | Body | String | - | ヘルスチェックのリクエストURL。省略した場合は`/`が設定されます。 <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.type | Body | Enum  | O | ヘルスチェックに使用するプロトコル。`TCP`、`HTTP`、`HTTPS`のいずれか |
| healthmonitor.host_header | Body | String | - | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|

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

<a id="create-health-monitor-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| healthmonitor | Body | Object | ヘルスモニター情報オブジェクト |
| healthmonitor.admin_state_up | Body | Boolean | 管理者制御ステータス |
| healthmonitor.delay | Body | Integer | ヘルスチェック間隔(秒) |
| healthmonitor.health_check_port | Body | Integer | ヘルスチェックの対象となるメンバーポート <br> * `member-port` または 0 の場合、各メンバーに指定されたポート番号を対象にヘルスチェックを実行します。 <br> * 正数の場合、各メンバーに指定されたポート番号に関係なく、入力されたポート番号でヘルスチェックを実行します。|
| healthmonitor.expected_codes | Body | String | 正常状態と見なすメンバーのHTTPレスポンスコード。省略した場合は200に設定されます。<br> 単一値(200)、一覧(201,202)、または範囲(201-204)で使用可能<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.max_retries | Body | Integer | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.timeout | Body | Integer | ヘルスチェックの応答待機時間(秒) |
| healthmonitor.pools | Body | Array | ヘルスモニターが接続されたプールオブジェクトの一覧 |
| healthmonitor.pools.id | Body | UUID | プールID |
| healthmonitor.url_path | Body | String | ヘルスチェックのリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.type | Body | Enum | ヘルスチェックに使用するプロトコル。`TCP`、`HTTP`、`HTTPS`のいずれか |
| healthmonitor.id | Body | UUID | ヘルスモニターID |
| healthmonitor.host_header | Body | String | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|



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
<a id="modify-health-monitor"></a>
### ヘルスモニター変更 { #modify-health-monitor }

```
PUT /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

<a id="modify-health-monitor-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| healthmonitorId | URL | UUID | O | ヘルスモニターID |
| healthmonitor | Body | Object | O | ヘルスモニター情報オブジェクト |
| healthmonitor.admin_state_up | Body | Boolean | - | 管理者制御ステータス |
| healthmonitor.health_check_port | Body | Integer | - | ヘルスチェックの対象となるメンバーポート <br> * `member-port` または 0 に指定すると、各メンバーに指定されたポート番号を対象にヘルスチェックを実行します。 <br> * 正数を入力すると、各メンバーに指定されたポート番号に関係なく、入力されたポート番号でヘルスチェックを実行します。|
| healthmonitor.delay | Body | Integer | - | ヘルスチェック間隔(秒) |
| healthmonitor.expected_codes | Body | String | - | 正常状態と見なすメンバーのHTTPレスポンスコード<br>単一値(200)、一覧(201,202)、または範囲(201-204)で使用可能<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.max_retries | Body | Integer | - | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | - | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.timeout | Body | Integer | - | ヘルスチェックの応答待機時間(秒) |
| healthmonitor.url_path | Body | String | - | ヘルスチェックのリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.host_header | Body | String | - | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|


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

<a id="modify-health-monitor-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| healthmonitor | Body | Object | ヘルスモニター情報オブジェクト |
| healthmonitor.admin_state_up | Body | Boolean | 管理者制御ステータス |
| healthmonitor.delay | Body | Integer | ヘルスチェック間隔(秒) |
| healthmonitor.health_check_port | Body | Integer | ヘルスチェックの対象となるメンバーポート <br> * `member-port` または 0 の場合、各メンバーに指定されたポート番号を対象にヘルスチェックを実行します。 <br> * 正数の場合、各メンバーに指定されたポート番号に関係なく、入力されたポート番号でヘルスチェックを実行します。|
| healthmonitor.expected_codes | Body | String | 正常状態と見なすメンバーのHTTPレスポンスコード<br>単一値(200)、一覧(201,202)、または範囲(201-204)で使用可能<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.max_retries | Body | Integer | 最大再試行回数 |
| healthmonitor.http_method | Body | Enum | ヘルスチェックに使用するHTTP Method <br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.timeout | Body | Integer | ヘルスチェックの応答待機時間(秒) |
| healthmonitor.pools | Body | Array | ヘルスモニターが接続されたプールオブジェクトの一覧 |
| healthmonitor.pools.id | Body | UUID | プールID |
| healthmonitor.url_path | Body | String | ヘルスチェックのリクエストURL<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|
| healthmonitor.type | Body | Enum | ヘルスチェックに使用するプロトコル。`TCP`、`HTTP`、`HTTPS`のいずれか |
| healthmonitor.id | Body | UUID | ヘルスモニターID |
| healthmonitor.host_header | Body | String | ヘルスチェックに使用するホストヘッダのフィールド値<br> ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視されます。|


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
<a id="delete-health-monitor"></a>
### ヘルスモニター削除 { #delete-health-monitor }

```
DELETE /v2.0/lbaas/healthmonitors/{healthMonitorId}
X-Auth-Token: {tokenId}
```

<a id="delete-health-monitor-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| healthMonitorId | URL | UUID | O | ヘルスモニターID |

<a id="delete-health-monitor-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

<a id="member"></a>
## メンバー { #member }
<a id="view-member-list"></a>
### メンバー一覧表示 { #view-member-list }

```
GET /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

<a id="view-member-list-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属するプールID |
| id | Query | UUID | - | メンバーID |
| weight | Query | Integer | - | メンバーの重み |
| admin_state_up | Query | Boolean | - | 管理者制御ステータス |
| subnet_id | Query | UUID | - | メンバーのサブネットID |
| tenant_id | Query | String | - | テナントID |
| address | Query | String | - | メンバーのIPアドレス |
| protocol_port | Query | Integer | - | メンバーのポート |
| operating_status | Query | Enum | - | メンバーの運用ステータス |


<a id="view-member-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| members | Body | Array | メンバー情報オブジェクトの一覧 |
| members.weight | Body | Integer | メンバーの重み |
| members.admin_state_up | Body | Boolean | 管理者制御ステータス |
| members.subnet_id | Body | UUID | メンバーのサブネットID |
| members.tenant_id | Body | String | テナントID |
| members.address | Body | String | メンバーのIPアドレス |
| members.protocol_port | Body | Integer | メンバーのポート |
| members.id | Body | UUID | メンバーID |
| members.operating_status | Body | Enum | メンバーの運用ステータス |

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


<a id="view-member"></a>
### メンバー表示 { #view-member }

```
GET /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

<a id="view-member-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属するプールID |
| memberId | URL | UUID | O | メンバーID |

<a id="view-member-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| member | Body | Object | メンバー情報オブジェクト |
| member.weight | Body | Integer | メンバーの重み |
| member.admin_state_up | Body | Boolean | 管理者制御ステータス |
| member.subnet_id | Body | UUID | メンバーのサブネットID |
| member.tenant_id | Body | String | テナントID |
| member.address | Body | String | メンバーのIPアドレス |
| member.protocol_port | Body | Integer | メンバーのポート |
| member.id | Body | UUID | メンバーID |
| member.operating_status | Body | Enum | メンバーの運用ステータス |

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
<a id="create-member"></a>
### メンバー作成 { #create-member }

```
POST /v2.0/lbaas/pools/{poolId}/members
X-Auth-Token: {tokenId}
```

<a id="create-member-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属するプールID |
| member | Body | Object | O | メンバー情報オブジェクト |
| member.weight | Body | Integer | - | メンバーの重み |
| member.admin_state_up | Body | Boolean | - | 管理者制御ステータス |
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

<a id="create-member-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| member | Body | Object | メンバー情報オブジェクト |
| member.weight | Body | Integer | メンバーの重み |
| member.admin_state_up | Body | Boolean | 管理者制御ステータス |
| member.subnet_id | Body | UUID | メンバーのサブネットID |
| member.tenant_id | Body | String | テナントID |
| member.address | Body | String | メンバーのIPアドレス |
| member.protocol_port | Body | Integer | メンバーのポート |
| member.id | Body | UUID | メンバーID |
| member.operating_status | Body | Enum | メンバーの運用ステータス |

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
<a id="modify-member"></a>
### メンバーの修正 { #modify-member }

```
PUT /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

<a id="modify-member-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属するプールID |
| memberId | URL | UUID | O | メンバーID |
| member | Body | Object | O | メンバー情報オブジェクト |
| member.weight | Body | Integer | - | メンバーの重み |
| member.admin_state_up | Body | Boolean | - | 管理者制御ステータス |

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

<a id="modify-member-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| member | Body | Object | メンバー情報オブジェクト |
| member.weight | Body | Integer | メンバーの重み |
| member.admin_state_up | Body | Boolean | 管理者制御ステータス |
| member.subnet_id | Body | UUID | メンバーのサブネットID |
| member.tenant_id | Body | String | テナントID |
| member.address | Body | String | メンバーのIPアドレス |
| member.protocol_port | Body | Integer | メンバーのポート |
| member.id | Body | UUID | メンバーID |
| member.operating_status | Body | Enum | メンバーの運用ステータス |

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
<a id="delete-member"></a>
### メンバーの削除 { #delete-member }

```
DELETE /v2.0/lbaas/pools/{poolId}/members/{memberId}
X-Auth-Token: {tokenId}
```

<a id="delete-member-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| poolId | URL | UUID | O | メンバーが属するプールID |
| memberId | URL | UUID | O | メンバーID |

<a id="delete-member-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

<a id="l7-polilcy"></a>
## L7ポリシー { #l7-polilcy }

<a id="view-l7-policy-list"></a>
### L7ポリシー一覧の表示 { #view-l7-policy-list }

```
GET /v2.0/lbaas/l7policies
X-Auth-Token: {tokenId}
```

<a id="view-l7-policy-list-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するL7ポリシーID |
| name | Query | String | - | 照会するL7ポリシー名 |
| description | Query | String | - | 照会するL7ポリシーの説明 |
| listener_id | Query | UUID | - | 照会するL7ポリシーのリスナーID |
| action | Query | Enum | - | 照会するL7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| redirect_pool_id | Query | UUID | - | 照会するL7ポリシーのリダイレクトプールID<br>アクションが `REDIRECT_TO_POOL`の場合にのみ適用 |
| redirect_url | Query | String | - | 照会するL7ポリシーのリダイレクトURL<br>アクションが `REDIRECT_TO_URL`の場合にのみ適用 |
| redirect_http_code | Query | Integer | - | L7ポリシーのリダイレクトHTTPレスポンスコード |
| position | Query | Integer | - | 照会するL7ポリシーの優先順位 |


<a id="view-l7-policy-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| l7policies | Body | Array | L7ポリシーオブジェクト一覧 |
| l7policies.description | Body | String | L7ポリシーの説明 |
| l7policies.tenant_id | Body | String | テナントID |
| l7policies.listener_id | Body | UUID | L7ポリシーのリスナーID |
| l7policies.name | Body | String | L7ポリシー名 |
| l7policies.rules | Body | Object | L7ポリシールールオブジェクト一覧 |
| l7policies.rules.id | Body | UUID | L7ルールID |
| l7policies.id | Body | UUID | L7ポリシーID |
| l7policies.admin_state_up | Body | Boolean | L7ポリシー管理者制御ステータス |
| l7policies.action | Body | Enum | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policies.redirect_pool_id | Body | UUID | L7ポリシーのリダイレクトプールID<br>アクションが `REDIRECT_TO_POOL`の場合にのみ適用 |
| l7policies.redirect_url | Body | String | L7ポリシーのリダイレクトURL<br>アクションが `REDIRECT_TO_URL`の場合にのみ適用 |
| l7policies.redirect_http_code | Body | Integer | - | L7ポリシーのリダイレクトHTTPレスポンスコード |
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
<a id="view-l7-policy"></a>
### L7ポリシーの表示 { #view-l7-policy }

```
GET /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

<a id="view-l7-policy-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |

<a id="view-l7-policy-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| l7policy | Body | Object | L7ポリシーオブジェクト |
| l7policy.description | Body | String | L7ポリシーの説明 |
| l7policy.tenant_id | Body | String | テナントID |
| l7policy.listener_id | Body | UUID | L7ポリシーのリスナーID |
| l7policy.name | Body | String | L7ポリシー名 |
| l7policy.rules | Body | Object | L7ポリシールールオブジェクト一覧 |
| l7policy.rules.id | Body | UUID | L7ルールID |
| l7policy.id | Body | UUID | L7ポリシーID |
| l7policy.admin_state_up | Body | Boolean | L7ポリシー管理者制御ステータス |
| l7policy.action | Body | Enum | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policy.redirect_pool_id | Body | UUID | L7ポリシーのリダイレクトプールID<br>アクションが `REDIRECT_TO_POOL`の場合にのみ適用 |
| l7policy.redirect_url | Body | String | L7ポリシーのリダイレクトURL<br>アクションが `REDIRECT_TO_URL`の場合にのみ適用 |
| l7policy.redirect_http_code | Body | Integer | - | L7ポリシーのリダイレクトHTTPレスポンスコード |
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
<a id="create-l7-policy"></a>
### L7ポリシーの作成 { #create-l7-policy }

```
POST /v2.0/lbaas/l7policies
X-Auth-Token: {tokenId}
```

<a id="create-l7-policy-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policy | Body | Object | - | L7ポリシーオブジェクト |
| l7policy.description | Body | String | - | L7ポリシーの説明 |
| l7policy.listener_id | Body | UUID | O | L7ポリシーのリスナーID |
| l7policy.name | Body | String | - | L7ポリシー名 |
| l7policy.admin_state_up | Body | Boolean | - | L7ポリシー管理者制御ステータス。省略すると `true` に設定されます |
| l7policy.action | Body | Enum | O | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policy.redirect_pool_id | Body | UUID | - | L7ポリシーのリダイレクトプールID<br>アクションが `REDIRECT_TO_POOL`の場合は必須 |
| l7policy.redirect_url | Body | String | - | L7ポリシーのリダイレクトURL<br>アクションが `REDIRECT_TO_URL`の場合は必須 <br> * 入力可能なフォーマットは `#{protocol}://#{host}:#{port}/#{path}?#{query}` 形式であり、`#{_}` 形式で入力すると既存のリクエストの値を維持します。`#{_}` 以外の値を直接入力した場合、リダイレクトURLに該当する値が適用されてクライアントに返却されます。 <br> * 無限リダイレクトを防止するため、protocol、host、port、pathのうち少なくとも1つ以上は変更する必要があります。 <br> * 正しくない形式で入力した場合、リダイレクトURLが実際の入力とは異なる値に変換される可能性があります。 |
| l7policy.redirect_http_code | Body | Integer | - | L7ポリシーのリダイレクトHTTPレスポンスコード <br> 301、302のいずれか。デフォルト値は302 |
| l7policy.position | Body | Integer | - | L7ポリシーの優先順位。省略した場合は最下位に設定されます |



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

<a id="create-l7-policy-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| l7policy | Body | Object | L7ポリシーオブジェクト |
| l7policy.description | Body | String | L7ポリシーの説明 |
| l7policy.tenant_id | Body | String | テナントID |
| l7policy.listener_id | Body | UUID | L7ポリシーのリスナーID |
| l7policy.name | Body | String | L7ポリシー名 |
| l7policy.rules | Body | Object | L7ポリシールールオブジェクト一覧 |
| l7policy.rules.id | Body | UUID | L7ルールID |
| l7policy.id | Body | UUID | L7ポリシーID |
| l7policy.admin_state_up | Body | Boolean | L7ポリシー管理者制御ステータス |
| l7policy.action | Body | Enum | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policy.redirect_pool_id | Body | UUID | L7ポリシーのリダイレクトプールID<br>アクションが `REDIRECT_TO_POOL`の場合にのみ適用 |
| l7policy.redirect_url | Body | String | L7ポリシーのリダイレクトURL<br>アクションが `REDIRECT_TO_URL`の場合にのみ適用 |
| l7policy.redirect_http_code | Body | Integer | - | L7ポリシーのリダイレクトHTTPレスポンスコード |
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
<a id="modify-l7-policy"></a>
### L7ポリシーの修正 { #modify-l7-policy }

```
PUT /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

<a id="modify-l7-policy-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| l7policy | Body | Object | O | L7ポリシーオブジェクト |
| l7policy.name | Body | String | - | L7ポリシー名 |
| l7policy.description | Body | String | - | L7ポリシーの説明 |
| l7policy.admin_state_up | Body | Boolean | - | L7ポリシーの管理者制御ステータス |
| l7policy.action | Body | Enum | - | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policy.redirect_pool_id | Body | UUID | - | L7ポリシーのリダイレクトプールID<br>アクションが `REDIRECT_TO_POOL`の場合は必須 |
| l7policy.redirect_url | Body | String | - | L7ポリシーのリダイレクトURL<br>アクションが `REDIRECT_TO_URL`の場合は必須 |
| l7policy.redirect_http_code | Body | Integer | - | L7ポリシーのリダイレクトHTTPレスポンスコード |
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

<a id="modify-l7-policy-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| l7policy | Body | Object | L7ポリシーオブジェクト |
| l7policy.description | Body | String | L7ポリシーの説明 |
| l7policy.tenant_id | Body | String | テナントID |
| l7policy.listener_id | Body | UUID | L7ポリシーのリスナーID |
| l7policy.name | Body | String | L7ポリシー名 |
| l7policy.rules | Body | Object | L7ポリシールールオブジェクト一覧 |
| l7policy.rules.id | Body | UUID | L7ルールID |
| l7policy.id | Body | UUID | L7ポリシーID |
| l7policy.admin_state_up | Body | Boolean | L7ポリシー管理者制御ステータス |
| l7policy.action | Body | Enum | L7ポリシーのアクション<br> `REDIRECT_TO_POOL`/`REDIRECT_TO_URL`/`REJECT`のいずれか |
| l7policy.redirect_pool_id | Body | UUID | L7ポリシーのリダイレクトプールID<br>アクションが `REDIRECT_TO_POOL`の場合にのみ適用 |
| l7policy.redirect_url | Body | String | L7ポリシーのリダイレクトURL<br>アクションが `REDIRECT_TO_URL`の場合にのみ適用 |
| l7policy.redirect_http_code | Body | Integer | - | L7ポリシーのリダイレクトHTTPレスポンスコード |
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
<a id="delete-l7-policy"></a>
### L7ポリシーの削除 { #delete-l7-policy }

```
DELETE /v2.0/lbaas/l7policies/{l7policyId}
X-Auth-Token: {tokenId}
```

<a id="delete-l7-policy-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |


<a id="delete-l7-policy-response"></a>
#### レスポンス
このAPIはレスポンスボディ(Body)を返却しません。

<a id="l7-rule"></a>
## L7ルール { #l7-rule }

<a id="view-l7-rule-list"></a>
### L7ルール一覧の表示 { #view-l7-rule-list }

```
GET /v2.0/lbaas/l7policies/{l7policyId}/rules
X-Auth-Token: {tokenId}
```

<a id="view-l7-rule-list-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ルールが属するL7ポリシーID |
| id | Query | UUID | - | 照会するL7ルールID |
| type | Query | Enum | - | 照会するL7ルールのタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| compare_type | Query | Enum | - | 照会するL7ルールの比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


<a id="view-l7-rule-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| rules | Body | Array | L7ルールオブジェクト一覧 |
| rules.tenant_id | Body | String | テナントID |
| rules.id | Body | UUID | L7ルールID |
| rules.admin_state_up | Body | Boolean | L7ルール管理者制御ステータス |
| rules.invert | Body | Boolean | マッチング結果に対する反転(invert)設定 |
| rules.key | Body | String | L7ルールのマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合にのみ適用 |
| rules.value | Body | String | L7ルールのマッチング時に使用される値 |
| rules.type | Query | Enum | L7ルールのタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rules.compare_type | Query | Enum | L7ルールの比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |

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
<a id="view-l7-rule"></a>
### L7ルールの表示 { #view-l7-rule }

```
GET /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

<a id="view-l7-rule-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| l7ruleId | URL | UUID | O | L7ルールID |

<a id="view-l7-rule-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| rule | Body | Object | L7ルールオブジェクト |
| rule.tenant_id | Body | String | テナントID |
| rule.id | Body | UUID | L7ルールID |
| rule.admin_state_up | Body | Boolean | L7ルール管理者制御ステータス |
| rule.invert | Body | Boolean | マッチング結果に対する反転(invert)設定 |
| rule.key | Body | String | L7ルールのマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合にのみ適用 |
| rule.value | Body | String | L7ルールのマッチング時に使用される値 |
| rule.type | Query | Enum | L7ルールのタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rule.compare_type | Query | Enum | L7ルールの比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


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
<a id="create-l7-rule"></a>
### L7ルールの作成 { #create-l7-rule }

```
POST /v2.0/lbaas/l7policies/{l7policyId}/rules
X-Auth-Token: {tokenId}
```

<a id="create-l7-rule-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| rule | Body | Object | O | L7ルールオブジェクト |
| rule.admin_state_up | Body | Boolean | - | L7ルール管理者制御ステータス |
| rule.invert | Body | Boolean | - | マッチング結果に対する反転(invert)設定。省略すると `true` に設定されます |
| rule.key | Body | String | - | L7ルールのマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合は必須 |
| rule.value | Body | String | O | L7ルールのマッチング時に使用される値 |
| rule.type | Query | Enum | O | L7ルールのタイプ <br> `COOKIE`/ `FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rule.compare_type | Query | Enum | O | L7ルールの比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


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

<a id="create-l7-rule-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| rule | Body | Object | L7ルールオブジェクト |
| rule.tenant_id | Body | String | テナントID |
| rule.id | Body | UUID | L7ルールID |
| rule.admin_state_up | Body | Boolean | L7ルール管理者制御ステータス |
| rule.invert | Body | Boolean | マッチング結果に対する反転(invert)設定 |
| rule.key | Body | String | L7ルールのマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合にのみ適用 |
| rule.value | Body | String | L7ルールのマッチング時に使用される値 |
| rule.type | Query | Enum | L7ルールのタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rule.compare_type | Query | Enum | L7ルールの比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


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
<a id="modify-l7-rule"></a>
### L7ルールの修正 { #modify-l7-rule }

```
PUT /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

<a id="modify-l7-rule-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| l7ruleId | URL | UUID | O | L7ルールID |
| rule | Body | Object | O | L7ルールオブジェクト |
| rule.admin_state_up | Body | Boolean | - | L7ルール管理者制御ステータス |
| rule.invert | Body | Boolean | - | マッチング結果に対する反転(invert)設定 |
| rule.key | Body | String | - | L7ルールのマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合にのみ適用 |
| rule.value | Body | String | - | L7ルールのマッチング時に使用される値 |
| rule.type | Query | Enum | - | L7ルールのタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rule.compare_type | Query | Enum | - | L7ルールの比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


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

<a id="modify-l7-rule-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| rule | Body | Object | L7ルールオブジェクト |
| rule.tenant_id | Body | String | テナントID |
| rule.id | Body | UUID | L7ルールID |
| rule.admin_state_up | Body | Boolean | L7ルール管理者制御ステータス |
| rule.invert | Body | Boolean | マッチング結果に対する反転(invert)設定 |
| rule.key | Body | String | L7ルールのマッチング時に使用されるキー<br> `COOKIE`/`HEADER`の場合にのみ適用 |
| rule.value | Body | String | L7ルールのマッチング時に使用される値 |
| rule.type | Query | Enum | L7ルールのタイプ <br> `COOKIE`/`FILE_TYPE`/`HEADER`/`HOST_NAME`/`PATH`のいずれか |
| rule.compare_type | Query | Enum | L7ルールの比較方式<br> `CONTAINS`/`ENDS_WITH`/`STARTS_WITH`/`EQUAL_TO`/`REGEX`のいずれか |


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
<a id="delete-l7-rule"></a>
### L7ルールの削除 { #delete-l7-rule }

```
DELETE /v2.0/lbaas/l7policies/{l7policyId}/rules/{l7ruleId}
X-Auth-Token: {tokenId}
```

<a id="delete-l7-rule-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| l7policyId | URL | UUID | O | L7ポリシーID |
| l7ruleId | URL | UUID | O | L7ルールID |


<a id="delete-l7-rule-response"></a>
#### レスポンス
このAPIはレスポンスボディ(Body)を返却しません。

<a id="secret"></a>
## シークレット { #secret }

シークレットAPIは、`key-manager`タイプのエンドポイントを利用して呼び出します。正確なエンドポイントは、トークン発行レスポンスの`serviceCatalog`を参照してください。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| key-manager | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>韓国(クァンジュ)リージョン<br>日本(東京)リージョン |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr3-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com |

APIレスポンスにガイドに明記されていないフィールドが表示される場合があります。このようなフィールドはNHN Cloudの内部用途で使用されており、事前の通知なしに変更される可能性があるため、使用しないでください。


<a id="view-secret-list"></a>
### シークレット一覧の表示 { #view-secret-list }

シークレット一覧を返却します。

```
GET /v1/secrets
X-Auth-Token: {tokenId}
```

<a id="view-secret-list-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| offset | Query | Integer | - | レスポンス一覧のオフセット。デフォルト値：0 |
| limit | Query | Integer| - | レスポンス一覧に表示する最大件数。デフォルト値：10 |
| name | Query | String | - | シークレット名 |
| alg | Query | String | - | シークレットアルゴリズム |
| mode | Query | String| - | ブロック暗号の運用方式 |
| bits | Query | Integer| - | 暗号化鍵長 |

<a id="view-secret-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| secrets | Body | Array | シークレットオブジェクト一覧 |
| secrets.secret_ref | Body | String | シークレットのパス<br>`<barbican endpoint>/v1/secrets/<secret id>` 形式 |
| secrets.secret_type | Body | Enum | シークレットタイプ <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` のいずれか |
| secrets.status | Body | Enum | シークレットのステータス |
| secrets.content_types | Body | Array | シークレットペイロードのコンテンツタイプ一覧 |
| secrets.content_types.default | Body | String | コンテンツタイプのデフォルト値 |
| secrets.creator_id | Body | String | シークレットを作成したユーザーID |
| secrets.mode | Body | String | ブロック暗号の運用方式。ユーザー入力のメタデータ |
| secrets.algorithm | Body | String | 暗号化アルゴリズム。ユーザー入力のメタデータ |
| secrets.bit_length | Body | Integer | 暗号化鍵長。ユーザー入力のメタデータ |
| secrets.expiration | Body | Datetime | 有効期限。ユーザー入力のメタデータ <br>`YYYY-MM-DDThh:mm:ss`<br> 有効期限が切れたシークレットは自動的に削除処理されます |
| secrets.name| Body | String | シークレット名 |
| secrets.created | Body | Datetime | 作成日時 <br> `YYYY-MM-DDThh:mm:ss` |
| secrets.updated | Body | Datetime | 変更日時 <br> `YYYY-MM-DDThh:mm:ss` |
| total | Body | Integer | リクエストクエリの総シークレット数 |
| next | Body | String | 現在表示されている一覧の次の一覧のURL |
| previous | Body | String | 現在表示されている一覧の前の一覧のURL |

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


<a id="view-secret"></a>
### シークレットの表示 { #view-secret }
指定したシークレット情報を返却します。
```
GET /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

<a id="view-secret-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| secretId | URL | UUID | O | シークレットID |

<a id="view-secret-response"></a>
#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| secret | Body | Object | シークレットオブジェクト |
| secret.secret_ref | Body | String | シークレットのパス<br>`<barbican endpoint>/v1/secrets/<secret id>` 形式 |
| secret.secret_type | Body | Enum | シークレットタイプ <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` のいずれか |
| secret.status | Body | Enum | シークレットのステータス |
| secret.content_types | Body | Array | シークレットペイロードのコンテンツタイプ一覧 |
| secret.content_types.default | Body | String | コンテンツタイプのデフォルト値 |
| secret.creator_id | Body | String | シークレットを作成したユーザーID |
| secret.mode | Body | String | ブロック暗号の運用方式。ユーザー入力のメタデータ |
| secret.algorithm | Body | String | 暗号化アルゴリズム。ユーザー入力のメタデータ |
| secret.bit_length | Body | Integer | 暗号化鍵長。ユーザー入力のメタデータ |
| secret.expiration | Body | Datetime | 有効期限。ユーザー入力のメタデータ <br>`YYYY-MM-DDThh:mm:ss`<br> 有効期限が切れたシークレットは自動的に削除されます |
| secret.name| Body | String | シークレット名 |
| secret.created | Body | Datetime | 作成日時 <br> `YYYY-MM-DDThh:mm:ss` |
| secret.updated | Body | Datetime | 変更日時 <br> `YYYY-MM-DDThh:mm:ss` |

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
<a id="create-secret"></a>
### シークレットの作成 { #create-secret }
新しいシークレットを作成します。
```
POST /v1/secrets
X-Auth-Token: {tokenId}
```

<a id="create-secret-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| name | Body | String | - | シークレット名 |
| expiration | Body | Datetime | - | 有効期限。ISO8601形式でリクエスト |
| algorithm | Body | String | - | 暗号化アルゴリズム |
| bit_length | Body | String | - | 暗号化鍵長 |
| mode | Body | String | - | ブロック暗号の運用方式 |
| payload | Body | String | - | 暗号化鍵ペイロード |
| payload_content_type | Body | String | - | 暗号化鍵ペイロードのコンテンツタイプ<br> payloadを入力する場合は必須です。 <br>サポートするコンテンツタイプ一覧：`text/plain`、`application/octet-stream`、`application/pkcs8`、`application/pkix-cert` |
| payload_content_encoding | Body | Enum | - | 暗号化鍵ペイロードのエンコーディング方式 <br>payload_content_typeがtext/plainではない場合は必須です。<br> `base64` のみサポート |
| secret_type | Body | Enum | - | シークレットタイプ <br> `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` のいずれか |



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

textでペイロードを送信
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

base64でペイロードを送信
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

<a id="create-secret-response"></a>
#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| secret_ref | Body | String | シークレットのパス<br>`<barbican endpoint>/v1/secrets/<secret id>` 形式 |

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
<a id="modify-secret"></a>
### シークレットの修正 { #modify-secret }
既存のメタデータのみ入力されたシークレットのペイロードデータを入力します。
```
PUT /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
Content-Type: {ConetentType}
```

<a id="modify-secret-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| secretId | URL | UUID | O | シークレットID |
| ContentType| Header | Enum | O | `text/plain`、`application/octet-stream`、`application/pkcs8`、`application/pkix-cert` のいずれか<br> 省略時は `text/plain` に設定されます |
| payload | Body | String | O | 暗号化鍵ペイロード |

<details><summary>例</summary>
```
{
	"payload": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANQE .... nyxm\n-----END PRIVATE KEY-----\n"
}
```
</details>

<a id="modify-secret-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

---
<a id="delete-secret"></a>
### シークレットの削除 { #delete-secret }
指定したシークレットを削除します。
```
DELETE /v1/secrets/{secretId}
X-Auth-Token: {tokenId}
```

<a id="delete-secret-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| secretId | URL | UUID | O | シークレットID |

<a id="delete-secret-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

<a id="secret-container"></a>
## シークレットコンテナ { #secret-container }

シークレットコンテナAPIは、`key-manager`タイプのエンドポイントを利用して呼び出します。正確なエンドポイントは、トークン発行レスポンスの`serviceCatalog`を参照してください。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| key-manager | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>韓国(クァンジュ)リージョン<br>日本(東京)リージョン |https://kr1-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr2-api-key-manager-infrastructure.nhncloudservice.com<br>https://kr3-api-key-manager-infrastructure.nhncloudservice.com<br>https://jp1-api-key-manager-infrastructure.nhncloudservice.com |

APIレスポンスにガイドに明記されていないフィールドが表示される場合があります。このようなフィールドはNHN Cloudの内部用途で使用されており、事前の通知なしに変更される可能性があるため、使用しないでください。


<a id="view-secret-container-list"></a>
### シークレットコンテナ一覧の表示 { #view-secret-container-list }

シークレットコンテナ一覧を返却します。

```
GET /v1/containers
X-Auth-Token: {tokenId}
```

<a id="view-secret-container-list-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| offset | Query | Integer | - | レスポンス一覧のオフセット。デフォルト値：0 |
| limit | Query | Integer | - | レスポンス一覧に表示する最大件数。デフォルト値：10 |

<a id="view-secret-container-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| containers | Body | Array | コンテナオブジェクト一覧 |
| containers.status | Body | Enum | コンテナのステータス |
| containers.updated | Body | Datetime | 変更日時 `YYYY-MM-DDThh:mm:ss` |
| containers.name | Body | String | コンテナ名 |
| containers.consumers | Body | Array | コンシューマー一覧 |
| containers.consumers.URL | Body | String | コンシューマーURL |
| containers.consumers.name | Body | String | コンシューマー名 |
| containers.created | Body | Datetime | 作成日時 `YYYY-MM-DDThh:mm:ss` |
| containers.container_ref | Body | String | コンテナのパス |
| containers.creator_id | Body | String | コンテナを作成したユーザーID |
| containers.secret_refs | Body | Array | シークレット一覧 |
| containers.secret_refs.secret_ref | Body | String | シークレットのパス |
| containers.secret_refs.name | Body | String | コンテナが指定したシークレット名<br> コンテナタイプが `certificate` の場合：`certificate`、`private_key`、`private_key_passphrase`、`intermediates` に指定<br> コンテナタイプが `rsa` の場合：`private_key`、`private_key_passphrase`、`public_key` に指定 |
| containers.type | Body | Enum | コンテナタイプ<br> `generic`, `rsa`, `certificate` のいずれか |
| containers.common_name | Body | String | コンテナに登録された証明書のCommon Name<br>コンテナタイプが `certificate` の場合のみ表示 |
| containers.expiration | Body | Datetime | コンテナに登録された証明書の有効期限<br>コンテナタイプが `certificate` の場合のみ表示。例：`YYYY-MM-DDThh:mm:ss` |
| total | Body | Integer | リクエストクエリのシークレットコンテナの総数 |
| next | Body | String | 現在表示されている一覧の次の一覧のURL |
| previous | Body | String | 現在表示されている一覧の前の一覧のURL |



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


<a id="view-secret-container"></a>
### シークレットコンテナの表示 { #view-secret-container }
指定したシークレットコンテナ情報を返却します。
```
GET /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

<a id="view-secret-container-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| containerId | URL | UUID | O | シークレットコンテナID |

<a id="view-secret-container-response"></a>
#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| status | Body | Enum | コンテナのステータス |
| updated | Body | Datetime | 更新時刻 `YYYY-MM-DDThh:mm:ss` |
| name | Body | String | コンテナ名 |
| consumers | Body | Array | コンシューマー一覧 |
| consumers.URL | Body | String | コンシューマーURL |
| consumers.name | Body | String | コンシューマー名 |
| created | Body | Datetime | 作成時刻 `YYYY-MM-DDThh:mm:ss` |
| container_ref | Body | String | コンテナアドレス |
| creator_id | Body | String | コンテナを作成したユーザーID |
| secret_refs | Body | Array | コンテナに登録したシークレット一覧 |
| secret_refs.secret_ref | Body | String | シークレットアドレス |
| secret_refs.name | Body | String | コンテナが指定したシークレット名<br>コンテナタイプが`certificate`の場合：`certificate`、`private_key`、`private_key_passphrase`、`intermediates`に指定<br>コンテナタイプが`rsa`の場合：`private_key`、`private_key_passphrase`、`public_key`に指定 |
| type | Body | Enum | コンテナタイプ<br>`generic`、`rsa`、`certificate`のいずれか |
| common_name | Body | String | コンテナに登録された証明書のCommon Name<br>コンテナタイプが`certificate`の場合のみ表示 |
| expiration | Body | Datetime | コンテナに登録された証明書の有効期限<br>コンテナタイプが`certificate`の場合のみ表示。例：`YYYY-MM-DDThh:mm:ss` |


<details><summary>例</summary>

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
<a id="create-secret-container"></a>
### シークレットコンテナ作成 { #create-secret-container }
新しいシークレットコンテナを作成します。
```
POST /v1/containers
X-Auth-Token: {tokenId}
```

<a id="create-secret-container-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| type | Body | Enum | O | コンテナタイプ<br>`generic`、`rsa`、`certificate`のいずれか |
| name | Body | String | - | コンテナ名 |
| secret_refs | Body | Array | - | コンテナに登録するシークレット一覧 |
| secret_refs.secret_ref | Body | String | - | シークレットアドレス |
| secret_refs.name | Body | String | - | コンテナが指定したシークレット名<br>コンテナタイプが`certificate`の場合：`certificate`、`private_key`、`private_key_passphrase`、`intermediates`に指定<br>コンテナタイプが`rsa`の場合：`private_key`、`private_key_passphrase`、`public_key`に指定 |


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

<a id="create-secret-container-response"></a>
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
<a id="delete-secret-container"></a>
### シークレットコンテナ削除 { #delete-secret-container }
指定したシークレットコンテナを削除します。
```
DELETE /v1/containers/{containerId}
X-Auth-Token: {tokenId}
```

<a id="delete-secret-container-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| containerId | URL | UUID | シークレットコンテナID |


<a id="delete-secret-container-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

<a id="ip-acl-group"></a>
## IP ACLグループ { #ip-acl-group }

<a id="view-ip-acl-group-list"></a>
### IP ACLグループ一覧 { #view-ip-acl-group-list }

IP ACLグループ一覧を返却します。

```
GET /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

<a id="view-ip-acl-group-list-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| id | Query | String | - | IP ACLグループID |
| name | Query | String | - | IP ACLグループ名 |
| description | Query | String | - | IP ACLグループの説明 |
| action | Query | Enum | - | IP ACLグループの制御アクション<br>`ALLOW`、`DENY`のいずれか |

<a id="view-ip-acl-group-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_groups | Body | Array | IP ACLグループオブジェクト一覧 |
| ipacl_groups.ipacl_target_count | Body | String | IP ACLグループに含まれるターゲット数 |
| ipacl_groups.description | Body | String | IP ACLグループの説明 |
| ipacl_groups.loadbalancers | Body | Object | IP ACLグループが適用されたロードバランサーオブジェクト一覧 |
| ipacl_groups.loadbalancers.loadbalancer_id | Body | String | ロードバランサーID |
| ipacl_groups.tenant_id | Body | String | テナントID |
| ipacl_groups.action | Body | Enum | IPアクセス制御グループの制御アクション<br>`ALLOW`、`DENY`のいずれか |
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

<a id="view-ip-acl-group"></a>
### IP ACLグループ表示 { #view-ip-acl-group }

指定したIP ACLグループを返却します。

```
GET /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

<a id="view-ip-acl-group-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclGroupId | Header | String | O | トークンID |

<a id="view-ip-acl-group-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACLグループオブジェクト |
| ipacl_group.ipacl_target_count | Body | String | IP ACLグループに含まれるターゲット数 |
| ipacl_group.description | Body | String | IP ACLグループの説明 |
| ipacl_group.loadbalancers | Body | Object | IP ACLグループが適用されたロードバランサーオブジェクト一覧 |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | ロードバランサーID |
| ipacl_group.tenant_id | Body | String | テナントID |
| ipacl_group.action | Body | Enum | IP ACLグループの制御アクション<br>`ALLOW`、`DENY`のいずれか |
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

<a id="create-ip-acl-group"></a>
### IP ACLグループ作成 { #create-ip-acl-group }

新しいIP ACLグループを作成します。

```
POST /v2.0/lbaas/ipacl-groups
X-Auth-Token: {tokenId}
```

<a id="create-ip-acl-group-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipacl_group | Body | Object | O | IP ACLグループオブジェクト |
| ipacl_group.description | Body | String | - | IP ACLグループの説明 |
| ipacl_group.action | Body | Enum | O | IP ACLグループの制御アクション<br>`ALLOW`、`DENY`のいずれか |
| ipacl_group.name | Body | String | - | IP ACLグループ名 |
| ipacl_group.ipacl_targets | Body | Object | - | IP ACLターゲットオブジェクト。値を入力するとターゲットも一緒に作成されます |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (ipacl_targetsオブジェクトが追加された場合) | IP ACLターゲットのCIDR<br>単独のIPアドレス、またはCIDR形式のIP RANGEを入力 |
| ipacl_group.ipacl_targets.descripion | Body | String | - | IP ACLターゲットの説明 |

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

<a id="create-ip-acl-group-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACLグループオブジェクト |
| ipacl_group.ipacl_target_count | Body | String | IP ACLグループに含まれるターゲット数 |
| ipacl_group.description | Body | String | IP ACLグループの説明 |
| ipacl_group.loadbalancers | Body | String | IP ACLグループが適用されたロードバランサーオブジェクト一覧 |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | ロードバランサーID |
| ipacl_group.tenant_id | Body | String | テナントID |
| ipacl_group.action | Body | Enum | IP ACLグループの制御アクション<br>`ALLOW`、`DENY`のいずれか |
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

<a id="modify-ip-acl-group"></a>
### IP ACLグループ修正 { #modify-ip-acl-group }

既存のIP ACLグループを修正します。
ipacl_group.actionは変更できません。
下位のIP ACLターゲット一覧を全体的に置き換える際に、このAPIを使用できます。
ただし、IP ACLグループに属していた既存の全てのターゲットが削除され、入力したターゲット一覧に置き換えられます。 
入力したターゲットの`cidr_address`は重複してはいけません。

```
PUT /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

<a id="modify-ip-acl-group-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclGroupId | URL | UUID | O | IP ACLグループID |
| ipacl_group | Body | String | O | IP ACLグループオブジェクト |
| ipacl_group.name | Body | String | - | IP ACLグループ名 |
| ipacl_group.description | Body | String | - | IP ACLグループの説明 |
| ipacl_group.ipacl_targets | Body | Object | - | IP ACLターゲットオブジェクト。値を入力するとターゲットも一緒に作成されます |
| ipacl_group.ipacl_targets.cidr_address | Body | String | O (ipacl_targetsオブジェクトが追加された場合) | IP ACLターゲットのCIDR<br>単独のIPアドレス、またはCIDR形式のIP RANGEを入力 |
| ipacl_group.ipacl_targets.descripion | Body | String | - | IP ACLターゲットの説明 |


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

<a id="modify-ip-acl-group-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_group | Body | Object | IP ACLグループオブジェクト |
| ipacl_group.ipacl_target_count | Body | String | IP ACLグループに含まれるターゲット数 |
| ipacl_group.description | Body | String | IP ACLグループの説明 |
| ipacl_group.loadbalancers | Body | String | IP ACLグループが適用されたロードバランサーオブジェクト一覧 |
| ipacl_group.loadbalancers.loadbalancer_id | Body | String | ロードバランサーID |
| ipacl_group.tenant_id | Body | String | テナントID |
| ipacl_group.action | Body | Enum | IP ACLグループの制御アクション<br>`ALLOW`、`DENY`のいずれか |
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

<a id="delete-ip-acl-group"></a>
### IP ACLグループ削除 { #delete-ip-acl-group }

指定したIP ACLグループを削除します。

```
DELETE /v2.0/lbaas/ipacl-groups/{ipaclGroupId}
X-Auth-Token: {tokenId}
```

IP ACLグループの削除時に、下位のIP ACLターゲットも全て削除されます。
削除されるIP ACLグループを使用している全てのロードバランサーから、このIP ACLグループに関連するルールが削除されます。

<a id="delete-ip-acl-group-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclGroupId | URL | UUID | O | IP ACLグループID |

<a id="delete-ip-acl-group-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

- - -


<a id="apply-ip-acl-group-to-load-balancer"></a>
### ロードバランサーにIP ACLグループを適用 { #apply-ip-acl-group-to-load-balancer }

ロードバランサーにIP ACLグループを適用します。
IP ACLグループが適用されたロードバランサーには、グループに含まれるIP ACLターゲットのルールが適用されます。
複数のグループをロードバランサーに適用できます。ただし、グループの`action`は全て同一である必要があります。
既存のロードバランサーに適用されていたIP ACLグループは全て削除され、入力されたグループ一覧で再適用されます。

```
PUT /v2.0/lbaas/loadbalancers/{lb_id}/bind_ipacl_groups
X-auth-Token: {tokenId}
```

<a id="apply-ip-acl-group-to-load-balancer-request"></a>
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

<a id="apply-ip-acl-group-to-load-balancer-response"></a>
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

<a id="ip-acl-target"></a>
## IP ACLターゲット { #ip-acl-target }

<a id="view-ip-acl-target-list"></a>
### IP ACLターゲット一覧 { #view-ip-acl-target-list }

IP ACLターゲット一覧を返却します。

```
GET /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

<a id="view-ip-acl-target-list-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| id | Query | String | - | IP ACLターゲットID |
| cidr_address | Query | String | - | IP ACLターゲットのCIDR<br>単独のIPアドレス、またはCIDR形式のIP RANGE |
| ipacl_group_id | Query | String | - | IP ACLグループID |
| description | Query | String | - | IP ACLグループの説明 |

<a id="view-ip-acl-target-list-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_targets | Body | Array | IP ACLターゲット情報オブジェクト一覧 |
| ipacl_targets.ipacl_group_id | Body | UUID | IP ACLグループID |
| ipacl_targets.tenant_id | Body | String | テナントID |
| ipacl_targets.cidr_address | Body | String | IP ACLターゲットのCIDR |
| ipacl_targets.description | Body | String | IP ACLターゲットの説明 |
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

<a id="view-ip-acl-target"></a>
### IP ACLターゲット表示 { #view-ip-acl-target }

指定したIP ACLターゲット情報を返却します。

```
GET /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

<a id="view-ip-acl-target-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclTargetId | URL | UUID | O | IP ACLターゲットID |

<a id="view-ip-acl-target-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_target | Body | Array | IP ACLターゲット情報オブジェクト |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACLグループID |
| ipacl_target.tenant_id | Body | String | テナントID |
| ipacl_target.cidr_address | Body | String | IP ACLターゲットのCIDR<br>単独のIPアドレス、またはCIDR形式のIP RANGE |
| ipacl_target.description | Body | String | IP ACLターゲットの説明 |
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

<a id="create-ip-acl-target"></a>
### IP ACLターゲット作成 { #create-ip-acl-target }

IP ACLターゲットを作成します。

```
POST /v2.0/lbaas/ipacl-targets
X-Auth-Token: {tokenId}
```

<a id="create-ip-acl-target-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipacl_target | Body | Object | O | IP ACLターゲット情報オブジェクト |
| ipacl_target.ipacl_group_id | Body | UUID | O | IP ACLグループID |
| ipacl_target.cidr_address | Body | String | O | IP ACLターゲットのCIDR<br>単独のIPアドレス、またはCIDR形式のIP RANGE |
| ipacl_target.description | Body | String | - | IP ACLターゲットの説明 |

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

<a id="create-ip-acl-target-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_target | Body | Object | IP ACLターゲット情報オブジェクト |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACLグループID |
| ipacl_target.tenant_id | Body | String | テナントID |
| ipacl_target.cidr_address | Body | String | IP ACLターゲットのCIDR<br>単独のIPアドレス、またはCIDR形式のIP RANGE |
| ipacl_target.description | Body | String | IP ACLターゲットの説明 |
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

<a id="modify-ip-acl-target"></a>
### IP ACLターゲット修正 { #modify-ip-acl-target }

既存のIP ACLターゲットを変更します。
`description`のみ変更できます。

```
PUT /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

<a id="modify-ip-acl-target-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclTargetId | URL | UUID | O | IP ACLターゲットID |
| ipacl_target | Body | Object | O | IP ACLターゲット情報オブジェクト |
| ipacl_target.description | Body | String | - | IP ACLターゲットの説明 |

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

<a id="modify-ip-acl-target-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| ipacl_target | Body | Object | IP ACLターゲット情報オブジェクト |
| ipacl_target.ipacl_group_id | Body | UUID | IP ACLグループID |
| ipacl_target.tenant_id | Body | String | テナントID |
| ipacl_target.cidr_address | Body | String | IP ACLターゲットのCIDR<br>単独のIPアドレス、またはCIDR形式のIP RANGE |
| ipacl_target.description | Body | String | IP ACLターゲットの説明 |
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

<a id="delete-ip-acl-target"></a>
### IP ACLターゲット削除 { #delete-ip-acl-target }

指定したIP ACLターゲットを削除します。

```
DELETE /v2.0/lbaas/ipacl-targets/{ipaclTargetId}
X-Auth-Token: {tokenId}
```

<a id="delete-ip-acl-target-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| ipaclTargetId | URL | UUID | O | IP ACLターゲットID |

<a id="delete-ip-acl-target-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

- - -

<a id="ssl-policy"></a>
## SSLポリシー { #ssl-policy }

カスタムSSLポリシーを作成してリスナーに適用できます。SSLポリシーには、最小TLSバージョンと、該当バージョンで使用する暗号化スイート(cipher suite)を指定します。SSLポリシーの概念と選択可能な暗号化スイートの一覧については、[カスタムSSLポリシー](/Network/Load%20Balancer/ko/overview/#ssl)をご参照ください。

!!! tip "ポイント"
    - SSLポリシーはテナントあたり最大10個まで作成できます。
    - SSLポリシーは、プロトコルが`TERMINATED_HTTPS`であるリスナーにのみ適用されます。

<a id="list-ssl-policies"></a>
### SSLポリシー一覧 { #list-ssl-policies }

```
GET /v2.0/lbaas/ssl_policies
X-Auth-Token: {tokenId}
```

<a id="list-ssl-policies-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するSSLポリシーID |
| name | Query | String | - | 照会するSSLポリシー名 |
| description | Query | String | - | 照会するSSLポリシーの説明 |
| min_tls_version | Query | Enum | - | 照会するSSLポリシーの最小TLSバージョン |

<a id="list-ssl-policies-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| ssl_policies | Body | Array | SSLポリシーオブジェクト一覧 |
| ssl_policies.id | Body | UUID | SSLポリシーID |
| ssl_policies.tenant_id | Body | String | テナントID |
| ssl_policies.name | Body | String | SSLポリシー名 |
| ssl_policies.description | Body | String | SSLポリシーの説明 |
| ssl_policies.min_tls_version | Body | Enum | SSLポリシーの最小TLSバージョン<br>`SSLv3`、`TLSv1.0`、`TLSv1.0_2016`、`TLSv1.1`、`TLSv1.2`、`TLSv1.3`のいずれか |
| ssl_policies.ciphers | Body | String | 使用する暗号化スイート一覧<br>TLS 1.2以下の暗号化スイートとTLS 1.3の暗号化スイートを`:`で連結した1つの文字列<br>レスポンスは、TLS 1.2以下の暗号化スイートが先、TLS 1.3の暗号化スイートが後にくる順序に正規化されて返却されます |
| ssl_policies.listeners | Body | Array | SSLポリシーが適用されたリスナー一覧 |
| ssl_policies.listeners.id | Body | UUID | リスナーID |
| ssl_policies.listeners.loadbalancer_id | Body | UUID | リスナーが属するロードバランサーID |
| ssl_policies.created_at | Body | String | 作成時刻 |
| ssl_policies.updated_at | Body | String | 最終更新時刻 |

<details><summary>例</summary>

```json
{
  "ssl_policies": [
    {
      "id": "b5b3f6f2-6c29-4f3a-9a2e-3b2e6b2b5c0a",
      "tenant_id": "8258ab391d854e8b878642b737017a3b",
      "name": "secure-tls12",
      "description": "TLS 1.2以上のみ許可",
      "min_tls_version": "TLSv1.2",
      "ciphers": "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384",
      "listeners": [
        {
          "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
          "loadbalancer_id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
        }
      ],
      "created_at": "2026-04-01T10:00:00",
      "updated_at": "2026-04-01T10:00:00"
    }
  ]
}
```

</details>

- - -

<a id="get-ssl-policy"></a>
### SSLポリシー表示 { #get-ssl-policy }

```
GET /v2.0/lbaas/ssl_policies/{sslPolicyId}
X-Auth-Token: {tokenId}
```

<a id="get-ssl-policy-request"></a>
#### リクエスト
このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| sslPolicyId | URL | UUID | O | SSLポリシーID |

<a id="get-ssl-policy-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| ssl_policy | Body | Object | SSLポリシーオブジェクト |
| ssl_policy.id | Body | UUID | SSLポリシーID |
| ssl_policy.tenant_id | Body | String | テナントID |
| ssl_policy.name | Body | String | SSLポリシー名 |
| ssl_policy.description | Body | String | SSLポリシーの説明 |
| ssl_policy.min_tls_version | Body | Enum | SSLポリシーの最小TLSバージョン |
| ssl_policy.ciphers | Body | String | 使用する暗号化スイート一覧<br>TLS 1.2以下の暗号化スイートとTLS 1.3の暗号化スイートを`:`で連結した1つの文字列<br>レスポンスは、TLS 1.2以下の暗号化スイートが先、TLS 1.3の暗号化スイートが後にくる順序に正規化されて返却されます |
| ssl_policy.listeners | Body | Array | SSLポリシーが適用されたリスナー一覧 |
| ssl_policy.listeners.id | Body | UUID | リスナーID |
| ssl_policy.listeners.loadbalancer_id | Body | UUID | リスナーが属するロードバランサーID |
| ssl_policy.created_at | Body | String | 作成時刻 |
| ssl_policy.updated_at | Body | String | 最終更新時刻 |

<details><summary>例</summary>

```json
{
  "ssl_policy": {
    "id": "b5b3f6f2-6c29-4f3a-9a2e-3b2e6b2b5c0a",
    "tenant_id": "8258ab391d854e8b878642b737017a3b",
    "name": "secure-tls12",
    "description": "TLS 1.2以上のみ許可",
    "min_tls_version": "TLSv1.2",
    "ciphers": "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384",
    "listeners": [
      {
        "id": "1b5e4950-71ae-4d67-bf97-453f986c9a20",
        "loadbalancer_id": "7b4cef78-72b0-4c3c-9971-98763ef6284c"
      }
    ],
    "created_at": "2026-04-01T10:00:00",
    "updated_at": "2026-04-01T10:00:00"
  }
}
```

</details>

- - -

<a id="create-ssl-policy"></a>
### SSLポリシー作成 { #create-ssl-policy }

```
POST /v2.0/lbaas/ssl_policies
X-Auth-Token: {tokenId}
```

<a id="create-ssl-policy-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| ssl_policy | Body | Object | O | SSLポリシーオブジェクト |
| ssl_policy.name | Body | String | - | SSLポリシー名 |
| ssl_policy.description | Body | String | - | SSLポリシーの説明 |
| ssl_policy.min_tls_version | Body | Enum | O | SSLポリシーの最小TLSバージョン<br>`SSLv3`、`TLSv1.0`、`TLSv1.0_2016`、`TLSv1.1`、`TLSv1.2`、`TLSv1.3`のいずれか<br>作成後は変更できません |
| ssl_policy.ciphers | Body | String | O | 使用する暗号化スイート一覧<br>TLS 1.2以下の暗号化スイートとTLS 1.3の暗号化スイートを`:`で連結した1つの文字列<br>サーバーが名前プレフィックス(`TLS_`で始まればTLS 1.3)で自動分類します<br>最低1つ以上指定する必要があります |

!!! danger "注意"
    - `min_tls_version`が`TLSv1.3`の場合、`ciphers`にTLS 1.2以下の暗号化スイートを含めることはできません。含めた場合はエラーが返却されます。
    - 選択可能な暗号化スイートは、[カスタムSSLポリシー](/Network/Load%20Balancer/ko/overview/#ssl)に定義された値のみ使用できます。

<details><summary>例</summary>

```json
{
  "ssl_policy": {
    "name": "secure-tls12",
    "description": "TLS 1.2以上のみ許可",
    "min_tls_version": "TLSv1.2",
    "ciphers": "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384"
  }
}
```

</details>

<a id="create-ssl-policy-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| ssl_policy | Body | Object | 作成されたSSLポリシーオブジェクト |
| ssl_policy.id | Body | UUID | SSLポリシーID |
| ssl_policy.tenant_id | Body | String | テナントID |
| ssl_policy.name | Body | String | SSLポリシー名 |
| ssl_policy.description | Body | String | SSLポリシーの説明 |
| ssl_policy.min_tls_version | Body | Enum | SSLポリシーの最小TLSバージョン |
| ssl_policy.ciphers | Body | String | 使用する暗号化スイート一覧<br>TLS 1.2以下の暗号化スイートが先、TLS 1.3の暗号化スイートが後にくる順序に正規化されて返却されます |
| ssl_policy.listeners | Body | Array | SSLポリシーが適用されたリスナー一覧<br>作成直後は空の配列になります |
| ssl_policy.created_at | Body | String | 作成時刻 |
| ssl_policy.updated_at | Body | String | 最終更新時刻 |

- - -

<a id="modify-ssl-policy"></a>
### SSLポリシー修正 { #modify-ssl-policy }

```
PUT /v2.0/lbaas/ssl_policies/{sslPolicyId}
X-Auth-Token: {tokenId}
```

<a id="modify-ssl-policy-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| sslPolicyId | URL | UUID | O | SSLポリシーID |
| ssl_policy | Body | Object | O | SSLポリシーオブジェクト |
| ssl_policy.name | Body | String | - | SSLポリシー名 |
| ssl_policy.description | Body | String | - | SSLポリシーの説明 |
| ssl_policy.ciphers | Body | String | - | 使用する暗号化スイート一覧<br>TLS 1.2以下の暗号化スイートとTLS 1.3の暗号化スイートを`:`で連結した1つの文字列<br>リクエストに含めると、新しい値が既存の保存値を完全に上書きします(TLS 1.2以下/TLS 1.3のいずれか一方のみを修正する場合でも、両方を含める必要があります) |

!!! danger "注意"
    `min_tls_version`は作成後に変更できません。リクエストに含めるとエラーが発生します。

!!! tip "ポイント"
    SSLポリシーを修正すると、該当ポリシーが適用された全てのリスナーの設定が自動的に更新されます。

<details><summary>例</summary>

```json
{
  "ssl_policy": {
    "description": "暗号化スイートの強化",
    "ciphers": "ECDHE-RSA-AES256-GCM-SHA384:TLS_AES_256_GCM_SHA384"
  }
}
```

</details>

<a id="modify-ssl-policy-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| ssl_policy | Body | Object | 修正されたSSLポリシーオブジェクト |

レスポンスの構造は `SSLポリシー表示` と同じです。

- - -

<a id="delete-ssl-policy"></a>
### SSLポリシー削除 { #delete-ssl-policy }

```
DELETE /v2.0/lbaas/ssl_policies/{sslPolicyId}
X-Auth-Token: {tokenId}
```

<a id="delete-ssl-policy-request"></a>
#### リクエスト

このAPIはリクエスト本文(Body)を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| sslPolicyId | URL | UUID | O | SSLポリシーID |

!!! danger "注意"
    SSLポリシーが1つ以上のリスナーに適用されている場合は削除できません。先に該当リスナーの`ssl_policy_id`を`null`に修正して接続を解除した後に削除してください。

<a id="delete-ssl-policy-response"></a>
#### レスポンス

このAPIはレスポンスボディ(Body)を返却しません。

- - -
