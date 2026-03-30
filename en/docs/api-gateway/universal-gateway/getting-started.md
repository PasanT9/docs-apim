# Getting Started with Universal Gateway

This guide walks you through setting up a Universal Gateway in your environment. Follow these steps to get the gateway running and connected to the control plane.

## Overview

The Universal Gateway allows you to run the API gateway in your own infrastructure while maintaining centralized management through the control plane.

## Prerequisites

Before you begin, ensure that you have the following:

- `curl`
- `unzip`
- Docker installed and running
- Docker Compose installed

## Create a Universal Gateway in the Admin Portal

1. Sign in to the admin portal.
2. Go to the Gateway section from the left navigation panel.
3. On WSO2 Gateways section, click on **Add WSO2 Gateways** button
4. Select the **Universal Gateway** from **Gateway Environment Type**.
5. Provide the following details:

    - **Display Name**: A unique name for your gateway.
    - **Description**: An optional description.
    - **URL**: The URL where the gateway will be accessible. For example, `https://localhost:8443`.
    - **Visibility**: The devportal visibility restriction base on th roal.

6. Click **Add**.

## Set up the Gateway

### Step 1: Download the Gateway

Run the following command to download the gateway:

```bash
curl -sLO https://github.com/wso2/api-platform/releases/download/gateway/v1.0.0/gateway-v1.0.0.zip && \
unzip gateway-v1.0.0.zip
```

### Step 2: Configure the Gateway

Run the following command to create the gateway configuration:

```bash
cat > gateway-v1.0.0/configs/keys.env << 'ENVFILE'
MOESIF_KEY=<your-moesif-key>
GATEWAY_CONTROLPLANE_HOST=localhost:9443
GATEWAY_REGISTRATION_TOKEN=<your-gateway-token>
ENVFILE
```

When you copy this command from the console, the placeholder values are populated automatically.

### Step 3: Start the Gateway

Navigate to the gateway directory and start the gateway using Docker Compose:

```bash
cd gateway-v1.0.0
docker compose --env-file configs/keys.env up
```

### Step 4: Verify the Gateway

Check that the gateway is running and connected:

```bash
# Check container status
docker compose ps

# Check gateway health
curl http://localhost:9002/health
```

The gateway should appear as active in the control plane console.

## Add an API and invoke it

!!! note
    This feature is currently available only for REST API proxies that are created from scratch, or by importing from OpenAP.

    It is not currently available for WebSocket, GraphQL, MCP, or AI APIs.

### Step 1: Create a REST API

In this example, you will use the URL of an OpenAPI definition to create an API proxy.

1. Sign in to the **publisher** portal.
2. Click on REST APIs.
3. Select **Import API Contract**.
4. Select the **URL** option and provide the following URL:

```text
https://raw.githubusercontent.com/wso2/bijira-samples/refs/heads/main/reading-list-api/openapi.yaml
```

5. Click **Next** and edit the predefined values if required.
6. Select the gateway type as **Universal Gateway**.
7. Click **Create**.

### Step 2: Deploy the REST API

This step is optional during the initial creation because the API is deployed to the gateway by default. If you change the API later, you must redeploy it.

To redeploy the API:

1. Navigate to the **Deploy** page of the API proxy.
2. Click **Deploy**.

## Test the API Proxy

1. Navigate to **Test** -> **cURL** in the API proxy.
2. Use the generated cURL command for the required resource to test the API proxy.

## Next steps

- [Setting Up Universal Gateway]({{base_path}}/api-gateway/universal-gateway/setting-up/)
- [Adding and Managing Policies]({{base_path}}/api-gateway/universal-gateway/adding-and-managing-policies/)
