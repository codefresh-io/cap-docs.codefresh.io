---
title: "Install Codefresh on-premises"
description: ""
group: runtime
toc: true
---

Codefresh supports on-premises installations using Helm charts. 

Codefresh on-premises installation can be divided into the following:

**Installation preparation**  
This stage includes the tasks before you start installation, such as verifying system requirements and prerequisites for installation.


**Codefresh on-premises installation**
This stage includes  installation, configuration, and deployment.

**Additional configurations**
This stage includes several post-installation configuration options based on your environment. 



### System requirements for on-premises installations
The table describes the minimum requirements for an on-premises installation.

{: .table .table-bordered .table-hover}
| Item                     | Requirement            |  
| --------------         | --------------           |  
|Kubernetes cluster      | Server version 1.19 to 1.22  {::nomarkdown}<br><b>Tip</b>:  To check the server version, run:<br> <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl version --short</span>.{:/}|
|Node requirements | {::nomarkdown}<ul><li>Memory: 5000 MB</li><li>CPU: 2</li></ul>{:/}|
|Git providers     |{::nomarkdown}<ul><li>GitHub</li></ul>{:/}|


### Prerequisites for on-premises installations
Make sure you have the following:

#### Service Account file
The GCR Service Account JSON file, `sa.json` is provided by Codefresh. Contact support to get the file before installation.


#### TLS certificates
For a secured installation, we highly recommend using TLS certificates. 
Make sure your `ssl.cert` and `private.key` are valid.

>Use a Corporate Signed certificate, or any valid TLS certificate, for example, from `lets-encrypt`.

#### PV provisioner support
Codefresh uses both cluster storage (volumes), as well as external storage. Make sure you meet the minimum capacity for the volumes.

##### Databases
The table below displays the list of databases created as part of the installation:

| Database | Purpose | | Minimum Capacity |Latest supported version |
|----------|---------| --------------     |---------|
| mongoDB    | Store all account data (account settings, users, projects, pipelines, builds etc.) | 8GB          | 4.2.x |
| postgresql | Store data about events in the account (pipeline updates, deletes, etc.). The audit log uses the data from this database. | 8GB          | 13.x |
| rabbitmq   | Message broker. | 8GB        | 3.10.x |
| redis      | Cache data, and key-value store for trigger manager. | 8GB          | 6.0.x |

> Running on netfs (nfs, cifs) is not recommended by the product admin guide.



### Install Codefresh on-premises
Follow the steps to install and deploy Codefresh on-premises.

{::nomarkdown}
</br></br>
{:/}

