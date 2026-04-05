# Setting Up

This guide provides detailed instructions for deploying Universal Gateway in production environments. Choose the infrastructure option that matches your environment.

!!! tip "Quick Start"
    If you are just getting started, see the [Getting Started]({{base_path}}/api-gateway/universal-gateway/getting-started/) guide for a quick setup.

## Setup Instructions

=== "Virtual Machine"

    ### Prerequisites

    - cURL installed
    - unzip installed
    - A Docker-compatible container runtime such as:
        - Docker Desktop (Windows / macOS)
        - Rancher Desktop (Windows / macOS)
        - Colima (macOS)
        - Docker Engine + Compose plugin (Linux)

    Ensure `docker` and `docker compose` commands are available:

    ```bash
    docker --version
    docker compose version
    ```

    ### Step 1: Download the Gateway

    Run this command in your terminal to download the gateway:

    ```bash
    curl -sLO https://github.com/wso2/api-platform/releases/download/gateway/v0.12.0/gateway-v0.12.0.zip && \
    unzip gateway-v0.12.0.zip
    ```

    ### Step 2: Configure the Gateway

    Run this command to create `gateway-v0.12.0/configs/keys.env` with the required environment variables:

    ```bash
    cat > gateway-v0.12.0/configs/keys.env << 'ENVFILE'
    MOESIF_KEY=<your-moesif-key>
    GATEWAY_CONTROLPLANE_HOST=<your-control-plane-host>:9443
    GATEWAY_REGISTRATION_TOKEN=<your-gateway-token>
    ENVFILE
    ```

    ### Step 3: Start the Gateway

    1. Navigate to the gateway folder:

        ```bash
        cd gateway-v0.12.0
        ```

    2. Run this command to start the gateway using the `configs/keys.env` file created in Step 2:

        ```bash
        docker compose --env-file configs/keys.env up
        ```

    To run in detached mode (background):

    ```bash
    docker compose --env-file configs/keys.env up -d
    ```

    ### Step 4: Verify the Gateway

    Check that the gateway is running:

    ```bash
    docker compose ps
    ```

    ### Stopping the Gateway

    To stop the gateway:

    ```bash
    docker compose down
    ```

=== "Docker"

    ### Prerequisites

    - cURL installed
    - unzip installed
    - Docker installed and running
    - Docker Compose installed

    ### Step 1: Download the Gateway

    Run this command in your terminal to download the gateway:

    ```bash
    curl -sLO https://github.com/wso2/api-platform/releases/download/gateway/v0.12.0/gateway-v0.12.0.zip && \
    unzip gateway-v0.12.0.zip
    ```

    ### Step 2: Configure the Gateway

    Run this command to create `gateway-v0.12.0/configs/keys.env` with the required environment variables:

    ```bash
    cat > gateway-v0.12.0/configs/keys.env << 'ENVFILE'
    MOESIF_KEY=<your-moesif-key>
    GATEWAY_CONTROLPLANE_HOST=<your-control-plane-host>:9443
    GATEWAY_REGISTRATION_TOKEN=<your-gateway-token>
    ENVFILE
    ```

    !!! warning "Important"
        Replace `<your-gateway-token>` with the Gateway Registration Token from the Admin Portal. This token is shown only once, so ensure you copy it before leaving the page.

    ### Step 3: Start the Gateway

    1. Navigate to the gateway folder:

        ```bash
        cd gateway-v0.12.0
        ```

    2. Run this command to start the gateway using the `configs/keys.env` file created in Step 2:

        ```bash
        docker compose --env-file configs/keys.env up
        ```

    To run in detached mode (background):

    ```bash
    docker compose --env-file configs/keys.env up -d
    ```

    ### Step 4: Verify the Gateway

    Check that the gateway is running:

    ```bash
    docker compose ps
    ```

    ### Stopping the Gateway

    To stop the gateway:

    ```bash
    docker compose down
    ```

=== "Kubernetes"

    ### Prerequisites

    - cURL installed
    - Kubernetes 1.32+ cluster
    - Helm 3.18+ installed
    - Either permissions to install cert-manager in the cluster or an existing cert-manager installation

    ### Install cert-manager (optional)

    If cert-manager is not already installed, run these commands before installing the gateway chart:

    ```bash
    helm repo add jetstack https://charts.jetstack.io --force-update
    helm repo update

    helm install cert-manager jetstack/cert-manager \
      --namespace cert-manager \
      --create-namespace \
      --set crds.enabled=true
    ```

    ### Installing the Chart

    Run this command to install the gateway chart with Control Plane configurations:

    ```bash
    helm install gateway oci://ghcr.io/wso2/api-platform/helm-charts/gateway --version 0.12.0 \
      --set gateway.controller.controlPlane.host="<your-control-plane-host>" \
      --set gateway.controller.controlPlane.port=9443 \
      --set gateway.controller.controlPlane.token.value="your-gateway-token" \
      --set gateway.config.analytics.publishers.moesif.application_id="your-moesif-key" \
      --set gateway.config.analytics.enabled=true
    ```

    !!! warning "Important"
        Replace `your-gateway-token` with the Gateway Registration Token from the Admin Portal. This token is shown only once, so ensure you copy it before leaving the page.

    ### Verifying the Installation

    Check that the gateway pods are running:

    ```bash
    kubectl get pods
    ```

    ### Upgrading the Gateway

    To upgrade to a new version:

    ```bash
    helm upgrade gateway oci://ghcr.io/wso2/api-platform/helm-charts/gateway --version <new-version> \
      -f values.yaml
    ```

    ### Uninstalling the Gateway

    To remove the gateway from your cluster:

    ```bash
    helm uninstall gateway
    ```

## What's Next?

- [Adding and Managing Policies]({{base_path}}/api-gateway/universal-gateway/adding-and-managing-policies/)
