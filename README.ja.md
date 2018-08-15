[![FIWARE Banner](https://fiware.github.io/tutorials.Roles-Permissions/img/fiware.png)](https://www.fiware.org/developers)

[![FIWARE Security](https://img.shields.io/badge/FIWARE-Security-ff7059.svg?logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABsAAAAVCAYAAAC33pUlAAAABHNCSVQICAgIfAhkiAAAA8NJREFUSEuVlUtIFlEUx+eO+j3Uz8wSLLJ3pBiBUljRu1WLCAKXbXpQEUFERSQF0aKVFAUVrSJalNXGgmphFEhQiZEIPQwKLbEUK7VvZrRvbr8zzjfNl4/swplz7rn/8z/33HtmRhn/MWzbXmloHVeG0a+VSmAXorXS+oehVD9+0zDN9mgk8n0sWtYnHo5tT9daH4BsM+THQC8naK02jCZ83/HlKaVSzBey1sm8BP9nnUpdjOfl/Qyzj5ust6cnO5FItJLoJqB6yJ4QuNcjVOohegpihshS4F6S7DTVVlNtFFxzNBa7kcaEwUGcbVnH8xOJD67WG9n1NILuKtOsQG9FngOc+lciic1iQ8uQGhJ1kVAKKXUs60RoQ5km93IfaREvuoFj7PZsy9rGXE9G/NhBsDOJ63Acp1J82eFU7OIVO1OxWGwpSU5hb0GqfMydMHYSdiMVnncNY5Vy3VbwRUEydvEaRxmAOSSqJMlJISTxS9YWTYLcg3B253xsPkc5lXk3XLlwrPLuDPKDqDIutzYaj3eweMkPeCCahO3+fEIF8SfLtg/5oI3Mh0ylKM4YRBaYzuBgPuRnBYD3mmhA1X5Aka8NKl4nNz7BaKTzSgsLCzWbvyo4eK9r15WwLKRAmmCXXDoA1kaG2F4jWFbgkxUnlcrB/xj5iHxFPiBN4JekY4nZ6ccOiQ87hgwhe+TOdogT1nfpgEDTvYAucIwHxBfNyhpGrR+F8x00WD33VCNTOr/Wd+9C51Ben7S0ZJUq3qZJ2OkZz+cL87ZfWuePlwRcHZjeUMxFwTrJZAJfSvyWZc1VgORTY8rBcubetdiOk+CO+jPOcCRTF+oZ0okUIyuQeSNL/lPrulg8flhmJHmE2gBpE9xrJNkwpN4rQIIyujGoELCQz8ggG38iGzjKkXufJ2Klun1iu65bnJub2yut3xbEK3UvsDEInCmvA6YjMeE1bCn8F9JBe1eAnS2JksmkIlEDfi8R46kkEkMWdqOv+AvS9rcp2bvk8OAESvgox7h4aWNMLd32jSMLvuwDAwORSE7Oe3ZRKrFwvYGrPOBJ2nZ20Op/mqKNzgraOTPt6Bnx5citUINIczX/jUw3xGL2+ia8KAvsvp0ePoL5hXkXO5YvQYSFAiqcJX8E/gyX8QUvv8eh9XUq3h7mE9tLJoNKqnhHXmCO+dtJ4ybSkH1jc9XRaHTMz1tATBe2UEkeAdKu/zWIkUbZxD+veLxEQhhUFmbnvOezsJrk+zmqMo6vIL2OXzPvQ8v7dgtpoQnkF/LP8Ruu9zXdJHg4igAAAABJRU5ErkJgggA=)](https://www.fiware.org/developers/catalogue/)
[![License: MIT](https://img.shields.io/github/license/fiware/tutorials.Roles-Permissions.svg)](https://opensource.org/licenses/MIT)
[![Documentation](https://img.shields.io/readthedocs/fiware-tutorials.svg)](https://fiware-tutorials.rtfd.io)

このチュートリアルでは、アプリケーションの作成方法と、ロールとパーミッションの割り当て方法について説明します。[以前のチュートリアル](https://github.com/Fiware/tutorials.Identity-Management)で作成したユーザと組織が必要であり、 正当なユーザだけがリソースにアクセスできるようにします。

このチュートリアルでは、**Keyrock** GUI を使用した対話の例や、**Keyrock** REST API へのアクセスに使用される [cUrl](https://ec.haxx.se/) コマンド、[Postman documentation](http://fiware.github.io/tutorials.Roles-Permissions/) も使用できます。

[![Run in Postman](https://run.pstmn.io/button.svg)](https://www.getpostman.com/collections/2febc0452a8977734480)

# 内容

- [認可とは何ですか？](#what-is-authorization)
  * [ID 管理の標準概念](#standard-concepts-of-identity-management)
  * [OAuth2](#oauth2)
- [前提条件](#prerequisites)
  * [Docker](#docker)
  * [Cygwin](#cygwin)
- [アーキテクチャ](#architecture)
  * [Keyrock の設定](#keyrock-configuration)
  * [MySQL の設定](#mysql-configuration)
- [起動](#start-up)
    + [登場人物 (Dramatis Personae)](#dramatis-personae)
    + [Keyrock MySQL データベースからの直接読み込み](#reading-directly-from-the-keyrock-mysql-database)
    + [Keyrock 内の UUIDs](#uuids-within-keyrock)
  * [REST API 呼び出しによるログイン](#logging-in-via-rest-api-calls)
    + [パスワードでトークンを作成](#create-token-with-password)
    + [トークン情報を取得](#get-token-info)
- [アプリケーションの管理](#managing-applications)
  * [:arrow_forward: ビデオ : Keyrock GUI を使用したアプリケーションの作成](#arrow_forward-video--creating-applications-with-the-keyrock-gui)
  * [アプリケーションの CRUD アクション](#application-crud-actions)
    + [アプリケーションを作成](#create-an-application)
    + [アプリケーションの詳細を取得](#read-application-details)
    + [すべてのアプリケーションのリストを取得](#list-all-applications)
    + [アプリケーションを更新](#update-an-application)
    + [アプリケーションを削除](#delete-an-application)
  * [パーミッションの CRUD アクション](#permission-crud-actions)
    + [パーミッションを作成](#create-a-permission)
    + [パーミッションの詳細を取得](#read-permission-details)
    + [パーミッションをリストを取得](#list-permissions)
    + [パーミッションを更新](#update-a-permission)
    + [パーミッションを削除](#delete-an-permission)
  * [ロールの CRUD アクション](#role-crud-actions)
    + [ロールを作成](#create-a-role)
    + [ロールの詳細を取得](#read-role-details)
    + [ロールのリストを取得](#list-roles)
    + [ロールを更新](#update-a-role)
    + [ロールを削除](#delete-a-role)
  * [各ロールへのパーミッションの割り当て](#assigning-permissions-to-each-role)
    + [ロールにパーミッションを追加](#add-a-permission-to-a-role)
    + [ロールのパーミッションのリストを取得](#list-permissions-of-a-role)
    + [ロールからパーミッションを削除](#remove-a-permission-from-a-role)
- [アプリケーションのアクセスを認可](#authorizing-application-access)
  * [組織を認可](#authorizing-organizations)
    + [組織にロールを付与](#grant-a-role-to-an-organization)
    + [付与された組織ロールのリストを取得](#list-granted-organization-roles)
    + [組織からロールを取り消す](#revoke-a-role-from-an-organization)
  * [個々のユーザ・アカウントを認可](#authorizing-individual-user-accounts)
    + [ユーザにロールを付与](#grant-a-role-to-a-user)
    + [付与されたユーザ・ロールのリストを取得](#list-granted-user-roles)
    + [ユーザからロールを取り消す](#revoke-a-role-from-a-user)
- [アプリケーション付与のリストを取得](#list-application-grantees)
    + [認可された組織のリストを取得](#list-authorized-organizations)
    + [認可されたユーザのリストを取得](#list-authorized-users)
- [次のステップ](#next-steps)


<a name="what-is-authorization"></a>
# 認可とは何ですか？

> "No matter what he does, every person on earth plays a central role in the history of the world.
> And normally he doesn't know it"
>
> — Paulo Coelho (The Alchemist)

認可は、情報セキュリティに関連するリソースへのアクセス権/特権を指定する機能です。より正式には、 "to authorize" (認可する) とはアクセス・ポリシーを定義することです。FIWARE **Keyrock** Generic Enabler を介して ID 管理を制御すると、ロールに割り当てられたパーミッションに基づいてユーザ・アクセスが許可されます。

**Keyrock** Generic Enabler によって保護されたすべてのアプリケーションは、一連のパーミッション、つまりアプリケーション内で実行可能な一連のパーミッションを定義できます。たとえば、アプリケーション内で、スマートドアのロックを解除するためのコマンドを送信する機能は、`Unlock Door` パーミッションを得て保護することができます。同様に、アラームを鳴らすためにコマンドを送る能力は `Ring Bell` パーミッションの背後に確保され、価格を変更する能力は `Price Change` パーミッションの背後に確保されます。

これらのパーミッションは、一連のロールにグループ化されています。たとえば `Unlock Door` と `Ring Bell` は、 両方ともセキュリティ・ロールに割り当てることができます。後でそのロールが付与されたユーザは両方のパーミッションを取得することを意味します。

パーミッションは重複して複数のロールに割り当てることができます。おそらく、`Ring Bell` は、`Price Change` と `Order Stock` と一緒に管理ロールにも割り当てられます。

次に、ユーザまたは組織は1つ以上のロールに割り当てられます。各ユーザは、それぞれのロールに対するすべてのパーミッションのすべてを取得します。たとえば、Alice は、両方の管理とセキュリティのロールに割り当てられている場合、彼女は、`Unlock Door`, `Ring Bell`, `Price Change`, `Order Stock` の 4つのすべてのパーミッションを持ちます。

ロールの概念はユーザには分かりません。パーミッションがアプリケーション内でどのように分割されるのではなく、付与 (grant) されているパーミッションの一覧のみがわかります。

要約すると、パーミッションはアプリケーション内のリソースに対して実行可能なすべてのアクションですが、ロールはそのアプリケーションのユーザのタイプによって実行できるアクションのグループです。

<a name="standard-concepts-of-identity-management"></a>
## ID 管理の標準概念

**Keyrock** ID 管理データベースには、次の共通オブジェクトがあります :

* **User** - 電子メールとパスワードを使用して自分自身を識別できる、登録済みのユーザ。ユーザには、個別にまたはグループとして権利を割り当てることができます
* **Application** -  一連のマイクロ・サービスで構成された任意のセキュアな FIWARE アプリケーション
* **Organization** - 一連の権利を割り当てることができるユーザのグループ。組織の権利を変更すると、その組織のすべてのユーザのアクセスが影響を受けます
* **OrganizationRole** - ユーザは組織のメンバまたは管理者になることができます。管理者は組織にユーザを追加または削除できます。メンバは組織のロールと権限を取得するだけです。これにより、各組織はメンバに対して責任を持つことができ、スーパー管理者 (super-admin) がすべての権限を管理する必要がなくなります
* **Role** - ロールは、一連のアクセス許可の説明的なバケットです。ロールは、単一のユーザまたは組織に割り当てることができます。サインインしたユーザは、自分のすべてのロールとその組織に関連付けられているすべてのロールのすべての権限を取得します
* **Permission** - システム内のリソース上で何かを行う能力

さらに、FIWARE アプリケーション内で、2つの人以外のアプリケーション (non-human application) のオブジェクトを保護することができます。

* **IoTAgent** - IoT センサとコンテキスト・ブローカー間のプロキシ
* **PEPProxy** - ユーザの権利を確認する Generic Enabler 間での使用のためのミドルウェア

オブジェクト間の関係は以下のようになります。赤でマークされたエンティティは、このチュートリアルで直接使用されています :

![](https://fiware.github.io/tutorials.Roles-Permissions/img/entities.png)

<a name="oauth2"></a>
## OAuth2

**Keyrock** は [OAuth2](https://oauth.net/2/) を使用して、サードパーティのアプリケーションがサービスへのアクセスを制限することを可能にします。**OAuth2** は、アクセス権限を与えるためのアクセス委任のためのオープン・スタンダードです。リソースのオーナーが自分の情報へのアクセスを第三者に許可することをリソース・プロバイダに通知することができます。例えば、あなたが、エンティティのリストへのアクセスを、Knowage アプリケーションに許可することを Knowage Generic Enabler に通知することができます。

いくつかの一般的な OAuth 2.0 グラント・フロー (grant flows) があります。その詳細は以下のとおりです :

* [Authorization Code](https://oauth.net/2/grant-types/authorization-code) (承認コード)
* [Implicit](https://oauth.net/2/grant-types/implicit) (暗黙)
* [Password](https://oauth.net/2/grant-types/password) (パスワード)
* [Client Credentials](https://oauth.net/2/grant-types/client-credentials) (クライアントの資格情報)
* [Device Code](https://oauth.net/2/grant-types/device-code) (デバイス・コード)
* [Refresh Token](https://oauth.net/2/grant-types/refresh-token) (トークンをリフレッシュ)

主なコンセプトは、**ユーザ**と**アプリケーション**の両方が、標準の OAuth2 チャレンジ・レスポンス・メカニズムを使用して、最初に自分自身を識別する必要があるということです。その後、ユーザは、その後のすべてのリクエストに追加するトークンを割り当てます。このトークンは、ユーザ、アプリケーション、およびユーザが実行できる権利を識別します。**Keyrock** を使用して、他の Enabler がアクセスを制限し、ロック・ダウンすることができます。アクセス・フローの詳細については、以下および後続のチュートリアルで説明します。

OAuth2 の背後にある理由は、ユーザに完全なアクセス権を与えるために、自分のユーザ名とパスワードを第三者に公開する必要がないことです。関連するアクセスを許可するだけです。読み取り専用または読み書きのいずれかになる関連アクセスを許可するだけで、このようなアクセスは細かいレベルまで定義できます。さらに、いつでもアクセスを取り消すための規定があり、リソースのオーナーは誰に何にアクセスできるかを制御できます。

<a name="prerequisites"></a>
# 前提条件

<a name="docker"></a>
## Docker

Docker

物事を単純にするために、両方のコンポーネントが [Docker](https://www.docker.com) を使用して実行されます。**Docker** は、さまざまコンポーネントをそれぞれの環境に分離することを可能にするコンテナ・テクノロジです。

* Docker Windows にインストールするには、[こちら](https://docs.docker.com/docker-for-windows/)の手順に従ってください
* Docker Mac にインストールするには、[こちら](https://docs.docker.com/docker-for-mac/)の手順に従ってください
* Docker Linux にインストールするには、[こちら](https://docs.docker.com/install/)の手順に従ってください

**Docker Compose** は、マルチコンテナ Docker アプリケーションを定義して実行するためのツールです。[YAML file](https://raw.githubusercontent.com/Fiware/tutorials.Identity-Management/master/docker-compose.yml) ファイルは、アプリケーションのために必要なサービスを構成するために使用します。つまり、すべてのコンテナ・サービスは1つのコマンドで呼び出すことができます。Docker Compose は、デフォルトで Docker for Windows とDocker for Mac の一部としてインストールされますが、Linux ユーザは[ここ](https://docs.docker.com/compose/install/)に記載されている手順に従う必要があります。

<a name="cygwin"></a>
## Cygwin

シンプルな bash スクリプトを使用してサービスを開始します。Windows ユーザは [cygwin](http://www.cygwin.com/) をダウンロードして、Windows 上の Linux ディストリビューションと同様のコマンドライン機能を提供する必要があります。

<a name="architecture"></a>
# アーキテクチャ

このイントロダクションでは、[Keyrock](http://fiware-idm.readthedocs.io/) Identity Management Generic Enabler という1つの FIWARE コンポーネントのみを使用します。**Keyrock** 単独での使用は、アプリケーションが *“Powered by FIWARE”* と認定するには不十分です。さらに、**MySQL** データベースにユーザ・データを保存する予定です。

全体的なアーキテクチャは、次の要素で構成されます :

* 1つの **FIWARE Generic Enabler** :

    * FIWARE [Keyrock](http://fiware-idm.readthedocs.io/) は補完的な ID 管理システムを提供します :
        * アプリケーションとユーザのための OAuth 2 認証システム
        * ID 管理のアドミニストレーションのための Web サイトのグラフィカル・フロントエンド
        * HTTP リクエストによる ID 管理用の同等の REST API

* 1つの [MySQL](https://www.mysql.com/) データベース :
    * ユーザ ID、アプリケーション、ロール、およびパーミッションを保持するために使用します

要素間のすべてのインタラクションは HTTP リクエストによって開始されるため、エンティティはコンテナ化され、公開されたポートから実行されます。


![](https://fiware.github.io/tutorials.Roles-Permissions/img/architecture.png)

チュートリアルの各セクションの具体的なアーキテクチャについては、以下で説明します。

<a name="keyrock-configuration"></a>
## Keyrock の設定

```yaml
  idm:
    image: fiware-idm
    container_name: idm
    hostname: idm
    depends_on:
      - mysql-db
    ports:
      - "3005:3005"
    networks:
      default:
    environment:
      - DATABASE_HOST=mysql-db
      - IDM_DB_PASS_FILE=/run/secrets/my_secret_data
      - IDM_DB_USER=root
      - IDM_HOST=http://localhost:3005
      - IDM_PORT=3005
    secrets:
      - my_secret_data
```

`idm` コンテナは、単一のポートでリッスンしている、Web アプリケーション・サーバです :

* Port `3005` は HTTP トラフィックで公開されているため、Web ページを表示して REST API とやりとりすることができます。

> **注** HTTP プロトコルはデモ目的でのみ使用しています。実稼働環境では、プレーン・テキストを使用して機密情報を送信しないように、すべての OAuth2 認証が HTTPS 経由で行われる必要があります。

`idm` コンテナは、次に示す環境変数によってドライブされます :

| キー| 値  |   説明    |
|-----|-----|-----------|
|IDM_DB_PASS|`idm`| 接続する MySQL データベースのパスワード。**Docker Secrets** によって保護されています。下記を参照してください |
|IDM_DB_USER|`root`|デフォルトの MySQL ユーザのユーザ名。プレーン・テキストです |
|IDM_HOST|`http://localhost:3005`| **Keyrock** アプリケーション・サーバのホスト名。ユーザ登録時にアクティベーション e-mail で使用されます|
|IDM_PORT|`3005`| **Keyrock** アプリケーション・サーバで使用されるポート。これは、衝突を避けるためにデフォルトの `3000` ポートから変更しています |

> :information_source: **注** この例では、**Docker Secrets** を使用して MySQL パスワードを保護していることに注意してください。`_FILE` サフィックスを持つ `IDM_DB_PASS` を使用し、シークレット・ファイルの場所を参照します。これによりプレーン・テキストの `ENV` 変数として、パスワードを公開することを避けることができます。`Dockerfile` イメージか `docker inspect` を使って読むことができる注入変数 (an injected variable ) です。

> 次の変数のリストは、プロダクション・システムで `_FILE` サフィックスを持つシークレットを使用して設定する必要があります :
>
> * `IDM_SESSION_SECRET`
> * `IDM_ENCRYPTION_KEY`
> * `IDM_DB_PASS`
> * `IDM_DB_USER`
> * `IDM_ADMIN_ID`
> * `IDM_ADMIN_USER`
> * `IDM_ADMIN_EMAIL`
> * `IDM_ADMIN_PASS`
> * `IDM_EX_AUTH_DB_USER`
> * `IDM_EX_AUTH_DB_PASS`

<a name="mysql-configuration"></a>
## MySQL の設定

```yaml
  mysql-db:
    image: mysql:5.7
    hostname: mysql-db
    container_name: db-mysql
    expose:
      - "3306"
    ports:
      - "3306:3306"
    networks:
      default:
    environment:
      - "MYSQL_ROOT_PASSWORD_FILE=/run/secrets/my_secret_data"
      - "MYSQL_ROOT_HOST=172.18.1.5"
    volumes:
      - mysql-db:/var/lib/mysql
    secrets:
      - my_secret_data
```

MySQLの設定

`mysql-db` コンテナは、単一ポートで待機しています :

* Port `3306` は MySQL サーバのデフォルト・ポートです。これは公開されているので、必要に応じて他のデータベース・ツールを実行してデータを表示することもできます

`mysql-db` コンテナは、次に示すような環境変数によってドライブされます :

| キー              | 値       | 説明                                     |
|-------------------|----------|------------------------------------------|
|MYSQL_ROOT_PASSWORD|`123`     | **Docker Secrets** によって保護されている MySQL の `root` アカウントに設定されているパスワードを指定します。下記を参照してください |
|MYSQL_ROOT_HOST    |`root`| デフォルトでは、MySQL は `root'@'localhost` アカウントを作成します。このアカウントはコンテナ内からのみ接続できます。この環境変数を設定すると、他のホストからのルート接続が可能になります。 |

<a name="start-up"></a>
# 起動

インストールを開始するには、次の手順を実行します :

```console
git clone git@github.com:Fiware/tutorials.Roles-Permissions.git
cd tutorials.Roles-Permissions

./services create
```

>**注** Docker イメージの最初の作成には最大3分かかります

その後、リポジトリ内で提供される [services](https://github.com/Fiware/tutorials.Identity-Management/blob/master/services) Bash スクリプトを実行することによって、コマンドラインからすべてのサービスを初期化することができます :

```console
./services <command>
```

ここで、`<command>` は、私たちがアクティベートしたいエクササイズに応じてかわります。

>:information_source: **注:** クリーンアップをやり直したい場合は、次のコマンドを使用して再起動することができます :
>
>```console
>./services stop
>```
>

<a name="dramatis-personae"></a>
### 登場人物 (Dramatis Personae)

次の `test.com` のメンバは、アプリケーション内に正当なアカウントを持っています。

* Alice, 彼女は **Keyrock** アプリケーションの管理者になります
* Bod, スーパー・マーケット・チェーンの地域マネージャ。彼の下に数人のマネージャがいます :
  * Manager1
  * Manager2
* Charlie, スーパー・マーケットチェーン・のセキュリティ責任者。彼の下に数人の警備員がいます。
  * Detective1
  * Detective2

次の `example.com` のメンバーはアカウントに登録しましたが、アクセスを許可する理由はありません。

* Eve - 盗聴者のイブ
* Mallory - 悪意のある攻撃者のマロリー
* Rob - 強盗のロブ

| 名前       |eMail                       |Password | UUID                                  |
|------------|----------------------------|---------|---------------------------------------|
| alice      | alice-the-admin@test.com   | `test`  |`aaaaaaaa-good-0000-0000-000000000000` |
| bob        | bob-the-manager@test.com   | `test`  |`bbbbbbbb-good-0000-0000-000000000000` |
| charlie    | charlie-security@test.com  | `test`  |`cccccccc-good-0000-0000-000000000000` |
| manager1   | manager1@test.com          | `test`  |`manager1-good-0000-0000-000000000000` |
| manager2   | manager2@test.com          | `test`  |`manager2-good-0000-0000-000000000000` |
| detective1 | detective1@test.com        | `test`  |`secure01-good-0000-0000-000000000000` |
| detective2 | detective2@test.com        | `test`  |`secure02-good-0000-0000-000000000000` |
| eve        | eve@example.com            | `test`  |`eeeeeeee-evil-0000-0000-000000000000` |
| mallory    | mallory@example.com        | `test`  |`mmmmmmmm-evil-0000-0000-000000000000` |
| rob        | rob@example.com            | `test`  |`rrrrrrrr-evil-0000-0000-000000000000` |

2つの組織が Alice によって設定されました :

| 名前       | 説明                                   | UUID                                 |
|------------|----------------------------------------|--------------------------------------|
| Security   | 店員のためのセキュリティ・グループ     |`security-0000-0000-0000-000000000000`|
| Management | ストア・マネージャのための管理グループ |`managers-0000-0000-0000-000000000000`|

時間を節約するために、[以前のチュートリアル](https://github.com/Fiware/tutorials.Identity-Management)からユーザと組織を作成するデータがダウンロードされ、起動時に自動的に MySQL データベースに保存されるため、UUIDs が変更されず、データを再入力する必要もありません。

ユーザと組織を作成する方法についてのあなたの記憶をリフレッシュするには、アカウント `alice-the-admin@test.com` を使用して、パスワード `test` で `http://localhost:3005/idm ` にログインできます。

![](https://fiware.github.io/tutorials.Roles-Permissions/img/log-in.png)

組織のリストを見てください。

<a name="reading-directly-from-the-keyrock-mysql-database"></a>
### Keyrock MySQL データベースからの直接読み込み

すべての ID 管理のレコードと関連性は、MySQL データベース内に保持されます。以下のように実行中の Docker コンテナを入力するとアクセスできます :

```console
docker exec -it db-mysql bash
```

```console
mysql -u <user> -p<password> idm
```

ここで、`<user>` と `<password>` は、`docker-compose` ファイル内で定義された値の  `MYSQL_ROOT_PASSWORD` と `MYSQL_ROOT_USER` に一致します。チュートリアルのデフォルト値は、通常 `root`、および `secret` です。

コマンドラインから SQL コマンドを入力することができます。例えば :

```SQL
select id, username, email, password from user;
```

<a name="uuids-within-keyrock"></a>
### Keyrock 内の UUIDs

**Keyrock** 内のすべての IDs とトークンは変更される可能性があります。レコードをクエリするときは、以下の値を修正する必要があります。レコード IDs は Universally Unique Identifiers - UUIDs を使用します。

| キー| 説明                              | サンプル値   |
|-----|-----------------------------------|--------------|
|`keyrock`| **Keyrock** サービスの場所の URL |`localhost:3005`|
|`X-Auth-token`| ユーザとしてログインするときにヘッダで受け取ったトークン |`aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa` = I am Alice|
|`X-Subject-token`| サブジェクトについて問い合わせるときに渡すトークンで、代替はユーザ・トークンを繰り返ためのものです |`bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb` = Asking about Bob|
|`user-id`| `user` テーブルで見つかった既存ユーザの id |`bbbbbbbb-good-0000-0000-000000000000` - Bob's User Id|
|`application-id`| `oauth_client` テーブルで見つかった、既存アプリケーションの id |`c978218d-ad63-4427-b12b-542b81299cfb`|
|`role-id`| `role` テーブルで見つかった、既存ロールの id |`d28baa00-839e-4b45-a6b2-1cec563942ee`|
|`permission-id`| `permission` テーブルで見つかった、既存パーミッションの id |`6b6cd19c-9398-4834-9ba1-1616c57139c0`|
|`organization-id`| `organization` テーブルで見つかった、既存組織の id |`e424ed98-c966-46e3-b161-a165fd31bc01`|
|`organization-role-id`| `owner` または `member` のいずれかの組織内でユーザが持つロールのタイプ | `member` |
|`iot-agent-id`| `iot` テーブルで見つかった、既存 IoT Agent の id  |`iot_sensor_f3d0245b-3330-4e64-a513-81bf4b0dae64`|
|`pep-proxy-id`| `pep_proxy` テーブルで見つかった、既存 PEP Proxy の id  |`iot_sensor_f3d0245b-3330-4e64-a513-81bf4b0dae64`|

トークンは、一定期間後に期限切れになるように設計されています。使用している `X-Auth-token` 値の有効期限が切れている場合は、再度ログインして新しいトークンを取得してください。このチュートリアルでは、ユーザごとに長く持続する一連のトークンが作成され、データベースに保存されるため、通常はトークンを更新する必要はありません。

<a name="logging-in-via-rest-api-calls"></a>
## REST API 呼び出しによるログイン

アプリケーションに入るには、ユーザ名とパスワードを入力します。デフォルトのスーパー・ユーザ (super-user) は、`alice-the-admin@test.com` と `test` の値を持っています。

<a name="create-token-with-password"></a>
### パスワードでトークンを作成

次の例では、Admin Super-User を使用してログインします :

#### :one: リクエスト :
```console
curl -iX POST \
  'http://localhost:3005/v1/auth/tokens' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "alice-the-admin@test.com",
  "password": "test"
}'
```

#### レスポンス :

```
HTTP/1.1 201 Created
X-Subject-Token: d848eb12-889f-433b-9811-6a4fbf0b86ca
Content-Type: application/json; charset=utf-8
Content-Length: 138
ETag: W/"8a-TVwlWNKBsa7cskJw55uE/wZl6L8"
Date: Mon, 30 Jul 2018 12:07:54 GMT
Connection: keep-alive
```
```json
{
    "token": {
        "methods": [
            "password"
        ],
        "expires_at": "2018-07-30T13:02:37.116Z"
    },
    "idm_authorization_config": {
        "level": "basic",
        "authzforce": false
    }
}
```

<a name="get-token-info"></a>
### トークン情報を取得


このチュートリアルでは、長く持続する `X-Auth-token=aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa` を使用して、Alice のふりをすることができます。

時間制限されたトークンがあれば、ユーザに関する詳細情報を見つけることができます。Bob に関する情報を検索するには、長く持続するトークン `X-Subject-token=bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb` を使用します。

このリクエストは、*トークン `{{X-Auth-token}}` で認可されたユーザ (Alice) は、トークン `{{X-Subject-token}}` を保持しているユーザ (Bob) について問い合わせていること*を示します。

#### :two: リクエスト :

```console
curl -iX GET \
  'http://localhost:3005/v1/auth/tokens' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa' \
  -H 'X-Subject-token: bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb'
```

#### レスポンス :

レスポンスは、関連するユーザの詳細を返します。Bob は 2026年まで持続するトークンを保持していることがわかります。

```json
{
    "access_token":"bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
    "expires":"2026-07-30T12:38:13.000Z",
    "valid":true,
    "User":{
        "id":"bbbbbbbb-good-0000-0000-000000000000",
        "username":"bob",
        "email":"bob-the-manager@test.com",
        "date_password":"2018-07-30T11:41:14.000Z",
        "enabled":true,
        "admin":false
    }
```


<a name="managing-applications"></a>
# アプリケーションの管理

どの FIWARE アプリケーションも、マイクロサービスの集合体に分解することができます。これらのマイクロサービスは、実際の世界の状態を読み取り、変更するために一緒に接続します。これらのサービスに対するセキュリティは、これらのリソースに対するアクションを適切なパーミッションを持つユーザに制限することによって追加できます。したがって、許可されたアクションのセットを提供し、許可されたユーザ、またはユーザのグループ、すなわち組織のリストを保持するために、アプリケーションを定義する必要があります。

したがって、アプリケーションは、どのリソース上で何ができるのかを把握する概念的なバケツ (a conceptual bucket) です。

<a name="arrow_forward-video--creating-applications-with-the-keyrock-gui"></a>
## :arrow_forward: ビデオ : Keyrock GUI を使用したアプリケーションの作成

![](http://img.youtube.com/vi/pjsl0eHpFww/0.jpg)

リンクをクリックすると、[**Keyrock GUI** を使用してアプリケーションを作成する方法](https://www.youtube.com/watch?v=pjsl0eHpFww&t=470 " Creating Applications")を示すビデオを見ることができます。

<a name="application-crud-actions"></a>
## アプリケーションの CRUD アクション

標準の CRUD アクションは、`/v1/applications` エンドポイントの下の適切な HTTP 動詞 (POST, GET, PATCH および DELETE) に割り当てられます。

<a name="create-an-application"></a>
### アプリケーションを作成

ログインすると、ユーザにはホーム・スクリーンが提示されます。

![](https://fiware.github.io/tutorials.Roles-Permissions/img/apps-and-orgs.png)

GUI のホームページから、**Register** ボタンをクリックすると新しいアプリケーションを作成できます。

![](https://fiware.github.io/tutorials.Roles-Permissions/img/create-app.png)


REST API を使用して新しいアプリケーションを作成するには、保護する Web サービスの `url` などの OAuth 情報フィールド、および `redirect_uri` などと`name` と `description` などのアプリケーションの詳細を含む POST リクエストを `/v1/applications` エンドポイントに送信します。ここでユーザはその資格情報の正当性をチェックされます。`grant_types` は、OAuth2 グラント・フローの利用可能なリストから選択されます。ヘッダには、以前にログインしたユーザの `X-Auth-token` が含まれており、アプリケーション上でプロバイダのロールが自動的に付与されます。

#### :three: リクエスト :

以下の例では、`X-Auth-token=aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa` を持つ Alice は、3つの異なるグラント・タイプ を受け入れる新しいアプリケーションを作成しています。

```console
curl -iX POST \
  'http://localhost:3005/v1/applications' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa' \
  -d '{
  "application": {
    "name": "Tutorial Application",
    "description": "FIWARE Application protected by OAuth2 and Keyrock",
    "redirect_uri": "http://context-provider/login",
    "url": "http://context-provider",
    "grant_type": [
      "authorization_code",
      "implicit",
      "password"
    ]
  }
}'
```

#### レスポンス :

レスポンスには、アプリケーションを保護するために使用できるクライアント Id と Secret が含まれています。

```json
{
    "application": {
        "id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482",
        "secret": "aa2d0845-0a8e-4ae8-addf-3c87bcab19e1",
        "image": "default",
        "name": "Tutorial Application",
        "description": "FIWARE Application protected by OAuth2 and Keyrock",
        "redirect_uri": "http://context-provider/login",
        "url": "http://context-provider",
        "grant_type": "password,authorization_code,implicit",
        "response_type": "code,token"
    }
}
```

他のすべてのアプリケーションのリクエストに使用するアプリケーションの クライアント id をコピーします。上記の場合、id は `3782c5e3-88f9-481a-9b3c-2f2d6f604482` です。

<a name="read-application-details"></a>
### アプリケーションの詳細を取得

`/v1/applications/{{application-id}}` エンドポイントの下のリソースに GET リクエストを行うと、その id の下にリストされているアプリケーションが返されます。`X-Auth-token` がヘッダに必要です。

![](https://fiware.github.io/tutorials.Roles-Permissions/img/app-with-oauth.png)

#### :four: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

```json
{
    "application": {
        "id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482",
        "name": "Tutorial Application",
        "description": "FIWARE Application protected by OAuth2 and Keyrock",
        "secret": "aa2d0845-0a8e-4ae8-addf-3c87bcab19e1",
        "url": "http://context-provider",
        "redirect_uri": "http://context-provider/login",
        "image": "default",
        "grant_type": "password,authorization_code,implicit",
        "response_type": "code,token",
        "client_type": null,
        "scope": null,
        "extra": null
    }
}
```

<a name="list-all-applications"></a>
### すべてのアプリケーションのリストを取得

ユーザは、関連付けられているアプリケーションのみを返すことが許可されます。アプリケーションをリストアップするには、`/v1/applications` エンドポイントへ `X-Auth-token` ヘッダを付けて、`GET` リクエストを行います。

#### :five: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

```json
{
    "applications": [
        {
            "id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482",
            "name": "Tutorial Application",
            "description": "FIWARE Application protected by OAuth2 and Keyrock",
            "image": "default",
            "url": "http://context-provider",
            "redirect_uri": "http://context-provider/login",
            "grant_type": "password,authorization_code,implicit",
            "response_type": "code,token",
            "client_type": null
        }
    ]
}
```

<a name="update-an-application"></a>
### アプリケーションを更新

GUI 内では、アプリケーションを選択して、`edit` をクリックすることでユーザを更新できます。これは、アプリケーション id がわかっているときに `/v1/applications/{{applications-id}}` エンドポイントに PATCH リクエストを出すことによって、コマンド・ラインから行うこともできます。ユーザは自身に関連付けられているアプリケーションを編集することができますので、`X-Auth-token` ヘッダも設定する必要があります。

#### :six: リクエスト :

```console
curl -X PATCH \
  'http://localhost:3005/v1/applications/{{application-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}' \
  -d '{
  "application": {
    "name": "Tutorial New Name",
    "description": "This is a new description",
    "redirect_uri": "http://context-provider/login",
    "grant_type": [
      "authorization_code",
      "password"
    ]
  }
}'
```

#### レスポンス :

レスポンスには更新されたフィールドがリストされていますが、上で定義した `redirect_uri` が既に設定されていることに注意してください :

```json
{
    "values_updated": {
        "name": "Tutorial New Name",
        "description": "This is a new description",
        "grant_type": "password,authorization_code",
        "response_type": "code"
    }
}
```

<a name="delete-an-application"></a>
### アプリケーションを削除

GUI 内で、ユーザはアプリケーションを選択して、`edit` をクリックしてからページの一番下までスクロールし、**Remove Application** を選択することで、アプリケーションを削除できます。これは、コマンド・ラインから、`/v1/applications/<applications-id>`  エンドポイントに DELETE リクエストを送信することによっても実行できます。`X-Auth-token` ヘッダが、設定されなければなりません。

#### :seven: リクエスト :

```console
curl -iX DELETE \
  'http://localhost:3005/v1/applications/{{applications-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

<a name="permission-crud-actions"></a>
## パーミッションの CRUD アクション

アプリケーションのパーミッションは、そのアプリケーション内のリソースに対して許可されるアクションです。各リソースは URL (たとえば /price-change) で定義され、アクションは任意の HTTP 動詞 (たとえば GET) です。

- この組み合わせは、許可されたユーザのみが `/price-change` リソースにアクセスできるようにするために使用されます

XACML を使用してさらに高度なパーミッション・ルールを記述することができます。これは別のチュートリアルの主題です。

パーミッションは常にアプリケーションにバインドされていることが強調されています。抽象的なパーミッションはそれ自身では存在しません。標準パーミッションの CRUD アクションは、`/v1/applications/{{application-id}}/permissions` エンドポイントの下の適切な HTTP 動詞 (POST, GET, PATCH および DELETE) に割り当てられます。

- ご覧のように、`<application-id>` 自体は URL に不可欠です

パーミッションは、通常、一度定義され、アプリケーションの作成時に設定されます。ユース・ケースの設計が、パーミッションを定期的に変更する必要があると判明した場合、その定義は間違った層で定義されている可能性があります。複雑なアクセス制御規則を XACML 定義に落とし込むか、アプリケーションのビジネス・ロジックに移動するべきです。 それらは、**Keyrock** 内で扱うべきではありません。

<a name="create-a-permission"></a>
### パーミッションを作成

GUI 内では、アプリケーションを選択し、**Manage Roles** をクリックし て、パーミッションのラベルの横にあるプラス記号を押すことで、アプリケーションにパーミッションを追加できます。

![](https://fiware.github.io/tutorials.Roles-Permissions/img/create-permission.png)

ウィザードに記入し、**Save** をクリックします。

REST API 経由で新しいパーミッションを作成するには、以前にログインしたユーザの `X-Auth-token` ヘッダとともに、アクションとリソースを含む `/applications/{{application-id}}/permissions` エンドポイントに POST リクエストを送信します。

#### :eight: リクエスト :

```console
curl -iX POST \
  'http://localhost:3005/v1/applications/{{application-id}}/permissions' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}' \
  -d '{
  "permission": {
    "name": "Access Price Changes",
    "action": "GET",
    "resource": "/price-change"
  }
}'
```

#### レスポンス :

レスポンスは、新しく作成されたパーミッションの詳細を返します。

```json
{
    "permission": {
        "id": "c8ace792-d058-4650-9958-59753215e1cc",
        "is_internal": false,
        "name": "Access Price Changes",
        "action": "GET",
        "resource": "/price-change",
        "oauth_client_id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482"
    }
}
```

<a name="read-permission-details"></a>
### パーミッションの詳細を取得

`/applications/{{application-id}}/permissions/{{permission-id}}` エンドポイントは、その id の下にリストされているパーミッションを返します。`X-Auth-token` をヘッダに指定しなければなりません。

#### :nine: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/permissions/{{permission-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

レスポンスは、リクエストされたパーミッションの詳細を返します。

```json
{
    "permission": {
        "id": "c21983d5-58f9-4bcc-b2b0-f21819080ad0",
        "name": "Enable Alarm Bell",
        "description": null,
        "is_internal": false,
        "action": "POST",
        "resource": "/ring",
        "xml": null,
        "oauth_client_id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482"
    }
}
```

<a name="list-permissions"></a>
### パーミッションをリストを取得

`/v1/applications/{{application-id}}/permissions` エンドポイントへの GET リクエストを行うことで、アプリケーションのパーミッションのリストを取得することができます。

#### :one::zero: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/permissions' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

パーミッションの完全なリストには、以前に作成されたカスタム・パーミッションとデフォルトで使用可能なすべての標準パーミッションが含まれます。

```json
{
    "permissions": [
        {
            "id": "c8ace792-d058-4650-9958-59753215e1cc",
            "name": "Access Price Changes",
            "description": null,
            "action": "GET",
            "resource": "/price-change",
            "xml": null
        },
        {
            "id": "c21983d5-58f9-4bcc-b2b0-f21819080ad0",
            "name": "Enable Alarm Bell",
            "description": null,
            "action": "POST",
            "resource": "/ring",
            "xml": null
        },
        ...etc
        {
            "id": "2",
            "name": "Manage the application",
            "description": null,
            "action": null,
            "resource": null,
            "xml": null
        },
        {
            "id": "1",
            "name": "Get and assign all internal application roles",
            "description": null,
            "action": null,
            "resource": null,
            "xml": null
        }
    ]
}
```

<a name="update-a-permission"></a>
### パーミッションを更新

既存のパーミッションの詳細を修正するには、PATCH リクエストを `/applications/{{application-id}}/permissions/{permission-id}}` エンドポイントに送信します。

#### :one::one: リクエスト :

```console
curl -X PATCH \
  'http://localhost:3005/v1/applications/{{application-id}}/permissions/{{permission-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}' \
  -d '{
  "permission": {
    "name": "Ring Alarm Bell",
    "action": "POST",
    "resource": "/ring"
  }
}'
```

#### レスポンス :

レスポンスには、修正されたフィールドのリストが含まれています。

```json
{
    "values_updated": {
        "name": "Ring Alarm Bell"
    }
}
```

<a name="delete-an-permission"></a>
### パーミッションを削除

アプリケーションからパーミッションを削除すると、関連するロールからそのパーミッションが自動的に削除されます。

#### :one::two: リクエスト :

```console
curl -X DELETE \
  'http://keyrock/v1/applications/{{application_id}}/permissions/{{permission_id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

<a name="role-crud-actions"></a>
## ロールの CRUD アクション

上記のように、パーミッションはリソースに対する許容されるアクションです。ロールは、パーミッションのグループ、言い換えればリソースのグループに対する一連の許可されたアクションで構成されます。ロールは通常、幅広い範囲の説明が与えられているため、幅広いユーザまたは組織に割り当てることができます。たとえば、*Reader* ロールは一連のデバイスにアクセスできますが、更新できません。

*Keyrock* には、あらかじめ定義された2つのロールがあります。

* *Purchaser*
   + すべての公開アプリケーションのロールを取得し割り当てることができます
* *Provider*
   + 公開の所有ロールのみを取得して割り当てることができます
   + すべての公開アプリケーションのロールを取得して割り当てることができます
   + 認可を管理することができます
   + ロールを管理することができます
   + アプリケーションを管理することができます
   + すべての内部アプリケーションのロールを取得して割り当てることができます

私たちのスーパー・マーケット・ストアの例を使用すると、管理者 Alice には、Provider のロールが割り当てられ、*管理やセキュリティのような*、必要とされるアプリケーション固有の追加のロールを作成できます。

再び、ロールは常にアプリケーションに直接バインドされます。抽象的なロールはそれ自身では存在しません。標準の CRUD アクションは、`/v1/applications/{{application-id}}/roles` エンドポイントの下の適切な HTTP 動詞 (POST, GET, PATCH および DELETE) に割り当てられます。

<a name="create-a-role"></a>
### ロールを作成

GUI 内で、アプリケーションを選択し、**Manage Roles** をクリックし て、Role ラベルの隣にあるプラス記号を押すことで、ロールをアプリケーションに追加できます。

![](https://fiware.github.io/tutorials.Roles-Permissions/img/create-role.png)

ウィザードに記入し、**Save** をクリックします。

REST API を介して新しいロールを作成するには、以前にログインしたユーザの `X-Auth-token` ヘッダと新しいロールの `name` を含む POST リクエストを `/applications/{{application-id}}/roles` エンドポイントに送信します。

#### :one::three: リクエスト :

```console
curl -X POST \
  'http://localhost:3005/v1/applications/{{application-id}}/roles' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}' \
  -d '{
  "role": {
    "name": "Management"
  }
}'
```

#### レスポンス :

作成されたロールの詳細が返されます。

```json
{
    "role": {
        "id": "bc64fe78-f440-4ce0-815d-78b1d3d8b9a1",
        "is_internal": false,
        "name": "Management",
        "oauth_client_id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482"
    }
}
```

<a name="read-role-details"></a>
### ロールの詳細を取得

`/applications/{{application-id}}/roles/{{role-id}}` エンドポイントは、その id の下にリストされているロールを返します。`X-Auth-token` をヘッダに指定しなければなりません。

#### :one::four: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/roles/{{role-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

レスポンスは、リクエストされたロールの詳細を返します。

```json
{
    "role": {
        "id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c",
        "name": "Security",
        "is_internal": false,
        "oauth_client_id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482"
    }
}
```

<a name="list-roles"></a>
### ロールのリストを取得

アプリケーションが提供するすべてのロールをリストするには、`/v1/applications/{{application-id}}/roles` エンドポイントに GET リクエストを行います。

#### :one::five: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/roles' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

標準 ロールとカスタム・ロールの両方を含むアプリケーションに関連付けられたすべてのロールの概要が返されます。

```json
{
    "roles": [
        {
            "id": "purchaser",
            "name": "Purchaser"
        },
        {
            "id": "provider",
            "name": "Provider"
        },
        {
            "id": "bc64fe78-f440-4ce0-815d-78b1d3d8b9a1",
            "name": "Management"
        },
        {
            "id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c",
            "name": "Security"
        }
    ]
}
```

<a name="update-a-role"></a>
### ロールを更新

`/applications/{{application-id}}/roles/{{role-id}}` エンドポイントに PATCH リクエストを送信することで、ロールの名前を修正することができます。

#### :one::six: リクエスト :

```console
curl -iX PATCH \
  'http://localhost:3005/v1/applications/{{application-id}}/roles/{{role-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}' \
  -d '{
  "role": {
    "name": "Security Team"
  }
}'
```

#### レスポンス :

レスポンスには、修正されたフィールドのリストが含まれています。

```json
{
    "values_updated": {
        "name": "Security Team"
    }
}
```

<a name="delete-a-role"></a>
### ロールを削除

アプリケーションのロールも削除することができます。これにより、どのユーザからもロールが削除されます。

#### :one::seven: リクエスト :

```console
curl -iX DELETE \
  'http://localhost:3005/v1/applications/{{application-id}}/roles/{{role-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

<a name="assigning-permissions-to-each-role"></a>
## 各ロールへのパーミッションの割り当て

一連のアプリケーションのパーミッションと一連のアプリケーションのロールを作成したら、次にロールに関連するパーミッションを割り当てます。つまり、*Who can do What* (誰が何をすることができる) を定義します。

<a name="add-a-permission-to-a-role"></a>
### ロールにパーミッションを追加

GUI 内で、ロールを選択し、保存する前にリストからパーミッションを確認します。

![](https://fiware.github.io/tutorials.Roles-Permissions/img/add-permission-to-role.png)

REST API を使用してアクセス許可を追加するために、次に示されるように、URL パスに `<application-id>`, `<role-id>`, `<permission-id>` を含め、ヘッダに `X-Auth-Token` を使用して自身を識別する、POST リクエストを行います。

#### :one::eight: リクエスト :

```console
curl -iX POST \
  'http://localhost:3005/v1/applications/{{application-id}}/roles/{{role-id}}/permissions/{{permission-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

レスポンスはロールのパーミッションを返します。

```json
{
    "role_permission_assignments": {
        "role_id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c",
        "permission_id": "c21983d5-58f9-4bcc-b2b0-f21819080ad0"
    }
}
```

<a name="list-permissions-of-a-role"></a>
### ロールのパーミッションのリストを取得

アプリケーションのロールに割り当てられたすべてのパーミッションの完全なリストは、`/v1/applications/{{application-id}}/roles/{{role-id}}/permissions` エンドポイントに GET リクエストを行うことで取得できます。

#### :one::nine: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/roles/{{role-id}}/permissions' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

```json
{
    "role_permission_assignments": [
        {
            "id": "c21983d5-58f9-4bcc-b2b0-f21819080ad0",
            "is_internal": false,
            "name": "Ring Alarm Bell",
            "description": null,
            "action": "POST",
            "resource": "/ring",
            "xml": null
        },
        {
            "id": "2d611223-0b9e-4ffb-83b4-518e236890b6",
            "is_internal": false,
            "name": "Unlock",
            "description": "Unlock main entrance",
            "action": "POST",
            "resource": "/door/unlock",
            "xml": null
        }
    ]
}
```

<a name="remove-a-permission-from-a-role"></a>
### ロールからパーミッションを削除

REST API を使用してパーミッションを削除するには、URL パスに `<application-id>`, `<role-id>` と `<permission-id>` を含め、ヘッダに `X-Auth-Token` を使用して自身を識別する、 DELETE リクエストを行います。

#### :two::zero: リクエスト :

```console
curl -X DELETE \
  'http://keyrock/v1/applications/{{application_id}}/roles/{{role_id}}/permissions/{{permission_id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

<a name="authorizing-application-access"></a>
# アプリケーションのアクセスを認可

最後に、ユーザはアプリケーションにログインし、自分自身を識別し、ユーザが実行できるパーミッションのリストを与えられます。ただし、パーミッションを保持し提供するユーザではなく、アプリケーションであることを強調する必要があります。ユーザは、付与されたロールを介して集約されたパーミッションのリストに関連付けられているだけです。

アプリケーションは、ユーザまたは組織のいずれかにロールを付与することができます。組織の所有者は、ユーザのメンテナンスの責任をより広いグループに委任することができるため、後者の方が常に望ましいはずです。

たとえば、スーパー・マーケットが別の店の警備員を得たとします。Alice は既に Security と呼ばれるロールを作成し、それをセキュリティ・チームに割り当てました。Charlie はセキュリティ・チームの組織の所有者であり、新しい *detective3* ユーザをチームに追加することができます。*detective3* は、アリスからのさらなる入力なしにチームのすべての権利を継承することができます。

個々のユーザにロールを付与する場合は、特別な場合に限定する必要があります。一部のロールは非常に専門的で、メンバーを1人しか含まないため、組織を作成する必要はありません。これにより、アプリケーションを設定する際の管理上の負担は軽減されましたが、他の変更 (アクセス権の削除など) は Alice 自身によって行われる必要があります。委任はできません。

<a name="authorizing-organizations"></a>
## 組織を認可

ロールは、アプリケーション自体の内部ですでに定義されていない限り、組織に付与することはできません。以前のチュートリアルで説明したように、組織を作成する必要があります。

<a name="grant-a-role-to-an-organization"></a>
### 組織にロールを付与

ロールは、アプリケーション自体の内部ですでに定義されていない限り、組織に付与することはできません。以前のチュートリアルで説明したように、組織を作成する必要があります。

![](https://fiware.github.io/tutorials.Roles-Permissions/img/add-role-to-org.png)

ロールは、組織の `members` または `owners` のいずれかに付与することができます。REST API を使用すると、次に示すように、URL パスに `<application-id>`, `<organzation-id>` と `<role-id>` を含め、ヘッダに `X-Auth-Token` を使用して自身を識別する、 POST リクエストを行うことで、ロールを付与することができます。

#### :two::one: リクエスト :

この例では、組織のすべてのメンバにロールを追加します。

```console
curl -X POST \
  'http://localhost:3005/v1/applications/{{application-id}}/organizations/{{organization-id}}/roles/{{role-id}}/organization_roles/member' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

レスポンスには、次のようなロールの割り当てが表示されます :

```json
{
    "role_organization_assignments": {
        "role_id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c",
        "organization_id": "security-0000-0000-0000-000000000000",
        "oauth_client_id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482",
        "role_organization": "member"
    }
}
```

<a name="list-granted-organization-roles"></a>
### 付与された組織ロールのリストを取得

組織に付与されたロールの完全なリストは、`/v1/applications/{{application-id}}/organizations/{{organization-id}}/roles` エンドポイントに GET リクエストを行うことで取得できます。

#### :two::two: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/organizations/{{organization-id}}/roles' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

レスポンスには、組織に割り当てられたすべてのロールが表示されます。

```json
{
    "role_organization_assignments": [
        {
            "organization_id": "security-0000-0000-0000-000000000000",
            "role_id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c"
        }
    ]
}
```

<a name="revoke-a-role-from-an-organization"></a>
### 組織からロールを取り消す

REST API を使用してロールを取り消すには、URL パスに `<application-id>`, `<organization-id>` と `<role-id>` を含め、ヘッダに `X-Auth-Token` を使用して自身を識別する、 DELETE リクエストを行います。

次の例では、組織の `members` に対して、ロールを取り消します。

#### :two::three: リクエスト :

```console
curl -iX DELETE \
  'http://localhost:3005/v1/applications/{{application-id}}/organizations/{{organization-id}}/roles/{{role-id}}/organization_roles/member' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

<a name="authorizing-individual-user-accounts"></a>
## 個々のユーザ・アカウントを認可

ロールがすでにアプリケーションに関連付けられていない限り、定義されたロールをユーザに付与することはできません。

<a name="grant-a-role-to-a-user"></a>
### ユーザにロールを付与

GUI によるユーザ・アクセスの付与は、組織と同じ方法で実行できます。

ロールは、組織の `members` または `owners` のいずれかに付与することができます。REST API を使用すると、次に示すように、URL パスに `<application-id>`, `<user-id>` と `<role-id>` を含め、ヘッダに `X-Auth-Token` を使用して自身を識別する、 POST リクエストを行うことで、ロールを付与することができます。

#### :two::four: リクエスト :

```console
curl -X POST \
  'http://localhost:3005/v1/applications/{{application-id}}/users/{{user-id}}/roles/{{role-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

```json
{
    "role_user_assignments": {
        "role_id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c",
        "user_id": "bbbbbbbb-good-0000-0000-000000000000",
        "oauth_client_id": "3782c5e3-88f9-481a-9b3c-2f2d6f604482"
    }
}
```

<a name="list-granted-user-roles"></a>
### 付与されたユーザ・ロールのリストを取得

個々のユーザに付与されたロールをリスト表示するには、`v1/applications/{{application-id}}/users/{{user-id}}/roles` エンドポイントに GET リクエストを行います。

#### :two::five: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/users/{{user-id}}/roles' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

レスポンスは、ユーザに割り当てられたすべてのロールを返します。

```json
{
    "role_user_assignments": [
        {
            "user_id": "bbbbbbbb-good-0000-0000-000000000000",
            "role_id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c"
        }
    ]
}
```

<a name="revoke-a-role-from-a-user"></a>
### ユーザからロールを取り消す

組織と同様に、REST API を使用してロールを取り消すには、URL パスに `<application-id>`, `<user-id>` と `<role-id>` を含め、ヘッダに `X-Auth-Token` を使用して自身を識別する、 DELETE リクエストを行います。

#### :two::six: リクエスト :

```console
curl -X DELETE \
  'http://localhost:3005/v1/applications/{{application-id}}/users/{{user-id}}/roles/{{role-id}}' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

<a name="list-application-grantees"></a>
# アプリケーション付与のリストを取得

一連のロールを作成し、それらをユーザや組織に付与することによって、それらの間の関連付けを行いました。REST API には、アプリケーションのすべての付与をリスト表示する2つの便利なメソッドがあります。

<a name="list-authorized-organizations"></a>
### 認可された組織のリストを取得

アプリケーションの使用を認可されているすべての組織をリストするには、`/v1/applications/{{application-id}}/organizations` エンドポイントに GET リクエストを行います。

#### :two::seven: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/organizations' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

レスポンスは、アプリケーションにアクセスできるすべての組織と、割り当てられているロールを返します。個々のメンバはリストされていません。

```json
{
    "role_organization_assignments": [
        {
            "organization_id": "security-0000-0000-0000-000000000000",
            "role_organization": "member",
            "role_id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c"
        }
    ]
}
```

<a name="list-authorized-users"></a>
### 認可されたユーザのリストを取得

アプリケーションの使用を許可されている個々のユーザをすべてリスト表示するには、`/v1/applications/{{application-id}}/users` エンドポイントに GET リクエストを行います。

#### :two::eight: リクエスト :

```console
curl -X GET \
  'http://localhost:3005/v1/applications/{{application-id}}/users' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: {{X-Auth-token}}'
```

#### レスポンス :

レスポンスは、アプリケーションにアクセスできる個々のユーザと、割り当てられたロールを返します。アクセス権を付与された組織のユーザはリストされていないことに注意してください。

```json
{
    "role_user_assignments": [
        {
            "user_id": "aaaaaaaa-good-0000-0000-000000000000",
            "role_id": "provider"
        },
        {
            "user_id": "bbbbbbbb-good-0000-0000-000000000000",
            "role_id": "64535f4d-04b6-4688-a9bb-81b8df7c4e2c"
        }
    ]
}
```
<a name="next-steps"></a>
# 次のステップ

高度な機能を追加することで、アプリケーションに複雑さを加える方法を知りたいですか？このシリーズの[他のチュートリアル](https://www.letsfiware.jp/fiware-tutorials)を読むことで見つけることができます。

---

## License

[MIT](LICENSE) © FIWARE Foundation e.V.
