---
title: "Webhook Relay"
description: ""
group: runtime
toc: true
---

Webhook Relay is a payload delivery service for webhooks that receives webhook payloads and sends them to listening clients. The clients then forward the webhook payloads to the specified target URLs. The clients must subscribe to a specific channel on the server which has the same name as that of the CSDP runtime. 


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

**K8s Cluster**  
The K8s cluster in the DMZ that is exposed to the webhooks from Git providers, GitHub in our case.

**Webhook Relay Server**  
The server component that receives all the webhooks from external systems, again, GitHub in our case. The server forwards the webhook event requests to the Webhook Relay Clients which are subscribed to receive the requests. To run the service in high-availability mode, Redis is required.  

**Ingress**  
The ingress controller on the K8s cluster in the DMZ, acting as proxy to receive the traffic (webhook requests) and route them to the Webhook Relay Server.  The webhook URL points to this ingress service.  

**CSDP runtime**  
The CSDP runtime or runtimes on which the Webhook Relay Clients are installed. Communication is allowed only from the runtime to the DMZ.

**Webhook Relay Client**  
The client component that receives the requests from the Webhook Relay Server or Servers it is subscribed to. Webhook Relay Clients must be installed on every Codefresh runtime in your deployment. 

### Customer Requirements
 
**Installation requirements:**  

* Webhook Relay Server in the DMZ
* Webhook Relay Clients in the CSDP runtime clusters 
* Redis to enable high-availability for Webhook Relay Server  
  To run multiple instances of the Webhook Relay Server, you must share events across those instances. A client may be connected to instance A, so if a relevant event is sent to instance B, instance A needs to know about it too. Webhook Relay Server has a built-in support for Redis as a message bus.  
* Ingress host to act as the proxy for the Webhook Relay Server

**Configuration requirements:**  

* IP whitelist  
  Because the channels are _not authenticated_, we highly recommend creating whitelists with the _IP ranges of your runtime clusters and of your Git provider_.  
  The runtime cluster whitelist is required to access the  `/subscribe/:channel/` endpoint (internal).  
  The Git provider whitelist is required to access the `/webhooks/:channel/*` endpoint (external).  

* Webhook URL  
  When creating the webhook in your Git provider, the webhook URL must be configured in the following format:  
    `https://${url-of-the-webhook-relay-server}/webhooks/:${channel}/*`  
  Every payload sent to the `/webhooks/:channel/*` endpoint is published immediately to all clients listening to the same channel, on the `/subscribe/:channel/` endpoint.  

  The clients forward the payloads to the URL defined by the `TARGET_BASE_URL` environment variable while keeping the original url path, for instance `https://base-url-of-your-runtime-cluster/webhooks/:channel/push-github/github-push-heads`. (NOT CLEAR what this is?)


### How Webhook Relay works
1. The Webhook Relay server, `webhook-relay-server`, receives the webhook via the ingress source.
1. The Webhook Client or Clients, `webhook-relay-client`, that are subscribed to the Webhook Relay Server, listen for messages via `Server-sent events`, and passes the webhook to the required `eventSource` service.  
> [`Server-sent events`](https://html.spec.whatwg.org/multipage/server-sent-events.html){:target="\_blank"}, is a type of connection that allows messages to be sent from a source to any listening client.


### Deploy the Webhook Relay in CSDP 
To deploy the Webhook Relay in CSDP, apply the server and client manifests:
* Server manifest to the Webhook Relay Server in the DMZ
  The `REDIS_URL` environment variable is mandatory, as it tells the server to use Redis when receiving payloads, and to publish them to all the instances of the server, if there are multiple instances. Apart from `REDIS_URL`, you can pass additional environment variables to the Webhook Relay Server. To see all the options, [click here](https://github.com/codefresh-io/webhook-relay/blob/main/apps/webhook-relay-server/README.md){:target="\_blank"}.

* Client manifest to the Webhook Relay Client in the CSDP Runtime cluster  
  Apart from the `SOURCE_URL` and `TARGET_BASE_URL` environment variables that are mandatory, you can pass additional environment variables to the Webhook Relay Client.  
  The `SOURCE_URL` defines the channel on the Webhook Relay Server that the client subscribes to, identical to the runtime name, in the format:  
  `https://${url-of-the-webhook-relay-server}/subscribe/:${channel}/`  
  To see all the options, [click here](https://github.com/codefresh-io/webhook-relay/blob/main/apps/webhook-relay-client/README.md){:target="\_blank"}.



#### Apply server manifest
Apply the Webhook Server Relay manifest on the Webhook Relay Server in the DMZ:  

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
          env:
            - name: REDIS_URL
              # You can specify your connection as a redis:// URL or rediss:// URL when using TLS encryption.
              # Username and password can also be passed via URL redis://username:authpassword@127.0.0.1:6380/4.
              value: redis://redis.my-service.com
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
#### Apply client manifest
Apply the client manifests to the runtime clusters in CSDP:

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
  Channels are always active. Once a client is connected, the server sends any payloads it gets at `/webhooks/:channel/*` to those clients.

**Are payloads stored anywhere?**  
Webhook payloads are _never_ stored on the server or in any database; the server is simply a pass-through.

**What are the best practices to employ in production?**  

* Use IP whitelists for your runtime clusters to access `/subscribe/:channel/` endpoint (internal), and for your git provider to access `/webhooks/:channel/*` endpoint (external).
* It is recommended to run multiple replicas of the server together with Redis using the `REDIS_URL` environment variable.
* `Server-sent events` connections are HTTP long-running (keep-alive) connections. We recommend that your reverse-proxy server is configured to avoid situations where a long-running connection is cut off by the reverse-proxy. For example, [Nginx](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive).
