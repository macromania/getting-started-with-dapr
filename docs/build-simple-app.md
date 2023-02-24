# Building A Simple Dapr Application

In this guide, you’ll simulate an application by running the sidecar and calling the API directly. After running Dapr using the Dapr CLI, you’ll:

- Save a state object.
- Read/get the state object.
- Delete the state object.

## Pre-requisites

> This guide assumes, you have completed the environment setup by following [README.md](../README.md)

In order to run more human readable terminal commands for sending/receiving HTTP data, I recommend using [HTTPIe](https://httpie.io/cli).
HTTPIe is built from the ground up for painless testing and debugging of APIs, HTTP servers, and web services.

[Install HTTPIe](https://httpie.io/docs/cli/installation) for your platform and verify installation:

```sh
http
```

This should output similar to:

```sh
usage:
    http [METHOD] URL [REQUEST_ITEM ...]

error:
    the following arguments are required: URL

for more information:
    run 'http --help' or visit https://httpie.io/docs/cli
```

## Start a Dapr Application

The dapr run command launches an application, together with a sidecar.

Launch a Dapr sidecar that will listen on port 3500 for a blank application named `myapp`:

```sh
dapr run --app-id myapp --dapr-http-port 3500
```

> Since no custom component folder was defined with the above command, Dapr uses the default component definitions created during the dapr init flow.

This should output similar to:

```sh
WARNING: no application command found.
ℹ️  Starting Dapr with id myapp. HTTP Port: 3500. gRPC Port: 55681
ℹ️  Checking if Dapr sidecar is listening on HTTP port 3500
INFO[0000] starting Dapr Runtime -- version 1.10.2 -- commit d02fb79c051f8d7d45097d5eb06b3153ce4a3a24  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] log level set to: info                        app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] metrics server started on :55682/             app_id=myapp instance=Macbook scope=dapr.metrics type=log ver=1.10.2
INFO[0000] Resiliency configuration loaded.              app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] standalone mode configured                    app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] app id: myapp                                 app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] mTLS is disabled. Skipping certificate request and tls validation  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] Dapr trace sampler initialized: DaprTraceSampler(P=1.000000)  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] local service entry announced: myapp -> 192.168.0.18:55683  app_id=myapp component="mdns (nameResolution/v1)" instance=Macbook scope=dapr.contrib type=log ver=1.10.2
INFO[0000] Initialized name resolution to mdns           app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] loading components                            app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] component loaded. name: pubsub, type: pubsub.redis/v1  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] waiting for all outstanding components to be processed  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] detected actor state store: statestore        app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] component loaded. name: statestore, type: state.redis/v1  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] all outstanding components processed          app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] gRPC proxy enabled                            app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] gRPC server listening on TCP address: :55681  app_id=myapp instance=Macbook scope=dapr.runtime.grpc.api type=log ver=1.10.2
INFO[0000] Enabled gRPC tracing middleware               app_id=myapp instance=Macbook scope=dapr.runtime.grpc.api type=log ver=1.10.2
INFO[0000] Enabled gRPC metrics middleware               app_id=myapp instance=Macbook scope=dapr.runtime.grpc.api type=log ver=1.10.2
INFO[0000] configuring workflow engine gRPC endpoint     app_id=myapp instance=Macbook scope=dapr.runtime.wfengine type=log ver=1.10.2
INFO[0000] API gRPC server is running on port 55681      app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] enabled metrics http middleware               app_id=myapp instance=Macbook scope=dapr.runtime.http type=log ver=1.10.2
INFO[0000] enabled tracing http middleware               app_id=myapp instance=Macbook scope=dapr.runtime.http type=log ver=1.10.2
INFO[0000] HTTP server listening on TCP address: :3500   app_id=myapp instance=Macbook scope=dapr.runtime.http type=log ver=1.10.2
INFO[0000] http server is running on port 3500           app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] The request body size parameter is: 4         app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] gRPC server listening on TCP address: :55683  app_id=myapp instance=Macbook scope=dapr.runtime.grpc.internal type=log ver=1.10.2
INFO[0000] Enabled gRPC tracing middleware               app_id=myapp instance=Macbook scope=dapr.runtime.grpc.internal type=log ver=1.10.2
INFO[0000] Enabled gRPC metrics middleware               app_id=myapp instance=Macbook scope=dapr.runtime.grpc.internal type=log ver=1.10.2
INFO[0000] internal gRPC server is running on port 55683  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
WARN[0000] App channel is not initialized. Did you configure an app-port?  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] actor runtime started. actor idle timeout: 1h0m0s. actor scan interval: 30s  app_id=myapp instance=Macbook scope=dapr.runtime.actor type=log ver=1.10.2
INFO[0000] configuring workflow engine with actors backend  app_id=myapp instance=Macbook scope=dapr.runtime.wfengine type=log ver=1.10.2
INFO[0000] Registering component for dapr workflow engine...  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] initializing Dapr workflow component          app_id=myapp component="dapr (workflow.dapr/v1)" instance=Macbook scope=dapr.contrib type=log ver=1.10.2
WARN[0000] app channel not initialized, make sure -app-port is specified if pubsub subscription is required  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
WARN[0000] failed to read from bindings: app channel not initialized   app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] dapr initialized. Status: Running. Init Elapsed 27ms  app_id=myapp instance=Macbook scope=dapr.runtime type=log ver=1.10.2
INFO[0000] placement tables updated, version: 0          app_id=myapp instance=Macbook scope=dapr.runtime.actor.internal.placement type=log ver=1.10.2
ℹ️  Checking if Dapr sidecar is listening on GRPC port 55681
ℹ️  Dapr sidecar is up and running.
✅  You're up and running! Dapr logs will appear here.

```

## Save State

By posting the below JSON, you will set the state of the application

```sh
echo -n '[{ "key": "name", "value": "Bruce Wayne"}]' | http POST http://localhost:3500/v1.0/state/statestore
```

This should output similar to:

```sh
HTTP/1.1 204 No Content
Content-Type: text/plain; charset=utf-8
Date: Fri, 24 Feb 2023 19:10:37 GMT
Traceparent: 00-0adec51881207429def6009d09cf532d-3ca1eae3779edc3b-01
```

