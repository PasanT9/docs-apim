# Overview

WSO2 API Manager enables you to expose and manage APIs across multiple gateway runtimes through a unified control plane. This single control plane model allows organizations to design, publish, govern, and secure APIs centrally, while deploying them to gateway runtimes that best align with their infrastructure, compliance, and operational needs.

With this capability, you can seamlessly work with different gateway types without compromising on governance, visibility, or lifecycle control.

Supported gateway types include:

- **WSO2 Gateways** – A fully managed, self-hosted gateway runtime designed for enterprise-grade deployments with maximum control and flexibility.
- **Third-Party Gateways** – Integration with supported external gateways, enabling centralized management of APIs deployed across heterogeneous environments.

---

## WSO2 Gateways

WSO2 Gateways provide a robust, self-hosted runtime that allows you to deploy APIs within your own infrastructure while maintaining centralized control from the API Manager control plane.

This model is ideal for organizations that require:

- Full control over runtime environments
- Data residency and compliance alignment
- Isolation across network boundaries
- Flexible deployment topologies (VM, docker, Kubernates)

With WSO2 Gateways, you get the best of both worlds: **centralized API management** and **localized runtime execution**.

For more information, see:

- [Getting Started with WSO2 Universal Gateway]({{base_path}}/api-gateway/universal-gateway/getting-started/)
- [Getting Started with WSO2 Universal Gateway - Classic]({{base_path}}/api-gateway/overview-of-the-api-gateway/)

---

## Third-Party Gateways

WSO2 API Manager also supports a federated gateway model, allowing you to manage APIs deployed on supported external gateway platforms.

This approach enables organizations to:

- Govern APIs across multiple gateway vendors
- Maintain a single source of truth for API definitions and policies
- Achieve consistency in security, lifecycle, and governance across distributed environments

For more information, see:

- [Federated Gateways Overview]({{base_path}}/api-gateway/federated-gateways/overview/)

---

## Related tutorials

- [WSO2's Centralized API Management: The Single Control Plane for Multiple Gateways]({{base_path}}/tutorials/single-control-plane-for-multiple-gateways/)