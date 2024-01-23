# external-secrets-components Helm Chart

## Overview

The `external-secrets-components` Helm chart is designed to simplify and automate the management of Kubernetes secrets, particularly in environments that practice GitOps, like with ArgoCD. This chart allows you to integrate external secrets into your Kubernetes environment, making it easier to manage sensitive information such as database credentials, API keys, and other confidential data.

This Helm chart supports two types of secrets:
1. **Generic Secrets**: Standard Kubernetes secrets.
2. **Docker Config JSON Secrets**: Specifically for Docker configuration files.

By using this chart as a dependency in other Helm charts, you can streamline the injection of secrets into your applications, ensuring that secret management adheres to the principles of infrastructure as code.

## Use Cases

- **ArgoCD GitOps**: In a GitOps workflow with ArgoCD, you can include this chart as a dependency. This allows you to manage secrets through code, ensuring that they are consistently deployed and managed alongside your applications.
- **Automating Secret Management**: Automatically fetch secrets from external secret stores (like HashiCorp Vault, AWS Secrets Manager, etc.) and inject them into your Kubernetes applications.
- **Enhanced Security**: Avoid manual secret creation and reduce the risk of exposing sensitive data.

## Prerequisites

- Helm 3 installed
- Access to a Kubernetes cluster
- An external secret management system (like HashiCorp Vault, AWS Secrets Manager, etc.)

## Installation

1. Add this chart to your Helm project as a dependency. Edit your `Chart.yaml`:

    ```yaml
    dependencies:
      - name: external-secrets-components
        version: 0.1.0
        repository: <URL to your chart repository>
    ```

2. Update dependencies:

    ```bash
    helm dependency update
    ```

## Configuration

Modify the `values.yaml` file to fit your secret requirements. There are two main sections:

1. **genericSecret**: Configure generic Kubernetes secrets here.
2. **dockerconfigjson**: Configure Docker configuration JSON secrets here.

### Generic Secret Example

Uncomment and set the `genericSecret` values in `values.yaml`:

```yaml
genericSecret:
  enabled: true
  name: my-generic-secret
  secretStoreRef:
    name: my-secret-store
    kind: ClusterSecretStore
  data:
    - secretKey: username
      remoteRef:
        key: path/to/external/username
        property: username
    - secretKey: password
      remoteRef:
        key: path/to/external/password
        property: password
```

### Docker Config JSON Secret Example

Uncomment and set the `dockerconfigjson` values in `values.yaml`:

```yaml
dockerconfigjson:
  enabled: true
  name: my-docker-secret
  secretStoreRef:
    name: my-secret-store
    kind: ClusterSecretStore
  data:
    - secretKey: dockerconfigjson
      remoteRef:
        key: path/to/dockerconfigjson
```

## Usage

Deploy the chart in your Kubernetes cluster as part of your application deployment. If using ArgoCD, this can be part of your application's Helm configuration.
