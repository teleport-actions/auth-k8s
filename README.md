<div align="center">
   <img src="https://github.com/gravitational/teleport-actions/raw/main/assets/img/readme-header.png" width=750/>
   <div align="center" style="padding: 25px">
      <a href="https://www.apache.org/licenses/LICENSE-2.0">
      <img src="https://img.shields.io/badge/Apache-2.0-red.svg" />
      </a>
   </div>
</div>
</br>

> Read our Blog: <https://goteleport.com/blog/>

> Read our Documentation: <https://goteleport.com/docs/getting-started/>

# `teleport-actions/auth-k8s`

`auth-k8s` uses Teleport Machine ID to generate an authorized Kubernetes client
configuration for a specified cluster protected by Teleport.

The action sets the `KUBECONFIG` environment variable for consecutive steps in
the job, meaning that tools like `kubectl` will automatically connect to the
requested Kubernetes cluster without additional configuration.

Pre-requisites:

- Teleport 11 or above must be used.
- Teleport binaries must already be installed in the job environment.
- The Kubernetes cluster you wish to access must already be connected to your
  Teleport cluster. See
  <https://goteleport.com/docs/kubernetes-access/getting-started/>
- You must have created a bot with a role with access to your Kubernetes cluster
  and created a GitHub join token that allows that bot to join.
- A Linux based runner.

Example usage:

```yaml
on:
  workflow_dispatch: {}
jobs:
  demo-auth-k8s:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
    steps:
      - name: Install Kubectl
        uses: azure/setup-kubectl@v3
      - name: Install Teleport
        uses: teleport-actions/setup@v1
        with:
          version: 12.1.0
      - name: Authorize against Teleport
        uses: teleport-actions/auth-k8s@v1
        with:
          # Specify the publically accessible address of your Teleport proxy.
          proxy: tele.example.com:443
          # Specify the name of the join token for your bot.
          token: my-github-join-token-name
          # Specify the length of time that the generated credentials should be
          # valid for. This is optional and defaults to "1h"
          certificate-ttl: 1h
          # Specify the name of the Kubernetes cluster you wish to access.
          kubernetes-cluster: my-kubernetes-cluster
          # Enable submission of anonymous usage telemetry to Teleport.
          # See https://goteleport.com/docs/machine-id/reference/telemetry/ for
          # more information.
          anonymous-telemetry: 1
      - name: List pods
        run: kubectl get pods
```

## Next steps

Read the `teleport-actions/auth-k8s` getting started guide:
<https://goteleport.com/docs/machine-id/guides/github-actions-kubernetes/>
