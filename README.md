# Huizhu.AI

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen)](#) [![Docs](https://img.shields.io/badge/Docs-Huizhu.AI-orange)](docs/architecture.md)

**Huizhu.AI ("慧助AI"，企业智能业务伙伴 - Enterprise Intelligent Business Partner)** is a comprehensive platform designed to empower enterprise customers with AI-driven solutions for their daily business operations. It integrates Generative AI (GenAI), AIOps (AI for IT Operations), and AI Security capabilities into a unified, conversational interface.

The primary goal of Huizhu.AI is to enhance operational efficiency, reduce costs, and improve decision-making by automating and augmenting tasks in marketing, IT operations, security, and internal collaboration.

## Key Features

Huizhu.AI offers a suite of intelligent modules:

* **Smart Marketing (慧营销)**: Automates the creation of marketing materials (copywriting, ad creatives, short video scripts), provides market trend insights, and performs competitive analysis.
* **Smart Operations (慧运维)**: Leverages AIOps for natural language-based IT fault diagnosis, log analysis, root cause identification, and proactive maintenance suggestions.
* **Smart Security (慧安全)**: Uses AI to interpret security events, recommend incident responses, and assist with compliance checks and report generation.
* **Smart Collaboration (慧协同)**: Facilitates internal knowledge management through AI-powered Q&A over enterprise knowledge bases, automated meeting summarization, document comparison, and workflow assistance.

## Architecture Overview

Huizhu.AI is built upon a **microservices architecture**, ensuring scalability, resilience, and maintainability. Key architectural layers include:

* **User Interface Layer**: Provides a conversational AI assistant and a unified business portal for user interaction.
* **API Gateway Layer**: Acts as the single entry point for all client requests, handling routing, authentication, and rate limiting.
* **Core Smart Services Layer**: Houses the main AI engines (GenAI, AIOps, AISec, Data Analytics).
* **Domain Services Layer**: Contains business logic services like User Management, Knowledge Management, Notifications, etc.
* **Infrastructure Layer**: Provides foundational support like databases, message queues, caching, and observability tools.
* **Data & Knowledge Layer**: Manages various data sources crucial for AI model training and inference.

All services are designed to be containerized and orchestrated using Kubernetes.

For a detailed understanding of the system architecture, components, design decisions, and technology stack, please refer to the [**Huizhu.AI Architecture Document (`docs/architecture.md`)**](docs/architecture.md).

## Technology Stack (Highlights)

* **Backend**: Go (Golang)
* **API & Communication**: gRPC (inter-service), RESTful APIs (via API Gateway), WebSockets
* **Frontend**: (To be decided - e.g., Vue.js, React)
* **Orchestration**: Kubernetes, Docker
* **Databases**:
    * PostgreSQL/MySQL (Relational data)
    * Elasticsearch (Logging, full-text search)
    * Prometheus/Mimir (Time-series data for monitoring & AIOps)
    * Vector Databases (e.g., Qdrant, Milvus, Weaviate for RAG)
    * MinIO/S3-compatible (Object storage for files, models)
* **Messaging**: NATS / Kafka / RabbitMQ (Asynchronous tasks, event-driven architecture)
* **Caching**: Redis
* **Observability**: Prometheus, Grafana, OpenTelemetry, ELK Stack (or similar)
* **AI/ML**: LangChain (Go version), Hugging Face models, OpenAI API integration, custom models.

## Getting Started

This section guides you through setting up Huizhu.AI for development and testing.

### Prerequisites

* **Go**: Version 1.21 or higher.
* **Docker & Docker Compose**: For managing services and dependencies locally.
* **Kubernetes Cluster**: Optional, for deployment (e.g., Minikube, Kind, or a cloud provider's K8s).
* **`protoc` Compiler**: For compiling Protocol Buffer definitions.
    * `protoc-gen-go`
    * `protoc-gen-go-grpc`
* **Task runner**: (Optional, e.g., Make or Task for script execution). Our scripts are primarily shell scripts.

### Clone the Repository

```bash
git clone [https://github.com/turtacn/huizhu.ai.git](https://github.com/turtacn/huizhu.ai.git)
cd huizhu.ai
````

### Configuration

1.  **Application Configuration**:
    Each service loads its configuration from YAML files and environment variables (refer to `configs/config.go`).
    Copy the default configuration file (e.g., `configs/defaults.yaml` or service-specific defaults like `configs/user_service.example.yaml`) to your own configuration file (e.g., `configs/user_service.dev.yaml`).
    Update the necessary parameters, such as database connection strings, LLM API keys, service ports, etc. Services will typically look for a config file path specified by an environment variable or a default location.

2.  **Environment Variables**:
    Many configurations can be overridden or set via environment variables. This is particularly useful for Docker and Kubernetes deployments. Check individual service documentation or `main.go` files for specific environment variables used.

### Build

1.  **Generate gRPC Stubs**:
    If you modify any `.proto` files in the `api/proto/` directory, or if they are not pre-generated, run the script:

    ```bash
    ./scripts/gen_proto.sh
    ```

2.  **Build Services**:
    To build all service binaries:

    ```bash
    ./scripts/build.sh
    ```

    Alternatively, to build an individual service (e.g., `user_service`):

    ```bash
    go build -o ./bin/user_service ./cmd/user_service/main.go
    ```

3.  **Build Docker Images**:
    Dockerfiles for each service are located in `deployments/dockerfiles/`.
    Example for `user_service`:

    ```bash
    docker build -f deployments/dockerfiles/Dockerfile.user_service -t turtacn/huizhu-user-service:latest .
    ```

    (Repeat for other services)

### Running Locally (Development)

It's recommended to use Docker Compose for a streamlined local development experience (a `docker-compose.yml` file should be present or created in the project root, defining all services and their dependencies like databases).

1.  **Using Docker Compose** (Recommended):
    Ensure you have a `docker-compose.yml` file configured.

    ```bash
    docker-compose up -d
    ```

    To view logs:

    ```bash
    docker-compose logs -f <service_name>
    ```

2.  **Running Individual Services Manually**:
    If you prefer to run services directly without Docker Compose (e.g., for debugging a specific service):

      * Ensure all dependencies (databases, message queues) are running and accessible.
      * Set necessary environment variables for configuration.
      * Run the service binary:
        ```bash
        ./bin/user_service
        # or using go run
        # go run ./cmd/user_service/main.go
        ```

### Running on Kubernetes

1.  **Prerequisites**:

      * A running Kubernetes cluster.
      * `kubectl` configured to communicate with your cluster.
      * Docker images for all services pushed to a container registry accessible by your cluster.

2.  **Deploy**:
    Kubernetes manifests are located in `deployments/kubernetes/services/`.

    ```bash
    # Apply common configurations first (if any)
    # kubectl apply -f deployments/kubernetes/common/

    # Deploy each service
    kubectl apply -f deployments/kubernetes/services/user-service.yaml
    kubectl apply -f deployments/kubernetes/services/knowledge-service.yaml
    # ... and so on for all other services and the API gateway
    ```

## Running Tests

To run all unit and integration tests (where applicable):

```bash
./scripts/run_tests.sh
```

Or, for more granular control:

```bash
go test -v ./...
# To run tests for a specific package:
# go test -v ./internal/service/user/application/...
```

## Project Structure

A brief overview of the main directories:

  * `api/proto/`: Contains Protocol Buffer definitions for gRPC services.
  * `cmd/`: Main applications for each microservice.
  * `configs/`: Configuration files and loading logic.
  * `deployments/`: Dockerfiles, Kubernetes manifests, and other deployment-related files.
  * `docs/`: Project documentation, including this README and the detailed [architecture document](docs/architecture.md).
  * `internal/`: Private application and library code. This is where the core logic of each service and shared platform components reside.
      * `internal/service/<service_name>/`: Code for individual microservices.
      * `internal/platform/`: Shared platform-level libraries (logging, errors, DB connectors, etc.).
      * `internal/common/`: Low-level shared constants, enums, and basic types.
  * `pkg/`: Public library code, intended to be importable by external projects (e.g., a client SDK for Huizhu.AI).
  * `scripts/`: Utility scripts for building, testing, generating code, etc.
  * `test/`: Integration and end-to-end tests.
  * `web/`: Placeholder for frontend application code (may be moved to a separate repository).

For a more detailed explanation of the project structure, refer to the [architecture document](docs/architecture.md).

## Contributing

We welcome contributions to Huizhu.AI\! Whether it's bug fixes, new features, or improvements to documentation, your help is appreciated.

Please read our [**Contributing Guidelines (`CONTRIBUTING.md`)**](https://www.google.com/search?q=CONTRIBUTING.md) for detailed information on how to contribute, including our code of conduct, development process, and coding standards. (Note: `CONTRIBUTING.md` should be created).

**General Contribution Flow:**

1.  **Fork** the repository on GitHub.
2.  **Clone** your fork locally: `git clone https://github.com/YOUR_USERNAME/huizhu.ai.git`
3.  Create a **new branch** for your changes: `git checkout -b feature/your-feature-name` or `bugfix/issue-number`.
4.  Make your **changes**, adhering to the project's coding style (`gofmt`, `golangci-lint`).
5.  **Test** your changes thoroughly. Add new tests if applicable.
6.  **Commit** your changes with clear, descriptive commit messages.
7.  **Push** your changes to your fork: `git push origin feature/your-feature-name`.
8.  Open a **Pull Request (PR)** against the `main` branch of `turtacn/huizhu.ai`.
9.  Engage in the PR review process and address any feedback.

Please use the GitHub **Issue Tracker** to report bugs, suggest features, or ask questions.

## License

This project is licensed under the **Apache License 2.0**. See the [LICENSE](https://www.google.com/search?q=LICENSE) file for details. (Note: `LICENSE` file should be created).

## Contact & Support

  * **Issues**: For bugs, feature requests, or problems, please [open an issue on GitHub](https://www.google.com/search?q=https://github.com/turtacn/huizhu.ai/issues).
  * **Discussions**: For general questions or discussions, please use the [GitHub Discussions tab](https://www.google.com/search?q=https://github.com/turtacn/huizhu.ai/discussions) (if enabled).
