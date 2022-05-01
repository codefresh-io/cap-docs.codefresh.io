---
title: "Webhook Relay"
description: ""
group: runtime
toc: true
---

Webhook Relay is a payload delivery service for webhooks that receives webhook payloads and sends them to listening clients. The clients then forward the webhook payloads to the specified target URLs. Webhook Relay provides a way to receive webhooks without having to expose internal Kubernetes clusters to the internet.


### Architecture
The diagrams illustrate the architecture of the Webhook Relay in the context of CSDP, without and with Redis.

  {% include 
	image.html 
	lightbox="true" 
	file="/images/webhook-relay/webhook-relay-main.png" 
	url="/images/webhook-relay/webhook-relay-main.png" 
	alt="Webhook Relay architecture without Redis" 
	caption="Webhook Relay architecture without Redis"
    max-width="30%" 
%}


  {% include 
	image.html 
	lightbox="true" 
	file="/images/webhook-relay/webhook-relay-with-redis.png" 
	url="/images/webhook-relay/webhook-relay-with-redis.png" 
	alt="Webhook Relay architecture with Redis" 
	caption="Webhook Relay architecture with Redis"
    max-width="30%" 
%}

Here's a description of the components.  


**Webhook Relay Server**  
The Server component that receives all the webhooks from external systems, GitHub in our case. The Server forwards the webhook event requests to the Webhook Relay Client which is subscribed to receive the requests. For high-availability, Redis is required.  

**Webhook Relay Client**  
The Client component that receives the requests from the Server it is subscribed to. The Client must be installed on every internal cluster with a CSDP runtime.  


### Prerequisites
  
* External cluster with ingress controller  
  The external cluster is a cluster with an ingress controller whose ingress host is accessible from the internet. 
* One or more internal clusters, also with ingress controllers  
  The internal cluster is a cluster with an ingress controller whose ingress host is accessible internally within the cluster, with the CSDP runtime installed. 
* CSDP runtime in the internal cluster, or on all internal clusters if there is more than one

  

### Configuration
The Client subscribes to a specific channel on the Server to receive the webhook payloads. The name of the channel should be identical to the name of the CSDP runtime installed in the same cluster as the Client.
Every payload sent to the `/webhooks/${channel}/*` endpoint is published immediately to all Clients listening to the same channel on the `/subscribe/${channel}/` endpoint.   

* Client environment variables  

  The `TARGET_BASE_URL` and the `SOURCE_URL` environment variables are mandatory. 
  * `TARGET_BASE_URL`  
    The base URL to which the Client forwards the webhook payloads while keeping the original URL path, in the format:   
      `https://${internal-cluster-ingress-host}`  

    For example, if the `endpoint` attribute in the `EventSource` manifest is set to `/webhooks/runtime-1/push-github/`, then the Client would forward all the payloads to this URL:    
    `${TARGET_BASE_URL}/webhooks/runtime-1/push-github/` 

  * `SOURCE_URL`  
    The URL to which the Client should subscribe.
    The URL should include the channel (runtime name) on the Server in the following format:  
      `https://${external-cluster-ingress-host}/subscribe/${channel}/`
  
