---
title: "Webhook Relay"
description: ""
group: runtime
toc: true
---

Webhook Relay is a payload delivery service for webhooks that receives webhook payloads and sends them to listening clients. The clients then forward the webhook payloads to the specified target URLs. Webhook Relay provides a way to receive webhooks without having to expose internal Kubernetes clusters to the internet.


### Architecture
The diagram illustrates the architecture of the Webhook Relay in the context of CSDP.

  {% include 
	image.html 
	lightbox="true" 
	file="/images/webhook-relay/webhook-relay-arch.png" 
	url="/images/webhook-relay/webhook-relay-arch.png" 
	alt="Webhook Relay architecture" 
	caption="Webhook Relay architecture"
    max-width="30%" 
%}
Here's a description of the components.  


**Webhook Relay Server**  
The Server component that receives all the webhooks from external systems, GitHub in our case. The Server forwards the webhook event requests to the Webhook Relay Client which is subscribed to receive the requests. For high-availability, Redis is required.  

**Webhook Relay Client**  
The Client component that receives the requests from the Server it is subscribed to. The Client must be installed on every private cluster with a CSDP runtime.  


### Prerequisites
  
* Public cluster with ingress controller  
  The public cluster is a cluster with an ingress controller whose ingress host is accessible from the internet. 
* One or more private clusters, also with ingress controllers  
  The private cluster is a cluster with an ingress controller whose ingress host is accessible internally within the cluster, with the CSDP runtime installed. 
* CSDP runtime in the private cluster, or on all private clusters if there is more than one

  

### Configuration

* Client environment variables  

  The `TARGET_BASE_URL` and the `SOURCE_URL` environment variables are mandatory. 
  * `TARGET_BASE_URL`  
    The base URL to which the Client forwards the webhook payloads while keeping the original URL path, in the format:   
      `https://${private-cluster-ingress-host}`  

    For example, if the `endpoint` attribute in the `EventSource` manifest is set to `/webhooks/${channel}/push-github/`, then the Client would forward all the payloads to this URL:    
    `${TARGET_BASE_URL}/webhooks/${channel}/push-github/` 

  * `SOURCE_URL`  
    The URL to which the Client should subscribe.
    The URL should include the channel on the Server in the following format:  
      `https://${public-cluster-ingress-host}/subscribe/${channel}/`
  
* Webhook URL in the `EventSource` manifest  
  The Client subscribes to a specific channel on the Server to receive the webhook payloads. The name of the channel should be identical to the name of the CSDP runtime installed in the same cluster as the Client.
  Every payload sent to the `/webhooks/${channel}/*` endpoint is published immediately to all Clients listening to the same channel on the `/subscribe/${channel}/` endpoint.   

  Argo Events creates the webhook in your Git provider, using the [`webhook` configuration](https://github.com/argoproj/argo-events/blob/master/api/event-source.md#webhookcontext){:target="\_blank"}, with the `endpoint` and `url` attributes:  

  `url` must be configured in the format: `https://${public-cluster-ingress-host}`  
  `endpoint` must be configured in the format: `/webhooks/${channel}/*`   

* (Optional) IP whitelist for runtime clusters  
  As the channels are _not authenticated_, we highly recommend creating a whitelist with the _IP ranges of your runtime clusters_.  
  The whitelist ensures that only your private clusters can access the `/subscribe/${channel}/` endpoint, while blocking access to other parties.   

  > Tip: If you choose, you can create a whitelist also for the Git provider to access the `/webhooks/${channel}/*` endpoint.  

### How Webhook Relay works
1. The Server receives the webhook from the Git provider.
1. The Client subscribed to the Server listens for messages via `Server-sent events`, and passes the webhook to the internal ingress controller.  
> [`Server-sent events`](https://html.spec.whatwg.org/multipage/server-sent-events.html){:target="\_blank"} connections are long-running HTTP (keep-alive) connections.

#### Running multiple instances of Webhook Relay Server
 If you need to run multiple instances of the Server, you need a way to share events across those instances. A Client may be connected to instance A, so if a relevant event is sent to instance B, instance A needs to know about it too.  
 For that reason, the Server has a built-in support for Redis as a message bus. To enable it, pass the `REDIS_URL` environment variable to the server. That will tell the server to use Redis when receiving payloads, and to publish them to all the instances of the Server.
​

### Deploy the Webhook Relay
To deploy the Webhook Relay, apply the Server and Client manifests.
> To view the latest image versions of the Server and the Client, [click here](https://github.com/codefresh-io/webhook-relay/releases){:target="\_blank"}.

#### Server manifest
Apply the Server manifest to the public cluster.  

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
  replicas: 3
  template:
    metadata:
      labels:
        app: webhook-relay-server
    spec:
      containers:
        - name: webhook-relay-server
          image: quay.io/codefresh/webhook-relay-server:stable
          ports:
            - containerPort: 3000
          # env:
          #   - name: REDIS_URL
          #     # You can specify your connection as a redis:// URL or rediss:// URL when using TLS encryption.
          #     # Username and password can also be passed via URL redis://username:authpassword@127.0.0.1:6380/4.
          #     value: redis://redis.my-service.com
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
          image: quay.io/codefresh/webhook-relay-client:stable
          env:
            - name: SOURCE_URL
              # Channel name should equal the runtime name
              value: https://url-of-the-webhook-relay-server/subscribe/:channel
            - name: TARGET_BASE_URL
              # All payloads will be sent to TARGET_BASE_URL/webhooks/:channel/*
              value: https://base-url-of-your-runtime-cluster

```

### FAQs

**What is the TTL for channels?**  
Channels are always active. Once a client is connected, the server sends any payloads it gets at `/webhooks/${channel}/*` to those clients.

**Are payloads stored anywhere?**  
Webhook payloads are _never_ stored on the server or in any database; the server is simply a pass-through.

**What are the best practices for production use?**  

* Use IP whitelists for your runtime clusters to access `/subscribe/${channel}/` endpoint, and for your git provider to access `/webhooks/${channel}/*` endpoint.
* It is recommended to run multiple replicas of the server together with Redis using the `REDIS_URL` environment variable.
* `Server-sent events` connections are long-running HTTP (keep-alive) connections. We recommend that your reverse-proxy server is configured correctly to avoid situations where a long-running connection is cut off by the reverse-proxy. For example, [Nginx](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive).