#### Before you begin
Make sure you:
* Meet the [system requirements](#system-requirements-for-on-premises-installations)
* Have completed the [prerequisites](#prerequisties-for-on-premises-installations)

{::nomarkdown}
</br></br>
{:/}

#### Step 1: Get repo info nad pull Helm chart
Retrieve the info on the Codefresh repo and pull the Heml chart for on-premises installation.

```yaml
helm repo add codefresh-onprem-dev http://chartmuseum-dev.codefresh.io/codefresh
helm repo update
helm pull codefresh-onprem-dev/codefresh --untar --version 1.2.18-onprem-argo-platform
```
{::nomarkdown}
</br></br>
{:/}

#### Step 2: Define the values.yaml to use
Use either the provided `values.yaml` and customize it as needed, or create a new, empty values file, such as `cf-values.yaml` and add the required settings. 

> For the purposes of documentation, in this section, we will use `values.yaml`.

{::nomarkdown}
</br></br>
{:/}

#### Step 3: Pass sa.json credentials
Pass `sa.json` as a single line for `password`.

```yaml
  imageCredentials:
  registry: gcr.io
  username: _json_key
  password: '{ "type": "service_account", "project_id": "codefresh-enterprise", "private_key_id": ... }'
```

{: .table .table-bordered .table-hover}
| Parameter      | Description            | Default value | 
|----------------|------------------------|------------------|
| `registry`     | The address of the registry to store the service account image.  | `gcr.io`              | 
| `username`     | The username to access the registry.  | `_json_key`              | 
| `password`     | The password to access the registry, and must include the content of `sa.json` in a single line.  | ``              | 

{::nomarkdown}
</br></br>
{:/}

#### Step 4: Define `global.appUrl`
Define the root URL for Codefresh. 

```yaml
global:
  appUrl: <onprem>.<mydomain>
```

{: .table .table-bordered .table-hover}
| Parameter      | Description            | Default value | 
|----------------|------------------------|------------------|
| `appUrl`       | The root URL of the codefresh application.  | `onprem.codefresh.local`              | 

{::nomarkdown}
</br></br>
{:/}

#### Step 5: Create TLS certificates
Enable TLS and certificates for ingress objects.

```yaml
webTLS:
  enabled: true
  secretName: star.codefresh.io
  cert: "" # (base64 encoded ssl certificate)
  key: "" # (base64 encoded private key)
```

{: .table .table-bordered .table-hover}
| Parameter      | Description            | Default value | 
|----------------|------------------------|------------------|
| `enabled`      | Enable and create TLS certificates for ingress objects.   | `true`              | 
| `secretName`   | The name of the TLS secret.                               | `star.codefresh.io`              |
| `cert`         | The base64 encoded custom certificate.                    |  `  `            |
| `key`          | The base64 encoded custom private key for the certificate. | `  `              |

{::nomarkdown}
</br></br>
{:/}

#### Step 6: Enable Codefresh On-premises 
Enable the Codefresh on-premises platform, and disable services that are not required, such as the Helm charts for Codefresh Classic:

```yaml
tags:
  cf-infra: false
  argo-platform: true

argo-platform:
  enabled: true

  seedJobs:
    enabled: true
```

{::nomarkdown}
</br></br>
{:/}

#### Step 7: Install the Helm chart
Install the Helm chart with the configuration to deploy Codefresh on-premises.

```yaml
helm upgrade --install cf ./codefresh \
    -f cf-values.yaml \
    --namespace codefresh \
    --create-namespace \
    --debug \
    --wait \
    --timeout 10m
```
>For parameter details, see the offical [Helm Upgrade](https://helm.sh/docs/helm/helm_upgrade/){:target="\_blank"} documentation.

{::nomarkdown}
</br></br>
{:/}

#### Step 8: Continue with additional configuration
There are several post-installation additional configuration options, based on your environment:  

* [Ingressless runtime provisioning](#configure-ingressless-runtime)
* [Annotations for NGINX ingress controller](#configure-annotations-for-nginx-ingress-controller)
* [Configure external services for database/messaging/caching](#configure-external-services-for-database-messaging-caching)



{::nomarkdown}
</br></br>
{:/}

### Additional configuration for on-premises installations
Apart from the mandatory settings you need to define to install Codefresh on-premises with Helm, you can define additional settings, based on your requirements.

#### Configure ingressless runtimes
Codefresh provides the option to provision runtimes in on-premises environments without an ingress controller. 
To provision ingressless runtimes, add the `codefresh-tunnel-server` section with the tunnel server defnitions to `values.yaml`. 

```yaml
codefresh-tunnel-server:
  enabled: true  # enable ingressless runtime

  codefreshBaseUrl: https://onprem.mydomain.com 

  tunnels:
    subdomainHost: tunnels.mydomain.com 

  ingress:
    host: register-tunnels.mydomain.com
```

{: .table .table-bordered .table-hover}
| Parameter      | Description            | Default value | 
|----------------|------------------------|------------------|
| `codefreshBaseUrl`  | The name of your domain URL.   | `cf-values.yaml`              | 
| `subdomainHost`     | ??               | `codefresh`              |
| `host`              | ??                                             |  `  `            |

{::nomarkdown}
</br></br>
{:/}

#### Configure annotations for NGINX ingress controller
By default Codefresh uses NGINX as the ingress controller. If you are using AWS L7 ELB with SSL Termination, add the annotations for `ingres-nginx`, and set `webTLS` to `false`.


```yaml
ingress-nginx:
  controller:
    service:
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-backend-protocol: "tcp"
        service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "443"
        service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: '3600'
        service.beta.kubernetes.io/aws-load-balancer-ssl-cert: < CERTIFICATE ARN >
      targetPorts:
        http: http
        https: http

webTLS:
  enabled: false
```
{::nomarkdown}
</br></br>
{:/}

#### Configure external services for database/messaging/caching
By default, Codefresh  uses internal services such as `cf-mongodb`, `cf-redis`, `cf-rabbitmq`, `cf-postgresql` to run the Argo project components.  
If you have your own services for data storage/messaging/caching, configure them in `values.yaml`.   

>For Codefresh on-premises, you need to define two sets of parameters, `global`, and `argo-platform`. We recommend using the same values for both sets of parameters.
  YAML anchors are used to populate connection URIs to env variables for the corresponding Codefresh services.

* Define the `global` parameters and values, and then copy the values to `argo-platform`.  
  Here is an example of `values.yaml` with all the external services:

```yaml

global:
  ...

  mongodbRootUser: root # privileged user will be used for seed jobs and for automatic user creation
  mongodbRootPassword: <password> # replace with the MongoDB root password 
  mongoURI: mongodb://cfuser:<password>@my-mongodb.bitnami.svc.cluster.local

  postgresSeedJob:
    user: postgres
    password: <password> 
  postgresUser: cf_user
  postgresPassword: <password> 
  postgresDatabase: codefresh
  postgresHostname: my-postgresql.bitnami.svc.cluster.local
  postgresPort: 5432

  redisUrl: my-redis-master.bitnami.svc.cluster.local
  redisPort: 6379
  redisPassword: <password> 

  rabbitmqHostname: my-rabbitmq.bitnami.svc.cluster.local
  rabbitmqUsername: user
  rabbitmqPassword: WBkKrFtyb8K5600y


argo-platform:
  ...

  seedJobs:
    enabled: true
    mongodbRootUser: root
    mongodbRootPassword: <password>
    mongoUri: mongodb://argo-platform:<password>@my-mongodb.bitnami.svc.cluster.local:27017
    postgresUser: postgres
    postgresPassword: eC9arYka4ZbH
    postgresHostname: cf-postgresql

  env:
    MONGODB_PROTOCOL: &MONGODB_PROTOCOL mongodb # mongodb OR mongodb+srv
    RABBITMQ_PROTOCOL: &RABBITMQ_PROTOCOL amqp # amqp OR amqps
  secrets:
    rabbitmq-host: &rabbitmq-host my-rabbitmq.bitnami.svc.cluster.local
    rabbitmq-port: &rabbitmq-port 5672
    rabbitmq-user: &rabbitmq-user user
    rabbitmq-password: &rabbitmq-password <password>
    mongodb-host: &mongodb-host my-mongodb.bitnami.svc.cluster.local
    mongodb-user: &mongodb-user argo-platform
    mongodb-password: &mongodb-password <password>
    cache-host: &cache-host my-redis-master.bitnami.svc.cluster.local
    cache-password: &cache-password <password>
    pg-host-name: &pg-host-name my-postgresql.bitnami.svc.cluster.local
    pg-port: &pg-port 5432
    pg-db-name: &pg-db-name analytics
    pg-user-name: &pg-user-name postgres
    pg-password: &pg-password <password>

  analytics-reporter:
    env:
      MONGODB_PROTOCOL: *MONGODB_PROTOCOL
      RABBITMQ_PROTOCOL: *RABBITMQ_PROTOCOL
    secrets:
      mongodb-host: *mongodb-host
      mongodb-user: *mongodb-user
      mongodb-password: *mongodb-password
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
      pg-host-name: *pg-host-name
      pg-port: *pg-port
      pg-db-name: *pg-db-name
      pg-user-name: *pg-user-name
      pg-password: *pg-password
  api-events:
    env:
      RABBITMQ_PROTOCOL: *RABBITMQ_PROTOCOL
    secrets:
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
  api-graphql:
    env:
      MONGODB_PROTOCOL: *MONGODB_PROTOCOL
      RABBITMQ_PROTOCOL: *RABBITMQ_PROTOCOL
    secrets:
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
      mongodb-host: *mongodb-host
      mongodb-user: *mongodb-user
      mongodb-password: *mongodb-password
      cache-host: *cache-host
      cache-password: *cache-password
  cron-executor:
    env:
      MONGODB_PROTOCOL: *MONGODB_PROTOCOL
      RABBITMQ_PROTOCOL: *RABBITMQ_PROTOCOL
    secrets:
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
      mongodb-host: *mongodb-host
      mongodb-user: *mongodb-user
      mongodb-password: *mongodb-password
  event-handler:
    env:
      MONGODB_PROTOCOL: *MONGODB_PROTOCOL
      RABBITMQ_PROTOCOL: *RABBITMQ_PROTOCOL
    secrets:
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
      mongodb-host: *mongodb-host
      mongodb-user: *mongodb-user
      mongodb-password: *mongodb-password
  audit:
    env:
      MONGODB_PROTOCOL: *MONGODB_PROTOCOL
      RABBITMQ_PROTOCOL: *RABBITMQ_PROTOCOL
    secrets:
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
      mongodb-host: *mongodb-host
      mongodb-user: *mongodb-user
      mongodb-password: *mongodb-password
      
...
```
{::nomarkdown}
</br>
{:/}

##### Global: Mongo DB parameters

{: .table .table-bordered .table-hover}
| Section | Parameter       | Description            | Default value | 
|----------|----------------|------------------------|------------------|
|`global`| `mongodbRootUser`      | The user with privileges for seed jobs and automatic user creation.    | `root`              | 
|        | `mongodbRootPassword`  | The default Mongo DB password.                                         | ` `              | 
|        | `mongoURI`             | The default Mongo DB URI.                                              | ` `              | 


> `argo-platform.seedJobs`:`mongodbRootUser`, `mongodbRootPassword` and `mongodbUri`  must be identical to `global` values.

{::nomarkdown}
</br>
{:/}

##### Global: PostgreSQL parameters

{: .table .table-bordered .table-hover}
| Section | Parameter      | Description            | Default value | 
|----------|----------------|------------------------|------------------|
|`global`| `postgresSeedJob`           |Instantiate databases with seed data in on-premises environments.    |   N/A           | 
|        | `postgresSeedJob.user`      |The username for the database.                                       | `postgres`             | 
|        | `postgresSeedJob.password`  | The password for the database.                                      | ` `              | 
|        | `postgresUser`              | The default internal PostgreSQL username. The`secrets.pg-user-name` parameter must have the same value.    | `postgres` | 
|        | `postgresPassword`          | The default internal PostgreSQL password. The `secrets.pg-password` parameter must have the same value.    | ` `     | 
|        | `postgresHostname`          | The default external address of the PostgreSQL service. The `secrets.pg-host-name` parameter must have the same value.   | `codefresh`     | 
|        | `postgresPort`              | The default internal PostgreSQL port. The `secrets.pg-port` parameter must have the same value.            | ` `     | 

{::nomarkdown}
</br>
{:/}

##### Global: Redis parameters

{: .table .table-bordered .table-hover}
| Section | Parameter      | Description            | Required value | 
|----------|----------------|------------------------|------------------|
|`global`| `redisUrl`      |The default external address of the Redis service.       | `nil`        | 
|        | `redisPort`     | The default internal port used by the Redis service.    | `6379`       | 
|        | `redisPassword` | The default internal password for the Redis service.    | `postgres`   | 

{::nomarkdown}
</br>
{:/}

##### Global: RabbitMQ parameters

{: .table .table-bordered .table-hover}
| Section  | Parameter      | Description            | Required value | 
|----------|----------------|------------------------|------------------|
|`global`  | `rabbitmqHostname` |The default external address of the RabbitMQ service.  | `nil`        | 
|          | `rabbitmqUsername` |The default RabbitMQ username.                         | `user`       | 
|          | `rabbitmqPassword` |The default RabbitMQ password.                         | ` `   | 

{::nomarkdown}
</br>
{:/}

##### argo-platform.env: MongoDB parameters

{: .table .table-bordered .table-hover}
| Section | Parameter      | Description            | Required value | 
|-------------|----------------|------------------------|------------------|
`argo-platform.env` |`MONGODB_PROTOCOL`| The protocol to connect to MongoDB. Can be either `mongodb` or  `mongodb+srv`      | `mongodb`        | 
|                   | `MONGODB_PROTOCOL` |The default MongoDB username.    | `user`       | 



<!---### Activate Codefresh
After installing and deploying Codefresh on-premises, in Codefresh Classic you must enable Codefresh feature flags for the account and switch to the Codefresh UI.

1. Log in ?????
1. Select **Admin Management > Accounts > Add feature to account**.
1. Enable the following features:
  * `environmentsV2Flag`
  * `codefreshV2`
  * `codefreshV2NonAdmins`
  * `usersManagementV2`
  * `showClassicCodefreshButton`

  {% include
image.html
lightbox="true"
file="/images/runtime/on-prem-install-feature-flags.png"
url="/images/runtime/on-prem-install-feature-flags.png"
alt="Enable feature flags for Codefresh activation"
caption="Enable feature flags for Codefresh activation"
max-width="80%"
%}

1. Click **Switch to Codefresh**.
  The new --->



### What to do next
Provision a hybrid runtime (TBD)
