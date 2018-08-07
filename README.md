[![FIWARE Banner](https://fiware.github.io/tutorials.Roles-Permissions/img/fiware.png)](https://www.fiware.org/developers)

[![FIWARE Security](https://img.shields.io/badge/FIWARE-Security-ff7059.svg?logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABsAAAAVCAYAAAC33pUlAAAABHNCSVQICAgIfAhkiAAAA8NJREFUSEuVlUtIFlEUx+eO+j3Uz8wSLLJ3pBiBUljRu1WLCAKXbXpQEUFERSQF0aKVFAUVrSJalNXGgmphFEhQiZEIPQwKLbEUK7VvZrRvbr8zzjfNl4/swplz7rn/8z/33HtmRhn/MWzbXmloHVeG0a+VSmAXorXS+oehVD9+0zDN9mgk8n0sWtYnHo5tT9daH4BsM+THQC8naK02jCZ83/HlKaVSzBey1sm8BP9nnUpdjOfl/Qyzj5ust6cnO5FItJLoJqB6yJ4QuNcjVOohegpihshS4F6S7DTVVlNtFFxzNBa7kcaEwUGcbVnH8xOJD67WG9n1NILuKtOsQG9FngOc+lciic1iQ8uQGhJ1kVAKKXUs60RoQ5km93IfaREvuoFj7PZsy9rGXE9G/NhBsDOJ63Acp1J82eFU7OIVO1OxWGwpSU5hb0GqfMydMHYSdiMVnncNY5Vy3VbwRUEydvEaRxmAOSSqJMlJISTxS9YWTYLcg3B253xsPkc5lXk3XLlwrPLuDPKDqDIutzYaj3eweMkPeCCahO3+fEIF8SfLtg/5oI3Mh0ylKM4YRBaYzuBgPuRnBYD3mmhA1X5Aka8NKl4nNz7BaKTzSgsLCzWbvyo4eK9r15WwLKRAmmCXXDoA1kaG2F4jWFbgkxUnlcrB/xj5iHxFPiBN4JekY4nZ6ccOiQ87hgwhe+TOdogT1nfpgEDTvYAucIwHxBfNyhpGrR+F8x00WD33VCNTOr/Wd+9C51Ben7S0ZJUq3qZJ2OkZz+cL87ZfWuePlwRcHZjeUMxFwTrJZAJfSvyWZc1VgORTY8rBcubetdiOk+CO+jPOcCRTF+oZ0okUIyuQeSNL/lPrulg8flhmJHmE2gBpE9xrJNkwpN4rQIIyujGoELCQz8ggG38iGzjKkXufJ2Klun1iu65bnJub2yut3xbEK3UvsDEInCmvA6YjMeE1bCn8F9JBe1eAnS2JksmkIlEDfi8R46kkEkMWdqOv+AvS9rcp2bvk8OAESvgox7h4aWNMLd32jSMLvuwDAwORSE7Oe3ZRKrFwvYGrPOBJ2nZ20Op/mqKNzgraOTPt6Bnx5citUINIczX/jUw3xGL2+ia8KAvsvp0ePoL5hXkXO5YvQYSFAiqcJX8E/gyX8QUvv8eh9XUq3h7mE9tLJoNKqnhHXmCO+dtJ4ybSkH1jc9XRaHTMz1tATBe2UEkeAdKu/zWIkUbZxD+veLxEQhhUFmbnvOezsJrk+zmqMo6vIL2OXzPvQ8v7dgtpoQnkF/LP8Ruu9zXdJHg4igAAAABJRU5ErkJgggA=)](https://www.fiware.org/developers/catalogue/)
[![Documentation](https://readthedocs.org/projects/fiware-tutorials/badge/?version=latest)](https://fiware-tutorials.readthedocs.io/en/latest)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

The tutorial explains how to create applications, and how to assign roles and permissions to them.
It takes the users and organizations created in the [previous tutorial](https://github.com/Fiware/tutorials.Identity-Management)
and ensures that only legitmate users will have access to resources.

The tutorial demonstrates examples of interactions using the **Keyrock** GUI, as well [cUrl](https://ec.haxx.se/) commands used
to access the **Keyrock** REST API - [Postman documentation](http://fiware.github.io/tutorials.Roles-Permissions/) is also available.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/2150531e68299d46f937)

# Contents

- [What is Authorization?](#what-is-authorization)
  * [Standard Concepts of Identity Management](#standard-concepts-of-identity-management)
  * [OAuth2](#oauth2)
- [Prerequisites](#prerequisites)
  * [Docker](#docker)
  * [Cygwin](#cygwin)
- [Architecture](#architecture)
  * [Keyrock Configuration](#keyrock-configuration)
  * [MySQL Configuration](#mysql-configuration)
- [Start Up](#start-up)
    + [Dramatis Personae](#dramatis-personae)
    + [Reading directly from the Keyrock MySQL Database](#reading-directly-from-the-keyrock-mysql-database)
    + [UUIDs within Keyrock](#uuids-within-keyrock)
  * [Logging In via REST API calls](#logging-in-via-rest-api-calls)
    + [Create Token with Password](#create-token-with-password)
    + [Get Token Info](#get-token-info)
- [Managing Applications](#managing-applications)
  * [Application CRUD Actions](#application-crud-actions)
    + [Create an Application](#create-an-application)
    + [Read Application Details](#read-application-details)
    + [List all Applications](#list-all-applications)
    + [Update an Application](#update-an-application)
    + [Delete an Application](#delete-an-application)
  * [Permission CRUD Actions](#permission-crud-actions)
    + [List Permissions](#list-permissions)
    + [Create a Permission](#create-a-permission)
    + [Read Permission Details](#read-permission-details)
    + [Delete an Permission](#delete-an-permission)
  * [Role CRUD Actions](#role-crud-actions)
    + [List Roles](#list-roles)
    + [Create a Role](#create-a-role)
    + [Read Role Details](#read-role-details)
    + [Delete a Role](#delete-a-role)
  * [Assigning Permissions to each Role](#assigning-permissions-to-each-role)
    + [List Permissions of a Role](#list-permissions-of-a-role)
    + [Add a Permission to a Role](#add-a-permission-to-a-role)
    + [Remove a Permission from a Role](#remove-a-permission-from-a-role)
- [Authorizing Application Access](#authorizing-application-access)
  * [Roles for Groups of Users](#roles-for-groups-of-users)
    + [List Roles of an Organization](#list-roles-of-an-organization)
    + [Add a Role to an Organization](#add-a-role-to-an-organization)
    + [Remove a Role from an Organization](#remove-a-role-from-an-organization)
  * [Roles for User Accounts](#roles-for-user-accounts)
    + [List Roles of a Users](#list-roles-of-a-users)
    + [Add a Role to a Users](#add-a-role-to-a-users)
    + [Remove a Role from a Users](#remove-a-role-from-a-users)
- [Defining Other Actors within an Application](#defining-other-actors-within-an-application)
  * [Defining Users of an Application](#defining-users-of-an-application)
    + [List Users of an Application](#list-users-of-an-application)
    + [Add a User to an Application](#add-a-user-to-an-application)
    + [Remove a User from an Application](#remove-a-user-from-an-application)
  * [Defining Organizations using an Application](#defining-organizations-using-an-application)
    + [List Organizations of an Application](#list-organizations-of-an-application)
    + [Add a Organization to an Application](#add-a-organization-to-an-application)
    + [Remove a Organization from an Application](#remove-a-organization-from-an-application)
- [Application Users and Other Actors](#application-users-and-other-actors)
  * [Defining Other Actors within an Application](#defining-other-actors-within-an-application-1)
    + [Securing PEP Proxies within an Application](#securing-pep-proxies-within-an-application)
    + [Securing IoT Agents within an Application](#securing-iot-agents-within-an-application)
- [Next Steps](#next-steps)


# What is Authorization?

> "No matter what he does, every person on earth plays a central role in the history of the world.
> And normally he doesn't know it"
>
> — Paulo Coelho (The Alchemist)


Authorization is the function of specifying access rights/privileges to resources related to information
security. More formally, "to authorize" is to define an access policy. In the case of Keyrock, User
access is granted based on permissions assigned to a role.

Every application secured by the **Keyrock** generic enabler can define a set of permissions - i.e.
a set of things that can be done within the application. For example within the application, the ability
to send a commmand to unlock a Smart Door could be secured behind a `Unlock Door` permission. Similarly
the ability to send a commmand to ring the alarm bell could be secured behind a `Ring Bell` permission,
and the ability to alter prices could be secured behind a `Price Change` permission

These permissions are grouped together in a series of roles - for example `Unlock Door` and `Ring Bell`
could both be assigned to the Security Role, meaning that Users who are subsequently given that role
would gain both permissions.

Permissions can overlap and be assigned to multiple roles - maybe `Ring Bell` is also assigned to the management
role along with `Price Change` and `Order Stock`.

In turn users or organizations will be assigned to one of more roles - each user will gain the sum of all the
permissions for each role they have. For example if Alice is assigned to both management and security roles,
she will gain all four permissions `Unlock Door`, `Ring Bell`, `Price Change` and `Order Stock`.

The concept of a role is unknown to a user - they only know the list of permissions they have been granted,
not how the permissions are split up within the application.

In summary, permissions are all the possible actions that can be done to resources within an application, whereas roles
are groups of actions which can be done by a type of user of that application.


## Standard Concepts of Identity Management

The following common objects are found with the **Keyrock** Identity Management database:

* **User** - Any signed up user able to identify themselves with an eMail and password. Users can be assigned
 rights individually or as a group
* **Application** -  Any securable FIWARE application consisting of a series of microservices
* **Organization** - A group of users who can be assigned a series of rights. Altering the rights of the organization
 effects the access of all users of that organization
* **OrganizationRole** - Users can either be members or admins of an organization - Admins are able to add and remove users
 from their organization, members merely gain the roles and permissions of an organiation. This allows each organization
 to be responisible for their members and removes the need for a super-admin to administer all rights
* **Role** - A role is a descriptive bucket for a set of permissions. A role can be assigned to either a single user
 or an organization. A signed-in user gains all the permissions from all of their own roles plus all of the roles associated
 to their organization
* **Permission** - An ability to do something on a resource within the system

Additionally two further non-human application objects can be secured within a FIWARE application:

* **IoTAgent** - a proxy betwen IoT Sensors and  the Context Broker
* **PEPProxy** - a middleware for use between generic enablers challenging the rights of a user.


 The relationship between the objects can be seen below:

![](https://fiware.github.io/tutorials.Roles-Permissions/img/entities.png)

## OAuth2

**Keyrock** uses [OAuth2](https://oauth.net/2/) to enable third-party applications
to obtain limited access to services. **OAuth2** is the open standard for access delegation to
grant access rights. It allows notifying a resource provider (e.g. the Knowage Generic Enabler)
that the resource  owner (e.g. you) grants permission to a third-party (e.g. a Knowage Application)
access to their information (e.g. the list of entities).

There are several common OAuth 2.0 grant flows, the details of which can be found below:

* [Authorization Code](https://oauth.net/2/grant-types/authorization-code)
* [Implicit](https://oauth.net/2/grant-types/implicit)
* [Password](https://oauth.net/2/grant-types/password)
* [Client Credentials](https://oauth.net/2/grant-types/client-credentials)
* [Device Code](https://oauth.net/2/grant-types/device-code)
* [Refresh Token](https://oauth.net/2/grant-types/refresh-token)

The primary concept is that both **Users**  and **Applications** must first identify themselves using
a standard OAuth2 Challenge-Response mechanism. Thereafter a user is assigned a token which they
append to every subsequent request. This token identifies the user, the application and the rights the
user is able to exercise.  **Keyrock** can then be used with other enablers can be used to limit and
lock-down access. The details of the access flows are discussed below and in subsequent tutorials.

The reasoning behind OAuth2 is that you never need to expose your own username and password to a
third party to give them  full access - you merely permit the relevant access which can be either Read-Only
or Read-Write and such access can be defined down to a granular level. Furthermore there is provision for
revoking access at any time, leaving the resource owner in control of who can access what.


# Prerequisites

## Docker

To keep things simple both components will be run using [Docker](https://www.docker.com). **Docker** is a
container technology which allows to different components isolated into their respective environments.

* To install Docker on Windows follow the instructions [here](https://docs.docker.com/docker-for-windows/)
* To install Docker on Mac follow the instructions [here](https://docs.docker.com/docker-for-mac/)
* To install Docker on Linux follow the instructions [here](https://docs.docker.com/install/)

**Docker Compose** is a tool for defining and running multi-container Docker applications. A
[YAML file](https://raw.githubusercontent.com/Fiware/tutorials.Entity-Relationships/master/docker-compose.yml) is used
configure the required services for the application. This means all container services can be brought up in a single
command. Docker Compose is installed by default as part of Docker for Windows and  Docker for Mac, however Linux users
will need to follow the instructions found  [here](https://docs.docker.com/compose/install/)

## Cygwin

We will start up our services using a simple bash script. Windows users should download [cygwin](http://www.cygwin.com/) to provide a
command line functionality similar to a Linux distribution on Windows.

# Architecture

This introduction will only make use of one FIWARE component - the [Keyrock](http://fiware-idm.readthedocs.io/)
Identity Management Generic Enabler. Usage of **Keyrock** alone alone is insufficient for an application to qualify
 as *“Powered by FIWARE”*.  Additionally will be persisting user data in a **MySQL**  database.


The overall architecture will consist of the following elements:

* One **FIWARE Generic Enabler**:
    * FIWARE [Keyrock](http://fiware-idm.readthedocs.io/) offer a complement Identity Management System including:
        * An OAuth2 authentication system for Applications and Users
        * A website graphical front-end for Identity Management Administration
        * An equivalent REST API for Identity Management via HTTP requests

* One [MySQL](https://www.mysql.com/) database :
    * Used to persist user identities, applications, roles and permsissions


Since all interactions between the elements are initiated by HTTP requests, the entities can be containerized and run from exposed ports.


![](https://fiware.github.io/tutorials.Roles-Permissions/img/architecture.png)

The specific architecture of each section of the tutorial is discussed below.

## Keyrock Configuration

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

The `idm` container is a web app server listening on a single port:

* Port `3005` has been  exposed for HTTP traffic so we can display the webpage and interact with the REST API.

> **Note** The HTTP protocols is being used for demonstration purposes only.
> In a production environment, all OAuth2 Authentication should occur over HTTPS, to avoid sending
> any sensitive information using plain-text.

The `idm` container is driven by environment variables as shown:

| Key |Value|Description|
|-----|-----|-----------|
|IDM_DB_PASS|`idm`| Password of the attached MySQL Database - secured by **Docker Secrets** (see below) |
|IDM_DB_USER|`root`|Username of the default MySQL user - left in plain-text |
|IDM_HOST|`http://localhost:3005`| Hostname of the **Keyrock**  App Server - used in activation eMails when signing up users|
|IDM_PORT|`3005`| Port used by the **Keyrock** App Server  - this has been altered from the default 3000 port to avoid clashes |


> :information_source: **Note** that this example has secured the MySQL password using **Docker Secrets**
> By using `IDM_DB_PASS` with the `_FILE` suffix and referring to a secrets file location.
> This avoids exposing the password as an `ENV` variable in plain-text - either in the `Dockerfile` Image or
> as an injected variable which could be read using `docker inspect`.
>
> The following list of variables (where used) should be set via secrets with the  `_FILE` suffix  in a Production System:
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



## MySQL Configuration

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


The `mysql-db` container is listening on a single port:

* Port `3306` is the default port for a MySQL server. It has been exposed so you can also run other database tools to display data if you wish

The `mysql-db` container is driven by environment variables as shown:

| Key               |Value.    |Description                               |
|-------------------|----------|------------------------------------------|
|MYSQL_ROOT_PASSWORD|`123`.    | specifies a password that is set for the MySQL `root` account - secured by **Docker Secrets** (see below)|
|MYSQL_ROOT_HOST    |`root`| By default, MySQL creates the `root'@'localhost` account. This account can only be connected to from inside the container. Setting this environment variable allows root connections from other hosts |

# Start Up

To start the installation, do the following:

```console
git clone git@github.com:Fiware/tutorials.Roles-Permissions.git
cd tutorials.Roles-Permissions

./services create
```

>**Note** The initial creation of Docker images can take up to three minutes


Thereafter, all services can be initialized from the command line by running the [services](https://github.com/Fiware/tutorials.Roles-Permissions/blob/master/services) Bash script provided within the repository:

```console
./services <command>
```

Where `<command>` will vary depending upon the exercise we wish to activate.

>:information_source: **Note:** If you want to clean up and start over again you can do so with the following command:
>
>```console
>./services stop
>```
>


### Dramatis Personae

The following people at `test.com` legitimately have accounts within the Application

* Alice, she will be the Administrator of the **Keyrock** Application
* Bob, the Regional Manager of the supermarket chain - he has several store managers under him:
  * Manager1
  * Manager2
* Charlie, the Head of Security of the supermarket chain - he has several store detectives under him:
  * Detective1
  * Detective2

The following people at `example.com`  have signed up for accounts, but have no reason to be granted access

* Eve - Eve the Eavesdropper
* Mallory - Mallory the malicious attacker
* Rob - Rob the Robber


| Name       |eMail                       |Password | UUID                                  |
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


Two organizations have also been set up by Alice:

| Name       | Description                         | UUID                                 |
|------------|-------------------------------------|--------------------------------------|
| Security   | Security Group for Store Detectives |`security-0000-0000-0000-000000000000`|
| Management | Management Group for Store Managers |`managers-0000-0000-0000-000000000000`|

The data creating users and organizations from the [previous tutorial](https://github.com/Fiware/tutorials.Identity-Management) has been downloaded:

```console
docker exec db-mysql /usr/bin/mysqldump -u root --password=idmx idm > backup.sql
```

and is injected into the MySQL Database on start-up.




To refresh your memory about how to create users and organizations, you can log in at `http://localhost:3005/idm`
using the account `alice-the-admin@test.com` with a password of `test`.

![](https://fiware.github.io/tutorials.Roles-Permissions/img/log-in.png)

and look at the organizations list.



### Reading directly from the Keyrock MySQL Database

All Identify Management records  and releationships are held within the the attached MySQL database. This can be
accessed by entering the running Docker container as shown:


```console
docker exec -it db-mysql bash
```

```console
mysql -u <user> -p<password> idm
```

Where `<user>` and `<password>` match the values defined in the `docker-compose` file for `MYSQL_ROOT_PASSWORD`
and `MYSQL_ROOT_USER`. The default values for the tutorial are usually `root` and `secret`.

SQL commands can then be entered from the command line. e.g.:

```SQL
select id, username, email, password from user;
```


### UUIDs within Keyrock

All ids and tokens within  **Keyrock** are subject to change. The following values will need to be amended when
querying for records .Record ids use Universally Unique Identifiers - UUIDs.

| Key |Description                        | Sample Value |
|-----|-----------------------------------|--------------|
|`keyrock`| URL for the location of the **Keyrock** service|`localhost:3005`|
|`X-Auth-token`| Token received in the Header when logging in as a user |`aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa` = I am Alice|
|`X-Subject-token`|Token to pass when asking about a subject, alternatively repeat the user token |`bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb` = Asking about Bob|
|`user-id`| id of an existing user, found with the `user`  table |`bbbbbbbb-good-0000-0000-000000000000` - Bob's User Id|
|`application-id`| id of an existing application, found with the `oauth_client` table |`c978218d-ad63-4427-b12b-542b81299cfb`|
|`role-id`| id of an existing role, found with the `role` table |`d28baa00-839e-4b45-a6b2-1cec563942ee`|
|`permission-id`| id of an existing permission, found with the `permission`  table |`6b6cd19c-9398-4834-9ba1-1616c57139c0`|
|`organization-id`| id of an existing organization, found with the `organization`  table |`e424ed98-c966-46e3-b161-a165fd31bc01`|
|`organization-role-id`| type of role a user has within an organization either `owner` or `member`|`member`|
|`iot-agent-id`| id of an existing IoT Agent, found with the `iot`  table  |`iot_sensor_f3d0245b-3330-4e64-a513-81bf4b0dae64`|
|`pep-proxy-id`| id of an existing PEP Proxy, found with the `pep_proxy`  table  |`iot_sensor_f3d0245b-3330-4e64-a513-81bf4b0dae64`|

Tokens are designed to expire after a set period. If the `X-Auth-token` value you are using has expired, log-in again to obtain a new token. For this tutorial, a long lasting set of tokens has been created for each user and persisted into the database,
so there is usually no need to refresh tokens.

## Logging In via REST API calls


Enter a username and password to enter the application. The default super-user has the values `alice-the-admin@test.com` and `test`.

### Create Token with Password

The following example logs in using the Admin Super-User:

#### :one: Request:
```console
curl -iX POST \
  'http://localhost:3005/v1/auth/tokens' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "alice-the-admin@test.com",
  "password": "test"
}'
```

#### Response:

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

### Get Token Info

You can use the long-lasting  `X-Auth-token=aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa` to pretend to be Alice throughout this
tutorial. To find information about Bob, use the long-lasting token `X-Subject-token=bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb`

#### :two: Request:

```console
curl -iX GET \
  'http://localhost:3005/v1/auth/tokens' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'X-Auth-token: aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa' \
  -H 'X-Subject-token: bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb'
```

#### Response:

The response will return the details of the associated user. As you can see Bob holds a long-lasting token until 2026.

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



# Managing Applications

Any FIWARE application can be broken down into a collection of microservices. These microservices connect together to read
and alter the state of the real world. Security can be added to these services by restricting actions on these resources
down to users how have appropriate permissions. It is therefore necessary to define an application to offer a set of permissible
actions and to hold a list of permitted users (or groups of users i.e. an Organization)

Applications are therefore a conceptual bucket holding who can do what on which resource.


## Application CRUD Actions

The standard CRUD actions are assigned to the appropriate HTTP verbs (POST, GET, PATCH and DELETE) under the `/v1/applications` endpoint.

### Create an Application

### Read Application Details

### List all Applications

### Update an Application

### Delete an Application


## Permission CRUD Actions

### List Permissions

### Create a Permission

### Read Permission Details

### Delete an Permission


## Role CRUD Actions


A permission is an allowable action on a resource. A role consists of a group of permissions, in other words a series of
permitted actions over a group of resources. Roles are usually usually given a description with a broad scope so that
they can be assigned to a wide range of users or organizations for example a *Reader* role could be able to access but
not update a series of devices.

There are two pre-defined roles with **Keyrock** :

* a *Purchaser* who can
   + Get and assign all public application roles
* a *Provider* who can:
   + Get and assign only public owned roles
   + Get and assign all public application roles
   + Manage authorizations
   + Manage roles
   + Manage the application
   + Get and assign all internal application roles

Using our Supermarket Store Example, Alice the admin would be assigned the *Provider* role, she could then create any additional
application-specific  roles needed (such as *Door Locker* or *Price Changer*) She could then delegate *Purchaser* roles to Bob
the regional manager and Charlie, Head of Security, who could assign the roles as necessary.




### List Roles

### Create a Role

### Read Role Details

### Delete a Role

## Assigning Permissions to each Role

### List Permissions of a Role

### Add a Permission to a Role

### Remove a Permission from a Role



# Authorizing Application Access

Finally

## Roles for Groups of Users

### List Roles of an Organization

### Add a Role to an Organization

### Remove a Role from an Organization


## Roles for User Accounts

A defined role cannot be assigned to a user unless it the role has already been associated to an application

### List Roles of a Users

### Add a Role to a Users

### Remove a Role from a Users





# Defining Other Actors within an Application


## Defining Users of an Application

### List Users of an Application

### Add a User to an Application

### Remove a User from an Application


##  Defining Organizations using an Application

### List Organizations of an Application

### Add a Organization to an Application

### Remove a Organization from an Application





# Application Users and Other Actors



## Defining Other Actors within an Application

Although

### Securing PEP Proxies within an Application

#### Create an PEP Proxy

#### Read PEP Proxy Details

#### List PEP Proxies

#### Reset the Password of an PEP Proxy

#### Delete an PEP Proxy


### Securing IoT Agents within an Application

#### Create an IoT Agent

#### Read IoT Agent Details

#### List IoT Agents

#### Reset the Password of an IoT Agent

#### Delete an IoT Agent

---




# Next Steps

Want to learn how to add more complexity to your application by adding advanced features?
You can find out by reading the other [tutorials in this series](https://fiware-tutorials.readthedocs.io/en/latest)

---

## License

[MIT](LICENSE) © FIWARE Foundation e.V.









