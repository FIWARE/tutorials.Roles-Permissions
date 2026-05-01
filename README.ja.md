<h1 align="center">
    <img src="https://fiware.github.io/tutorials.Step-by-Step/img/fiware-farm.png" />
    <img src="https://img.shields.io/badge/NGSI-LD-d6604d.svg" width="90"/>
    <br/>
    👨‍🌾 👩‍🌾 🐄 🐐 🐑 🐖 🐓 🌻 🥕 🌽
</h1>

## ロールと権限

[![FIWARE Security](https://fiware.github.io/catalogue/badges/chapters/security.svg)](https://github.com/FIWARE/catalogue/blob/master/security/README.md)
[![License: MIT](https://img.shields.io/github/license/fiware/tutorials.Roles-Permissions.svg)](https://opensource.org/licenses/MIT)
[![Support badge](https://img.shields.io/badge/tag-fiware-orange.svg?logo=stackoverflow)](https://stackoverflow.com/questions/tagged/fiware)
<br/> [![Documentation](https://img.shields.io/readthedocs/ngsi-ld-tutorials.svg)](https://ngsi-ld-tutorials.rtfd.io)

このチュートリアルでは、Keycloak 内にクライアントアプリケーションを登録し、
Keycloak Authorization Services を使用してロールと権限を定義・割り当てる方法を説明します。
[前のチュートリアル](https://github.com/FIWARE/tutorials.Identity-Management/tree/NGSI-LD) で
作成したユーザとグループを使用して、正当なユーザのみが NGSI-LD リソースにアクセスできるように設定します。

このチュートリアルでは、**Keycloak** 管理コンソール GUI を使用したインタラクションの例と、
**Keycloak** Admin REST API へのアクセスに使用される [cUrl](https://ec.haxx.se/) コマンドを示します。

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new?repo=FIWARE/tutorials.Roles-Permissions&ref=NGSI-LD)

-   This tutorial is also available in [English](README.md).

# コンテンツ

<details>
<summary>詳細 <b>(クリックして拡大)</b></summary>

-   [認可とは](#認可とは)
    -   [Keycloak における認可の標準概念](#keycloak-における認可の標準概念)
-   [前提条件](#前提条件)
-   [アーキテクチャ](#アーキテクチャ)
-   [起動](#起動)
    -   [登場人物 (Dramatis Personae)](#登場人物-dramatis-personae)
    -   [REST API でのログイン](#rest-api-でのログイン)
-   [クライアントの管理](#クライアントの管理)
-   [レルムロールの管理](#レルムロールの管理)
-   [Authorization Services](#authorization-services)
    -   [スコープの作成](#スコープの作成)
    -   [リソースの作成](#リソースの作成)
    -   [ポリシーの作成](#ポリシーの作成)
    -   [権限の作成](#権限の作成)
    -   [権限の評価](#権限の評価)
-   [アプリケーションアクセスの認可](#アプリケーションアクセスの認可)
    -   [グループの認可](#グループの認可)
    -   [個別ユーザの認可](#個別ユーザの認可)

</details>

# 認可とは

> 「何をしていても、地球上のすべての人は世界の歴史において中心的な役割を果たしている。そして通常、本人はそれを知らない。」
>
> — パウロ・コエーリョ（「アルケミスト」より）

認可とは、認証されたユーザが特定のリソースに対して特定のアクションを実行する権限を持っているかどうかを
判断するプロセスです。ユーザが誰であるか (認証) を確認した後、システムはそのユーザが何を許可されているか
(認可) を判断する必要があります。

NGSI-LD によって支援される農場管理システムの文脈では、認可は以下のような質問を管理します:

-   畜産ワーカーはスプリンクラーにコマンドを送信できるか？
-   外部農学者は土壌センサーデータを読み取れるか？
-   トラクターオペレーターはトラクターエンティティの属性を更新できるか？

## Keycloak における認可の標準概念

| 概念 | 説明 |
|---|---|
| **レルムロール (Realm Role)** | レルムレベルで定義された名前付き権限バケット。ユーザまたはグループに割り当て可能 |
| **クライアント (Client)** | レルムから認証トークンをリクエストできる登録済みアプリケーション |
| **Authorization Services** | リソース、スコープ、ポリシー、権限によるきめ細かいアクセス制御を定義する Keycloak 機能 |
| **リソース (Resource)** | 保護された資産 — このチュートリアルでは各 NGSI-LD API エンドポイント |
| **スコープ (Scope)** | リソースに対して実行できるアクション — HTTP メソッドにマッピング (GET、POST、PATCH、DELETE) |
| **ポリシー (Policy)** | サブジェクト (ユーザ、グループ、ロール) がリソースにアクセスできるかどうかを評価するルール |
| **権限 (Permission)** | リソースとスコープをポリシーに紐付けたもの |

# 起動

```console
git clone https://github.com/FIWARE/tutorials.Roles-Permissions.git
cd tutorials.Roles-Permissions
git checkout NGSI-LD

./services create
./services start
```

## 登場人物 (Dramatis Personae)

| ユーザ | ロール | グループ |
|---|---|---|
| alice | システム管理者 | (なし — レルム管理者) |
| bob | `farm-manager` | `farm-management` |
| carol | `livestock-supervisor` | `livestock-team` |
| dave | `crop-supervisor` | `crop-team` |
| eve | `equipment-supervisor` | `equipment-team` |
| frank | `field-worker` | `livestock-team` |
| grace | `field-worker` | `livestock-team` |
| harry | `field-worker` | `crop-team` |
| ivy | `field-worker` | `equipment-team` |
| jenny | `read-only-consultant` | `external-consultants` |
| ken | `read-only-consultant` | `external-consultants` |

すべてのアカウントのパスワードは `test` です。

## REST API でのログイン

### 管理者トークンを取得

#### 1️⃣ Request:

```console
curl -iX POST \
  'http://localhost:3005/realms/master/protocol/openid-connect/token' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=password' \
  --data-urlencode 'client_id=admin-cli' \
  --data-urlencode 'username=admin' \
  --data-urlencode 'password=1234'
```

取得した `access_token` の値を `{{token}}` として保存してください。

# クライアントの管理

Keycloak では、**クライアント** はレルムから認証トークンをリクエストするアプリケーションの登録です。
NGSI-LD 農場管理プロキシ (`ngsi-ld-farm`) はレルムインポートで事前登録されています。

### クライアントを作成

#### 2️⃣ Request:

```console
curl -iX POST \
  'http://localhost:3005/admin/realms/farm-management/clients' \
  -H 'Authorization: Bearer {{token}}' \
  -H 'Content-Type: application/json' \
  -d '{
    "clientId": "ngsi-ld-farm",
    "name": "NGSI-LD Farm Management Application",
    "enabled": true,
    "publicClient": false,
    "secret": "1234",
    "serviceAccountsEnabled": true,
    "authorizationServicesEnabled": true,
    "redirectUris": ["http://localhost:3000/*"]
  }'
```

### クライアントの詳細を取得

#### 3️⃣ Request:

```console
curl -X GET \
  'http://localhost:3005/admin/realms/farm-management/clients?clientId=ngsi-ld-farm' \
  -H 'Authorization: Bearer {{token}}'
```

# レルムロールの管理

**レルムロール** はレルムレベルで定義された名前付き権限バケットです。
以下のロールがレルムインポートで事前作成されています:

| ロール | 説明 |
|---|---|
| `farm-manager` | すべての農場コンテキストデータとコマンドへのフルアクセス |
| `livestock-supervisor` | Animal、Water、FillingLevelSensor エンティティへの読み書きアクセス |
| `crop-supervisor` | Field、SoilSensor、WeatherObserved エンティティへの読み書きアクセス |
| `equipment-supervisor` | Tractor エンティティへの読み書きアクセス |
| `field-worker` | 計測値の書き込み、自ドメインエンティティの読み込み |
| `read-only-consultant` | すべてのエンティティタイプへの読み取り専用アクセス |

### ロールを作成

#### 7️⃣ Request:

```console
curl -iX POST \
  'http://localhost:3005/admin/realms/farm-management/roles' \
  -H 'Authorization: Bearer {{token}}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "drone-operator",
    "description": "作物畑上の自律型測量ドローンを操作する"
  }'
```

### ロールの詳細を取得

#### 8️⃣ Request:

```console
curl -X GET \
  'http://localhost:3005/admin/realms/farm-management/roles/farm-manager' \
  -H 'Authorization: Bearer {{token}}'
```

# Authorization Services

Keycloak の **Authorization Services** は、きめ細かいアクセス制御のための豊富なフレームワークを提供します。

## スコープの作成

**スコープ** は保護されたリソースに対して実行できるアクションを表します。

#### 1️⃣2️⃣ Request:

```console
curl -iX POST \
  'http://localhost:3005/admin/realms/farm-management/clients/{{client-uuid}}/authz/resource-server/scope' \
  -H 'Authorization: Bearer {{token}}' \
  -H 'Content-Type: application/json' \
  -d '{"name": "GET", "displayName": "HTTP GET"}'
```

`POST`、`PATCH`、`DELETE` についても同様に繰り返します。

## リソースの作成

**リソース** は保護されたエンドポイントを表します。

#### 1️⃣3️⃣ Request:

```console
curl -iX POST \
  'http://localhost:3005/admin/realms/farm-management/clients/{{client-uuid}}/authz/resource-server/resource' \
  -H 'Authorization: Bearer {{token}}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Entity Collection",
    "displayName": "NGSI-LD エンティティコレクションエンドポイント",
    "uris": ["/ngsi-ld/v1/entities", "/ngsi-ld/v1/entities/*"],
    "scopes": [{"name": "GET"}, {"name": "POST"}, {"name": "PATCH"}, {"name": "DELETE"}]
  }'
```

## ポリシーの作成

**ポリシー** はアクセスが許可される条件を定義します。

#### 1️⃣4️⃣ Request:

```console
curl -iX POST \
  'http://localhost:3005/admin/realms/farm-management/clients/{{client-uuid}}/authz/resource-server/policy/role' \
  -H 'Authorization: Bearer {{token}}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Farm Manager Policy",
    "roles": [{"id": "{{farm-manager-role-id}}", "required": false}]
  }'
```

## 権限の作成

**権限** はリソースとスコープをポリシーに紐付けます。

#### 1️⃣6️⃣ Request:

```console
curl -iX POST \
  'http://localhost:3005/admin/realms/farm-management/clients/{{client-uuid}}/authz/resource-server/permission/scope' \
  -H 'Authorization: Bearer {{token}}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Farm Managers can write all entities",
    "type": "scope",
    "decisionStrategy": "AFFIRMATIVE",
    "resources": ["{{entity-collection-resource-id}}"],
    "scopes": ["POST", "PATCH", "DELETE"],
    "policies": ["{{farm-manager-policy-id}}"]
  }'
```

## 権限の評価

#### 1️⃣7️⃣ Request:

```console
curl -X POST \
  'http://localhost:3005/realms/farm-management/protocol/openid-connect/token' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=urn:ietf:params:oauth:grant-type:uma-ticket' \
  --data-urlencode 'client_id=ngsi-ld-farm' \
  --data-urlencode 'client_secret=1234' \
  --data-urlencode 'audience=ngsi-ld-farm' \
  --data-urlencode 'permission=Entity Collection#POST'
```

`200` レスポンスに RPT が含まれていれば権限が付与されています。`403` で `access_denied` であれば拒否されています。

# アプリケーションアクセスの認可

## グループの認可

### グループにロールを割り当てる

#### 1️⃣8️⃣ Request:

```console
ROLE=$(curl -s 'http://localhost:3005/admin/realms/farm-management/roles/livestock-supervisor' \
  -H 'Authorization: Bearer {{token}}')

curl -iX POST \
  'http://localhost:3005/admin/realms/farm-management/groups/{{livestock-team-group-id}}/role-mappings/realm' \
  -H 'Authorization: Bearer {{token}}' \
  -H 'Content-Type: application/json' \
  -d "[${ROLE}]"
```

### グループのロール一覧を取得

#### 1️⃣9️⃣ Request:

```console
curl -X GET \
  'http://localhost:3005/admin/realms/farm-management/groups/{{group-id}}/role-mappings/realm' \
  -H 'Authorization: Bearer {{token}}'
```

## 個別ユーザの認可

### ユーザにロールを割り当てる

#### 2️⃣1️⃣ Request:

```console
ROLE=$(curl -s 'http://localhost:3005/admin/realms/farm-management/roles/farm-manager' \
  -H 'Authorization: Bearer {{token}}')

curl -iX POST \
  'http://localhost:3005/admin/realms/farm-management/users/{{user-id}}/role-mappings/realm' \
  -H 'Authorization: Bearer {{token}}' \
  -H 'Content-Type: application/json' \
  -d "[${ROLE}]"
```

### ユーザのロール一覧を取得

#### 2️⃣2️⃣ Request:

```console
curl -X GET \
  'http://localhost:3005/admin/realms/farm-management/users/{{user-id}}/role-mappings/realm' \
  -H 'Authorization: Bearer {{token}}'
```

# 次のステップ

他の [NGSI-LD チュートリアル](https://ngsi-ld-tutorials.rtfd.io) を参照して、
アプリケーションに高度な機能を追加する方法をご確認ください。
