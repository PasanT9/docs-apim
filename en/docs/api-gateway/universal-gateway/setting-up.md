# Setting Up Universal Gateway

This guide provides detailed instructions for deploying the Universal Gateway in production environments. Choose the setup option that matches your infrastructure.

If you are getting started for the first time, see [Getting Started with Universal Gateway]({{base_path}}/api-gateway/universal-gateway/getting-started/).

## Docker setup

### Prerequisites

- `curl`
- `unzip`
- A Docker-compatible container runtime such as Docker Desktop, Rancher Desktop, Colima, or Docker Engine with Compose support

Ensure that the following commands are available:

```bash
docker --version
docker compose version
```

### Step 1: Download the Gateway

```bash
curl -sLO https://github.com/wso2/api-platform/releases/download/gateway/v0.12.0/gateway-v0.12.0.zip && \
unzip gateway-v0.12.0.zip
```

### Step 2: Configure the Gateway

Create `gateway-v0.12.0/configs/keys.env` with the required environment variables:

```bash
cat > gateway-v0.12.0/configs/keys.env << 'ENVFILE'
MOESIF_KEY=<your-moesif-key>
GATEWAY_CONTROLPLANE_HOST=<your-control-plane-host>:<9443>
GATEWAY_REGISTRATION_TOKEN=<your-gateway-token>
ENVFILE
```

### Step 3: Start the Gateway

```bash
cd gateway-v0.12.0
docker compose --env-file configs/keys.env up
```

To run the gateway in detached mode:

```bash
docker compose --env-file configs/keys.env up -d
```

### Step 4: Verify the Gateway

```bash
docker compose ps
```

### Stop the Gateway

```bash
docker compose down
```

## Virtual machine setup

### Prerequisites

- `curl`
- `unzip`
- Docker Compose installed

### Step 1: Download the Gateway

```bash
curl -sLO https://github.com/wso2/api-platform/releases/download/gateway/v0.12.0/gateway-v0.12.0.zip && \
unzip gateway-v0.12.0.zip
```

### Step 2: Configure the Gateway

Create `gateway-v0.12.0/configs/keys.env` with the required environment variables:

```bash
cat > gateway-v0.12.0/configs/keys.env << 'ENVFILE'
MOESIF_KEY=<your-moesif-key>
GATEWAY_CONTROLPLANE_HOST=connect.bijira.dev
GATEWAY_REGISTRATION_TOKEN=<your-gateway-token>
ENVFILE
```

!!! important
    Replace `<your-gateway-token>` with the gateway registration token from the console. This token is shown only once, so ensure that you copy it before leaving the page.

### Step 3: Start the Gateway

```bash
cd gateway-v0.12.0
docker compose --env-file configs/keys.env up
```

To run the gateway in detached mode:

```bash
docker compose --env-file configs/keys.env up -d
```

### Step 4: Verify the Gateway

```bash
docker compose ps
```

### Stop the Gateway

```bash
docker compose down
```

## Kubernetes setup

### Prerequisites

- `curl`
- Kubernetes 1.32 or later
- Helm 3.18 or later
- Permission to install `cert-manager`, or an existing `cert-manager` installation

### Install cert-manager

If `cert-manager` is not already installed, run the following commands:

```bash
helm repo add jetstack https://charts.jetstack.io --force-update
helm repo update

helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --set crds.enabled=true
```

### Install the Gateway chart

Run the following command to install the gateway chart with control plane configurations:

```bash
helm install gateway oci://ghcr.io/wso2/api-platform/helm-charts/gateway --version 0.12.0 \
  --set gateway.controller.controlPlane.host="connect.bijira.dev" \
  --set gateway.controller.controlPlane.port=443 \
  --set gateway.controller.controlPlane.token.value="your-gateway-token" \
  --set gateway.config.analytics.publishers.moesif.application_id="your-moesif-key" \
  --set gateway.config.analytics.enabled=true
```

!!! important
    Replace `your-gateway-token` with the gateway registration token from the console. This token is shown only once, so ensure that you copy it before leaving the page.

### Verify the installation

```bash
kubectl get pods
```

### Upgrade the Gateway

```bash
helm upgrade gateway oci://ghcr.io/wso2/api-platform/helm-charts/gateway --version <new-version> \
  -f values.yaml
```

### Uninstall the Gateway

```bash
helm uninstall gateway
```

## What's next

- [Adding and Managing Policies]({{base_path}}/api-gateway/self-hosted-gateway/adding-and-managing-policies/)
