---
title: "Install Codefresh on-premises"
description: ""
group: runtime
toc: true
---

Codefresh fully supports on-premises installations. Our proprietary Kubernetes Codefresh Installer, `kcfi`, aggregates all installation components into a single tool, helping you to install and deploy the on-premises version of the Codefresh platform in your environment.  

<!---Codefresh on-premises installation can be divided into the following:

**Preparing for installation**  
This stage includes the tasks before you start installation, such as verifying system requirements and prerequisites for installation.
* Take survey to inform Codefresh about your specifications
* 
  Before you start Verify that your installation environment matches the system requirements, and you have all the files, certificates before you start the installatio.

**Installing Codefresh on-premises**
This stage includes the installation, configuration, and deployment.

** Activate Codefresh**
Activate the 
  After installation and deployment, activate Codefresh by turning on feature flgas.--->

### Take the Codefresh survey

_Before_ starting the installation, complete the survey for Codefresh confirm that your environment is ready for on-premises installation and deployment.  

[Survey](https://docs.google.com/forms/d/e/1FAIpQLSf18sfG4bEQuwMT7p11F6q70JzWgHEgoAfSFlQuTnno5Rw3GQ/viewform){:target="\_blank"}

### System requirements for on-premises installations
The table describes the minimum requirements for an on-premises installation.

{: .table .table-bordered .table-hover}
| Item                     | Requirement            |  
| --------------         | --------------           |  
|Kubernetes cluster      | Server version 1.19 to 1.22  {::nomarkdown}<br><b>Tip</b>:  To check the server version, run:<br> <span style="font-family: var(--font-family-monospace); font-size: 87.5%; color: #ad6800; background-color: #fffbe6">kubectl version --short</span>.{:/}|
|Operating system|{::nomarkdown} <ul><li>Windows 10/7</li><li>OSX</li>{:/}|
|Node requirements TBD| {::nomarkdown}<ul><li>Memory: 5000 MB</li><li>CPU: 2</li></ul>{:/}|
|Git providers TBD    |{::nomarkdown}<ul><li>GitHub</li></ul>{:/}|


### Prerequisties for on-premises installation
Make sure you have the following:

#### Service Account file
The Service Account file is provided by Codefresh ????.  
(NIMA: why and what is this used for? where should users save this?)

#### TLS certificates
For a secured installation, we highly recommend using TLS certificates. 
Make sure you have your `ssl.cert` and `private.key` are valid.

>Use a Corporate Signed certificate, or any valid TLS certificate, for example, from lets-encrypt.

#### Storage for persistent services
Codefresh uses both cluster storage (volumes), as well as external storage. Make sure you meet the minimum capacity for the volumes.

##### Databases
The table below displays the list of databases created as part of the installation:

| Database | Purpose | Latest supported version |
|----------|---------| ---------------|
| mongoDB | Store all account data (account settings, users, projects, pipelines, builds etc.) | 4.2.x |
| postgresql | Store data about events in the account (pipeline updates, deletes, etc.). The audit log uses the data from this database. | 13.x |
| redis | Mainly for caching, and as a key-value store for our trigger manager. | 6.0.x |

##### Volumes

The table below lists the volumes required by Codefresh on-premises.


{: .table .table-bordered .table-hover}
| Name           | Purpose                | Minimum Capacity | Can run on netfs (nfs, cifs) |
|----------------|------------------------|------------------|------------------------------|
| cf-mongodb*    | Main database - Mongo  | 8GB              | Yes**                        |
| cf-postgresql* | Events databases - Postgres | 8GB         | Yes**                        |
| cf-rabbitmq*   | Message broker         | 8GB              | No**                         |
| cf-redis*      | Cache                  | 8GB              | No**                         |

{% raw %}

 (*) Possibility to use external service

 (**) Running on netfs (nfs, cifs) is not recommended by product admin guide

{% endraw %}

(NIMA: need to rewrite this)
The default initial volume size (100 Gi) can be overridden in the custom `config.yaml` file. Values descriptions are in the `config.yaml` file.
The registry’s initial volume size is 100Gi. It also can be overridden in a custom `config.yaml` file. There is a possibility to use a customer-defined registry configuration file (`config.yaml`) that allows using different registry storage back-ends (S3, Azure Blob, GCS, etc.) and other parameters. More details can be found in the [Docker documentation](https://docs.docker.com/registry/configuration/).

Depending on the customer’s Kubernetes version we can assist with PV resizing. Details are can be found in this [Kubernetes blog post](https://kubernetes.io/blog/2018/07/12/resizing-persistent-volumes-using-kubernetes/).

##### Automatic Volume Provisioning
Codefresh installation supports automatic storage provisioning based on the standard Kubernetes dynamic provisioner Storage Classes and Persistent Volume Claims. All required installation volumes are provisioned automatically using the default Storage Class or custom Storage Class that can be specified as a parameter in `config.yaml` under `storageClass: my-storage-class`.

### Install Codefresh on-premises
Follow the steps to install and deploy Codefresh on-premises.

#### Before you begin
Make sure you:
* Meet the system requirements
* Have completed the prerequisites


#### Step 1: Download and install kcfi

* Download the `kcfi` binary from [GitHub](https://github.com/codefresh-io/kcfi/releases){:target="\_blank"}.

#### Step 2: Set the current Kube context
Define the correct context in `kubeconfig`. This context is used for all runtimes that you will later install in your account.

> To display list of contexts, run `kubectl config get-contexts`.

* Set the context:  
  `kubectl config use-context <my-cluster-name>`  
  where:   
  `<my-cluster-name>` is the context for the cluster.
* Verify the current context:  
  `kubectl config current-context`
  

#### Step 3: Initialize the installation directory
Run the init command to create the installation directory with the `config.yaml` file, and other files and directories required for on-premises installation. The `config.yaml` has the installation settings that you can customize for your on-premises environment.


* Run:  
  `kcfi init codefresh [-d /path/to/stage-dir]`

#### Step 4: Configure config.yaml
Configure the installation settings in the `config.yaml`.

1. In the `installer` section, define Helm as the installation method:

```yaml
  installer:
    # type:
    #   "operator" - apply codefresh crd definition
    "helm" - install/upgrade helm chart from client 
```

{:start="2"}
1. Add tags to enable Argo Platform, and disable Helm charts for Codefresh Classic:

```yaml
metadata:
 ...

tags:
  cf-infra: false
  argo-platform: true

global:
  ...
```

{:start="3"}
1. Copy Codefresh images to your private container registry:
  (NIMA: is this needed?)
  If you install Codefresh in an air-gapped environment (without access to public Docker Hub or codefresh-enterprise registry), copy the images to your organization's private  container registry. Kubernetes pulls the images from the private registry as part of the installation.
  * Below `images`, set `usePrivateRegistry` to `true`
  * Define the `address`, `username`, and `password` for the private registry. 

  ```yaml
  images:
  codefreshRegistrySa: sa.json (NIMA: should we say to comment this out?)
    usePrivateRegistry: true
    privateRegistry:
      address: <private registry address>
      username: <private registry username>
      password: <private registry password>
  lists:
  - images/images-list
  ```

  * To push single or multiple images, do one of the following:

    Single image:
    
    ```yaml
    kcfi images push [-c|--config /path/to/config.yaml] [options] repo/image:tag [repo/image:tag]
    ```

    Multiple images:

    ```yaml
    kcfi images push  [-c|--config <path-to-config.yaml>]
    ```
(NIMA: is this relevant?) Even if you are running a Kubernetes cluster that has outgoing access to the public Internet, note that Codefresh platform images are not public and can be obtained by using sa.json file provided by Codefresh support personnel.

Use the flag --codefresh-registry-secret to pass the path to the file sa.json.

#### Step 6: Enable TLS certificates
If using TLS certificates, do the following:

* Save both `ssl.cert` and `private.key` in the `certs/` directory.
* Set `tls.selfSigned` to `false`.

#### Step 5: Configure external services for database/messaging/caching
By default, Codefresh  uses internal services such as `cf-mongodb`, `cf-redis`, `cf-rabbitmq`, `cf-postgresql` to run the Argo components. 
To use your own existing services for data storage/messaging/caching, configure them in `config.yaml`. 
* In the `global`section, specify the connection URI for required services:
  MongoDB
  RabbitMQ
  Redis
  Postgresql

> YAML anchors are used to populate connection URIs to env variables for the corresponding argo-platform services.

Here is an example of the `config.yaml` with all the external services:

```yaml
metadata:
  kind: codefresh
  installer:
    type: helm
    helm:
      chart: codefresh
      repoUrl: https://chartmuseum-dev.codefresh.io/codefresh
      version: 1.2.15-onprem-argo-platform

global:
  appUrl: onprem.mydomain.com
  appProtocol: https

  postgresSeedJob:
    user: postgres
    password: 7Aql96FEMI
  postgresUser: cf_user
  postgresPassword: fJTFJMGV7sg5E4Bj
  postgresDatabase: codefresh
  postgresHostname: my-postgresql.bitnami.svc.cluster.local
  postgresPort: 5432

  redisUrl: my-redis-master.bitnami.svc.cluster.local
  redisPort: 6379
  redisPassword: I88I01784Y
  
  runtimeRedisHost: my-redis-master.bitnami.svc.cluster.local
  runtimeRedisPassword: I88I01784Y
  runtimeRedisPort: 6379
  runtimeRedisDb: 2

  rabbitmqHostname: my-rabbitmq.bitnami.svc.cluster.local
  rabbitmqUsername: user
  rabbitmqPassword: WBkKrFtyb8K5600y

  mongodbRootUser: root # privileged user will be used for seed jobs and for automatic user creation
  mongodbRootPassword: bioFPLaDN8 
  
  mongoURI: mongodb://cfuser:mTiXcU2wafr9@my-mongodb.bitnami.svc.cluster.local
  runtimeMongoURI: mongodb://cfuser:mTiXcU2wafr9@my-mongodb.bitnami.svc.cluster.local # in case OfflineLogging feature is enabled (i.e. you have no Firebase)
  argoPlatformMongoUri: mongodb://argo-platform:oDpLLqEoPM@my-mongodb.bitnami.svc.cluster.local/read-models

argo-platform:
  enabled: true

  ingress:
    enabled: true

  secrets:
    rabbitmq-host: &rabbitmq-host my-rabbitmq.bitnami.svc.cluster.local
    rabbitmq-port: &rabbitmq-port 5672
    rabbitmq-user: &rabbitmq-user user
    rabbitmq-password: &rabbitmq-password cUzWIAVYlB
    mongodb-host: &mongodb-host my-mongodb.bitnami.svc.cluster.local
    mongodb-user: &mongodb-user argo-platform
    mongodb-password: &mongodb-password oDpLLqEoPM
    cache-host: &cache-host my-redis-master.bitnami.svc.cluster.local
    cache-password: &cache-password t90cppjJvH
    pg-host-name: &pg-host-name my-postgresql.bitnami.svc.cluster.local
    pg-port: &pg-port 5432
    pg-db-name: &pg-db-name analytics
    pg-user-name: &pg-user-name postgres
    pg-password: &pg-password 7Aql96FEMI

  api-graphql:
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
  api-events:
    secrets:
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
  event-handler:
    secrets:
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
      mongodb-host: *mongodb-host
      mongodb-user: *mongodb-user
      mongodb-password: *mongodb-password
  cron-executor:
    secrets:
      rabbitmq-host: *rabbitmq-host
      rabbitmq-port: *rabbitmq-port
      rabbitmq-user: *rabbitmq-user
      rabbitmq-password: *rabbitmq-password
      mongodb-host: *mongodb-host
      mongodb-user: *mongodb-user
      mongodb-password: *mongodb-password
  analytics-reporter:
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
      
...
```

#### Step 6: (Optional) Configure Network Load Balancer
To use an AWS Network Load Balancer, add the following `annotations` to `ingress-nginx.controller.service`:

```yaml
...

ingress-nginx:
  controller:
    service:
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-type: nlb
        service.beta.kubernetes.io/aws-load-balancer-backend-protocol: tcp
        service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: '60'
        service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled: 'true'

...
```

#### Step 7: Deploy Codefresh on-premises 
Deploy the Codefresh on-premises platform.

* Run:  
  `kcfi deploy -c config.yaml --debug --wait --timeout 10m`


### Activate Codefresh
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
  The new 



### What to do next
Provision a runtime - TBD
