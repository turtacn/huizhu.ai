# Huizhu.AI

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) [![Docs](https://img.shields.io/badge/Docs-Huizhu.AI-orange)](docs/architecture.md)

**Huizhu.AI ("慧助"，智能业务伙伴 - Intelligent Business Partner)** is a comprehensive platform designed to empower enterprise customers with AI-driven solutions for their daily business operations. It integrates Generative AI (GenAI), AIOps (AI for IT Operations), and AI Security capabilities into a unified, conversational interface.

The primary goal of Huizhu.AI is to enhance operational efficiency, reduce costs, and improve decision-making by automating and augmenting tasks across various business functions.

  * **中文**: [README-zh.md](README-zh.md).

## Customer Pain Points & Value Proposition

Huizhu.AI aims to address several critical challenges faced by modern enterprises:

**Pain Points:**

* **Operational Inefficiencies**: Businesses grapple with numerous repetitive, information-intensive tasks in marketing, customer service, internal knowledge management, IT operations, and security.
* **Lack of Specialized Tools**: Many companies lack dedicated professional tools and AI assistants, often resorting to expensive outsourcing or high-cost consultants.
* **Marketing & Sales Challenges**: Difficulty in professional market analysis, creating engaging marketing content, and gaining timely competitor insights, leading to suboptimal campaign performance.
* **IT Complexity & Security Risks**: Complex IT environments and frequent security incidents are hard to manage without specialized personnel, resulting in delays in fault resolution and risk mitigation.
* **Knowledge & Collaboration Barriers**: Cumbersome internal processes, dispersed documentation, and inefficient knowledge retrieval hinder team productivity and collaboration.

**Our Value Proposition:**

Huizhu.AI provides a one-stop, conversational, intelligent business operations support platform, delivering tangible value:

* **Smart Marketing (慧营销)**:
    * Automates the generation of diverse marketing materials (copy, images, video scripts).
    * Delivers one-click market trend reports and intelligent competitor analysis.
    * Streamlines the "Data → Insight → Creativity → Campaign Delivery" lifecycle.
* **Smart Operations & Security (慧运维 / 慧安全)**:
    * Offers natural language-based IT fault diagnosis, log correlation analysis, and actionable root cause analysis.
    * Provides intelligent interpretation of security events with clear disposition advisories.
    * Includes a built-in compliance self-assessment assistant for generating reports and remediation plans.
* **Smart Collaboration (慧协同)**:
    * Enables intelligent Q&A based on the enterprise's own knowledge base.
    * Automates comparison and review of documents and contracts, highlighting risks.
    * Generates, archives, and distributes meeting minutes automatically.
    * Offers a unified portal to track key business progress and manage tasks.

## Key Functional Features

* **Conversational AI Assistant**: Natural language interaction for accessing all platform capabilities.
* **Unified Business Portal**: A centralized dashboard for visualizations, reports, and module access.
* **Multi-Scenario Marketing Support**: AI-powered content generation, market analysis, and competitor insights.
* **Intelligent Customer Service**: Automated responses, multi-turn dialogue support, and FAQ generation from historical data.
* **Proactive IT Operations**: Real-time monitoring, automated alerting, and AI-driven root cause analysis for IT infrastructure.
* **Responsive AI Security**: Automated detection of security events (anomalous logins, suspicious traffic) with actionable remediation advice and compliance reporting.
* **Enhanced Knowledge Management**: Intelligent search, document comparison, and automated summarization for internal documents and meeting recordings.
* **Multi-Tenancy & RBAC**: Secure data isolation between enterprise clients and fine-grained role-based access control.

## Architecture Overview

Huizhu.AI is built upon a **microservices architecture**, ensuring scalability, resilience, and maintainability. Key architectural layers include:

* **User Interface Layer**: Provides a conversational AI assistant and a unified business portal.
* **API Gateway Layer**: Acts as the single entry point for all client requests.
* **Core Smart Services Layer**: Houses the main AI engines (GenAI, AIOps, AISec, Data Analytics).
* **Domain Services Layer**: Contains business logic services like User Management, Knowledge Management, etc.
* **Infrastructure Layer**: Provides foundational support like databases, message queues, and observability tools.
* **Data & Knowledge Layer**: Manages various data sources for AI model training and inference.

All services are designed to be containerized and orchestrated using Kubernetes.

For a detailed understanding of the system architecture, components, design decisions, and technology stack, please refer to the [**Huizhu.AI Architecture Document (`docs/architecture.md`)**](docs/architecture.md).

## Technology Stack (Highlights)

* **Backend**: Go (Golang)
* **API & Communication**: gRPC (inter-service), RESTful APIs (via API Gateway), WebSockets
* **Orchestration**: Kubernetes, Docker
* **Databases**: PostgreSQL/MySQL, Elasticsearch, Prometheus/Mimir, Vector Databases (e.g., Qdrant, Milvus), MinIO/S3-compatible
* **Messaging**: NATS / Kafka / RabbitMQ
* **Observability**: Prometheus, Grafana, OpenTelemetry, ELK Stack (or similar)
* **AI/ML**: LangChain (Go version), Hugging Face models, OpenAI API integration.

## Getting Started

This section guides you through setting up Huizhu.AI for development.

### Prerequisites

* **Go**: Version 1.21 or higher.
* **Docker & Docker Compose**: For managing services and dependencies locally.
* **Kubernetes Cluster**: Optional, for deployment (e.g., Minikube, Kind).
* **`protoc` Compiler**: For Protocol Buffer definitions.
    * `protoc-gen-go`
    * `protoc-gen-go-grpc`

### Clone the Repository

```bash
git clone [https://github.com/turtacn/huizhu.ai.git](https://github.com/turtacn/huizhu.ai.git)
cd huizhu.ai
````

### Configuration

1.  Each service loads its configuration from YAML files (e.g., `configs/defaults.yaml`, `configs/user_service.example.yaml`) and environment variables.
2.  Copy default/example configuration files to your local setup (e.g., `configs/user_service.dev.yaml`) and update parameters like database DSNs, LLM API keys, etc.

### Build

1.  **Generate gRPC Stubs**: If `.proto` files are modified:
    ```bash
    ./scripts/gen_proto.sh
    ```
2.  **Build Services**:
    ```bash
    ./scripts/build.sh
    ```
    Or an individual service (e.g., `user_service`):
    ```bash
    go build -o ./bin/user_service ./cmd/user_service/main.go
    ```
3.  **Build Docker Images** (Example for `user_service`):
    ```bash
    docker build -f deployments/dockerfiles/Dockerfile.user_service -t turtacn/huizhu-user-service:latest .
    ```

### Running Locally (Development)

1.  **Using Docker Compose** (Recommended, requires a `docker-compose.yml`):
    ```bash
    docker-compose up -d
    ```
2.  **Running Individual Services Manually**:
    Ensure dependencies are running. Set environment variables.
    ```bash
    ./bin/user_service
    # Or: go run ./cmd/user_service/main.go
    ```

### Running on Kubernetes

1.  Push Docker images to a registry accessible by your K8s cluster.
2.  Apply manifests from `deployments/kubernetes/services/`:
    ```bash
    kubectl apply -f deployments/kubernetes/services/user-service.yaml
    # ... and so on for other services
    ```

## Running Tests

```bash
./scripts/run_tests.sh
# Or:
# go test -v ./...
```

## Project Structure

  * `api/proto/`: Protocol Buffer definitions.
  * `cmd/`: Main applications for each microservice.
  * `configs/`: Configuration files and loading logic.
  * `deployments/`: Dockerfiles, Kubernetes manifests.
  * `docs/`: Project documentation, including [architecture](docs/architecture.md).
  * `internal/`: Private application and library code.
  * `pkg/`: Public library code (e.g., client SDK).
  * `scripts/`: Utility scripts.
  * `test/`: Integration and E2E tests.

For more details, see the [architecture document](docs/architecture.md).

## Contributing

Contributions are welcome\! Please read our [**Contributing Guidelines (`CONTRIBUTING.md`)**](CONTRIBUTING.md) (to be created) for details on our development process, coding standards, and how to submit pull requests.

Use the GitHub **Issue Tracker** to report bugs and suggest features.

## License

This project is licensed under the **Apache License 2.0**. See the `LICENSE` file for details (to be created).

## Contact & Support

  * **Issues**: [Open an issue on GitHub](https://github.com/turtacn/huizhu.ai/issues).
  * **Discussions**: Use [GitHub Discussions](https://github.com/turtacn/huizhu.ai/discussions) (if enabled).
