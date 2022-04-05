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
The server component that receives all the webhooks from external systems, again, GitHub in our case. The Webhook Relay Serve forwards the webhook event requests to the Webhook Relay Client which is subscribed to receive the requests. For high-availability, Redis is required.  

**Webhook Relay Client**  
The client component that receives the requests from the Webhook Relay Server it is subscribed to. A Webhook Relay Client must be installed on every Codefresh runtime in your deployment.  


### Prerequisites

* Two clusters, one public, and one private. The public cluster must be accessible from the internet.  
* CSDP runtime
* Two ingress controllers:
  * External ingress controller in the public cluster
  * Internal ingress controller in the private cluster with the runtime and the Webhook Relay Client. The internal ingress controller is used for internal communication within the cluster.


### Configuration

* Webhook URL  
  Argo Events creates the webhook in your Git provider. 
  In the `EventSource`, the `webhook` is created from the `url` and the `endpoint` attributes:   
    `endpoint` is the channel on the Server that the Client listens to.  
    `url` is the base URL, identical to the value defined for the `TARGET_BASE_URL` environment variable.  

  The webhook URL has the following format:  
    `${TARGET_BASE_URL}/webhooks/${channel}/*`  

  Each payload sent to the `/webhooks/${channel}/*` endpoint is published immediately to all clients listening to the same channel on the `/subscribe/${channel}/` endpoint.  

* Client environment variables  

  The `TARGET_BASE_URL` and the `SOURCE_URL` environment variables are mandatory. 
  * `TARGET_BASE_URL`  
    The base URL of the endpoint that defines the Event Source.  
    The Client forwards the payload to the URL defined by `TARGET_BASE_URL`, retaining the full URL path.  
    For example:  
    `${TARGET_BASE_URL}/webhooks/${channel}/push-github/github-push-heads` 

  * `SOURCE_URL`: The specific channel on the Server that the client subscribes to, identical to the runtime name, in the format:  
    `${TARGET_BASE_URL}/subscribe/${channel}/`  

* (Optional) IP whitelist for runtime clusters  
  Because the channels are _not authenticated_, we highly recommend creating a whitelist with the _IP ranges of your runtime clusters_.  
  The runtime cluster whitelist is required to access the `/subscribe/${channel}/` endpoint.   

  > Tip: If you choose, you can also create a whitelist for the Git provider to access the `/webhooks/${channel}/*` endpoint.  


### How Webhook Relay works
1. The Server receives the webhook from the Git provider.
1. The Client subscribed to the Server listens for messages via `Server-sent events`, and passes the webhook to the internal ingress controller.  
> [`Server-sent events`](https://html.spec.whatwg.org/multipage/server-sent-events.html){:target="\_blank"} connections are long-running HTTP (keep-alive) connections.

#### High-availability (HA)
 To run multiple instances of the Webhook Relay Server, you must share events across those instances. All Server instances are then aware of the relevant events their Clients are subscribed to, even when the Client is connected only to a specific Server instance. The Server has built-in support for Redis as a message bus.  
 > To enable high-availability, pass `REDIS_URL` as an environment variable.


### Deploy the Webhook Relay in CSDP 
To deploy the Webhook Relay in CSDP, apply the Server and Client manifests:

#### Server manifest

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

* Use IP whitelists for your runtime clusters to access `/subscribe/${channel}/` endpoint (internal), and for your git provider to access `/webhooks/${channel}/*` endpoint (external).
* It is recommended to run multiple replicas of the server together with Redis using the `REDIS_URL` environment variable.
* `Server-sent events` connections are long-running HTTP (keep-alive) connections. We recommend that your reverse-proxy server is configured to avoid situations where a long-running connection is cut off by the reverse-proxy. For example, [Nginx](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive).
