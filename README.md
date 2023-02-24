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

This should output:

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
