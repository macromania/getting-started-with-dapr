# Getting Started with Dapr

Dapr is a portable, event-driven runtime that makes it easy for any developer to build resilient, stateless and stateful applications that run on the cloud and edge and embraces the diversity of languages and developer frameworks. Leveraging the benefits of a sidecar architecture, Dapr helps you tackle the challenges that come with building microservices and keeps your code platform agnostic.

## Installation

Choose a Dapr CLI installation for your platform
<https://docs.dapr.io/getting-started/install-dapr-cli/>

### Verify Installation

Verify your installation by running:

```sh
dapr -h
```

This should output similar to:

```sh

          __
     ____/ /___ _____  _____
    / __  / __ '/ __ \/ ___/
   / /_/ / /_/ / /_/ / /
   \__,_/\__,_/ .___/_/
      /_/

===============================
Distributed Application Runtime

Usage:
  dapr [flags]
  dapr [command]

Available Commands:
  annotate       Add dapr annotations to a Kubernetes configuration. Supported platforms: Kubernetes
  build-info     Print build info of Dapr CLI and runtime
  completion     Generates shell completion scripts
  components     List all Dapr components. Supported platforms: Kubernetes
  configurations List all Dapr configurations. Supported platforms: Kubernetes
  dashboard      Start Dapr dashboard. Supported platforms: Kubernetes and self-hosted
  help           Help about any command
  init           Install Dapr on supported hosting platforms. Supported platforms: Kubernetes and self-hosted
  invoke         Invoke a method on a given Dapr application. Supported platforms: Self-hosted
  list           List all Dapr instances. Supported platforms: Kubernetes and self-hosted
  logs           Get Dapr sidecar logs for an application. Supported platforms: Kubernetes
  mtls           Check if mTLS is enabled. Supported platforms: Kubernetes
  publish        Publish a pub-sub event. Supported platforms: Self-hosted
  run            Run Dapr and (optionally) your application side by side. Supported platforms: Self-hosted
  status         Show the health status of Dapr services. Supported platforms: Kubernetes
  stop           Stop Dapr instances and their associated apps. Supported platforms: Self-hosted
  uninstall      Uninstall Dapr runtime. Supported platforms: Kubernetes and self-hosted
  upgrade        Upgrades or downgrades a Dapr control plane installation in a cluster. Supported platforms: Kubernetes
  version        Print the Dapr runtime and CLI version

Flags:
  -h, --help                  help for dapr
      --log-as-json           Log output in JSON format
      --runtime-path string   The path to the dapr runtime installation directory
  -v, --version               version for dapr

Use "dapr [command] --help" for more information about a command.

```

## Initialize

Now that you’ve installed the Dapr CLI, use the CLI to initialize Dapr on your local machine.

Dapr runs as a sidecar alongside your application. In self-hosted mode, this means it is a process on your local machine. By initializing Dapr, you:

- Fetch and install the Dapr sidecar binaries locally.
- Create a development environment that streamlines application development with Dapr.

Dapr initialization includes:

- Running a Redis container instance to be used as a local state store and message broker.
- Running a Zipkin container instance for observability.
- Creating a default components folder with component definitions for the above.
- Running a Dapr placement service container instance for local actor support.


Run following to download container images and install latest runtime version:

```sh
dapr init
```

This should output similar to:

```sh
⌛  Making the jump to hyperspace...
ℹ️  Container images will be pulled from Docker Hub
ℹ️  Installing runtime version 1.10.2
↑  Downloading binaries and setting up components...
Dapr runtime installed to /Users/{username}/.dapr/bin, you may run the following to add it to your path if you want to run daprd directly:
    export PATH=$PATH:/Users/{username}/.dapr/bin
✅  Downloading binaries and setting up components...
✅  Downloaded binaries and completed components set up.
ℹ️  daprd binary has been installed to /Users/{user}/.dapr/bin.
ℹ️  dapr_placement container is running.
ℹ️  dapr_redis container is running.
ℹ️  dapr_zipkin container is running.
ℹ️  Use `docker ps` to check running containers.
✅  Success! Dapr is up and running. To get started, go here: https://aka.ms/dapr-getting-started

```

You can verify `Redis`, `Zipkin` and `Placement` containers running:

```sh
docker ps
```

This should output similar to:

```sh
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS                   PORTS                              NAMES
7c060c462943   daprio/dapr:1.10.2   "./placement"            2 minutes ago   Up 2 minutes             0.0.0.0:50005->50005/tcp           dapr_placement
aecaa08fe299   openzipkin/zipkin    "start-zipkin"           2 minutes ago   Up 2 minutes (healthy)   9410/tcp, 0.0.0.0:9411->9411/tcp   dapr_zipkin
b5177f319d54   redis:6              "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes             0.0.0.0:6379->6379/tcp             dapr_redis
```

On dapr init, the CLI also creates a default components folder that contains several YAML files with definitions for a state store, Pub/sub, and Zipkin. The Dapr sidecar will read these components and use:

- The Redis container for state management and messaging.
- The Zipkin container for collecting traces.

Verify by opening your components directory:

```sh
ls ~/.dapr
```

This should output similar to:

```sh
bin         components  config.yaml
```c