* Webhook URL in the `EventSource` manifest  

  Argo Events creates the webhook in your Git provider, using the [webhook configuration of the EventSource](https://github.com/argoproj/argo-events/blob/master/api/event-source.md#webhookcontext){:target="\_blank"}, with the `endpoint` and `url` attributes:  

  `url` must be configured in the format: `https://${external-cluster-ingress-host}`  
  `endpoint` must be configured in the format: `/webhooks/${channel}/*`, for example, `/webhooks/runtime-1/push-github/)`  
    > Note: If you used the [Create Delivery Pipeline wizard]({{site.baseurl}}/docs/pipelines/create-pipeline), the `endpoint` is automatically configured in the correct format. 

* (Optional) IP whitelist for runtime clusters  
  As the channels are _not authenticated_, we highly recommend creating a whitelist with the _IP ranges of your runtime clusters_.  
  The whitelist ensures that only your internal clusters can access the `/subscribe/${channel}/` endpoint, while blocking access to other parties.   

  > Tip: If you choose to, you can create a whitelist also for the Git provider to access the `/webhooks/${channel}/*` endpoint.  

### How Webhook Relay works
1. The Server receives the webhook from the Git provider.
1. The Client subscribed to the Server listens for messages via `Server-sent events`, and passes the webhook to the internal ingress controller.  
> [`Server-sent events`](https://html.spec.whatwg.org/multipage/server-sent-events.html){:target="\_blank"} connections are long-running HTTP (keep-alive) connections.

#### Running multiple instances of Webhook Relay Server
 If you need to run multiple instances of the Server, you need a way to share events across those instances. A Client may be connected to instance A, so if a relevant event is sent to instance B, instance A needs to know about it too.  
 For that reason, the Server has a built-in support for Redis as a message bus. To enable it, pass the `REDIS_URL` environment variable to the Server. That will tell the Server to use Redis when receiving payloads, and to publish them to all the instances of the Server.
​

### Deploy the Webhook Relay
To deploy the Webhook Relay, apply the Server and Client manifests.
> To view the latest image versions of the Server and the Client, [click here](https://github.com/codefresh-io/webhook-relay/releases){:target="\_blank"}.

#### Server manifest
Apply the Server manifest to the external cluster.  

In addition, you will need to create an Ingress for the Server service to reach the `/webhooks/${channel}/*` endpoint from your Git provider, and the `/subscribe/${channel}/` endpoint from your internal runtime clusters.

> To see all environment variables you can configure for the Server, [click here](https://github.com/codefresh-io/webhook-relay/blob/main/apps/webhook-relay-server/README.md){:target="\_blank"}.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webhook-relay-server-svc
spec:
  ports:
    - name: web
      port: 3000
      targetPort: 3000
  selector:
    app: webhook-relay-server
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webhook-relay-server
spec:
  selector:
    matchLabels:
      app: webhook-relay-server
  # It is recommended to run multiple replicas of the Server together
  # with Redis using the REDIS_URL environment variable
  replicas: 1
  template:
    metadata:
      labels:
        app: webhook-relay-server
    spec:
      containers:
        - name: webhook-relay-server
          # To view the latest image versions, visit here: https://github.com/codefresh-io/webhook-relay/releases
          image: quay.io/codefresh/webhook-relay-server:${version-tag}
          ports:
            - containerPort: 3000
#          env:
#            - name: REDIS_URL
#              # You can specify your connection as a redis:// URL or rediss:// URL when using TLS encryption.
#              # Username and password can also be passed via URL redis://username:authpassword@127.0.0.1:6380/4.
#              value: redis://redis.my-service.com
          readinessProbe:
            httpGet:
              path: /ready
              port: 9000
            failureThreshold: 1
            initialDelaySeconds: 5
            periodSeconds: 5
            successThreshold: 1
            timeoutSeconds: 5
          livenessProbe:
            httpGet:
              path: /live
              port: 9000
            failureThreshold: 3
            initialDelaySeconds: 10
            # Allow sufficient amount of time (90 seconds = periodSeconds * failureThreshold)
            # for the registered shutdown handlers to run to completion.
            periodSeconds: 30
            successThreshold: 1
            # Setting a very low timeout value (e.g. 1 second) can cause false-positive
            # checks and service interruption.
            timeoutSeconds: 5

```

#### Client manifest

Apply the Client manifest to each of your private clusters with the CSDP runtimes.

> To see all environment variables you can configure for the Client, [click here](https://github.com/codefresh-io/webhook-relay/blob/main/apps/webhook-relay-client/README.md){:target="\_blank"}.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webhook-relay-client
spec:
  selector:
    matchLabels:
      app: webhook-relay-client
  replicas: 1
  template:
    metadata:
      labels:
        app: webhook-relay-client
    spec:
      containers:
        - name: webhook-relay-client
          # To view the latest image versions, visit here: https://github.com/codefresh-io/webhook-relay/releases
          image: quay.io/codefresh/webhook-relay-client:${version-tag}
          env:
            - name: SOURCE_URL
              # Channel name should equal the runtime name
              value: https://${public-cluster-ingress-host}/subscribe/${channel}
            - name: TARGET_BASE_URL
              # All payloads will be sent to TARGET_BASE_URL/webhooks/${channel}/*
              value: https://${private-cluster-ingress-host}
```

### FAQs

**What is the TTL for channels?**  
Channels are always active. Once a Client is connected, the Server sends any payloads it gets at `/webhooks/${channel}/*` to those clients.

**Are payloads stored anywhere?**  
Webhook payloads are _never_ stored on the Server or in any database; the Server is simply a pass-through.

**What are the best practices for production use?**  

* Use IP whitelists for your runtime clusters to access `/subscribe/${channel}/` endpoint, and for your git provider to access `/webhooks/${channel}/*` endpoint.
* It is recommended to run multiple replicas of the Server together with Redis using the `REDIS_URL` environment variable.
* `Server-sent events` connections are long-running HTTP (keep-alive) connections. We recommend that your reverse-proxy server is configured correctly to avoid situations where a long-running connection is cut off by the reverse-proxy. For example, [Nginx](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive).
