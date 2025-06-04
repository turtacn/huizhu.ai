# Huizhu.AI - Your Intelligent Business Co-Pilot

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Docs](https://img.shields.io/badge/docs-available-orange.svg)](docs/architecture.md)

**Huizhu.AI** (慧助AI - "HuiZhu AI for Enterprise") is a one-stop, conversational intelligent business operations support platform powered by Generative AI, AIOps, and AI Security capabilities. It aims to empower enterprise clients by addressing pain points in marketing, IT operations & security response, and internal collaboration & knowledge management. Through intelligent automation, Huizhu.AI strives to enhance operational efficiency and decision-making quality.

**Current Status:** Project Initialized - Architecture Design & Core Scaffolding in Progress.

## ✨ Features (Planned)

Huizhu.AI will offer a suite of intelligent services, categorized as follows:

*   **🧠 GenAI Marketing (慧营销 - HuiMarketing)**
    *   Automated generation of marketing copy (ads, social media posts, product descriptions).
    *   Market trend analysis and insight generation.
    *   Competitor analysis and reporting.
    *   Personalized content recommendations.
*   **⚙️ AIOps (慧运维 - HuiOps)**
    *   Intelligent IT infrastructure monitoring and alerting.
    *   Automated log analysis and anomaly detection.
    *   Root cause analysis for incidents.
    *   Predictive maintenance suggestions.
*   **🛡️ AI Security (慧安全 - HuiSecurity)**
    *   Security event correlation and intelligent interpretation.
    *   Automated threat intelligence gathering.
    *   Incident response suggestions and playbooks.
    *   Compliance reporting assistance (e.g., GDPR, SOX).
*   **🤝 Intelligent Collaboration (慧协同 - HuiCollab)**
    *   Automatic meeting minute generation and summarization.
    *   Smart document comparison and review (e.g., contracts).
    *   Intelligent SOP (Standard Operating Procedure) search and Q&A.
    *   Knowledge base creation and management.
*   **📚 Enterprise Knowledge Base**
    *   Centralized repository for internal documents, SOPs, case studies.
    *   RAG (Retrieval Augmented Generation) capabilities for contextual Q&A.
    *   Semantic search over enterprise data.
*   **🌐 Unified Portal & Conversational AI**
    *   A central web portal for accessing all services and dashboards.
    *   A conversational AI assistant for interactive task execution and information retrieval.
    *   Multi-tenancy support with role-based access control (RBAC).

##  arquitetura Architecture Overview

Huizhu.AI adopts a microservices architecture, primarily built with **Golang**, and designed for scalability, resilience, and maintainability.

Key architectural components:

1.  **User Interface Layer**:
    *   **Conversational AI Assistant**: WebSocket-based chat interface.
    *   **Unified Business Portal**: Web application (React/Vue).
2.  **API Gateway Layer**:
    *   Single entry point for all client requests (e.g., Traefik, Kong, or custom Go-based).
    *   Handles routing, authentication (JWT), rate limiting, and basic monitoring.
3.  **Core Services Layer (Microservices)**:
    *   `GenAI Marketing Service`
    *   `AIOps Service`
    *   `AI Security Service`
    *   `Collaboration Service`
    *   `Knowledge Base Service`
    *   `User Management Service`
    *   `Data Analytics Service`
    *   `Billing Service`
    *   `Notification Service`
    *   Communication: gRPC (primary), REST, Message Queues (Kafka/RabbitMQ).
4.  **Data & Knowledge Layer**:
    *   **Databases**: PostgreSQL (system data), Vector DB (Milvus/Weaviate for RAG), Time-Series DB (Prometheus/Mimir for metrics), Elasticsearch (logs).
    *   **Storage**: Object Storage (MinIO/S3 for models, files), Distributed Cache (Redis).
    *   **Data Sources**: Enterprise knowledge, business data (CRM, ERP - with authorization), platform data (logs, metrics), public data.
5.  **Infrastructure Layer**:
    *   **Containerization & Orchestration**: Docker, Kubernetes.
    *   **AI Models**: LLMs (OpenAI, fine-tuned open-source models), custom AIOps/Security models.
    *   **Inference Serving**: Triton Inference Server / TorchServe.
    *   **Message Queues**: Kafka / RabbitMQ.
    *   **Caching**: Redis.

For a detailed architecture design, please refer to [`docs/architecture.md`](docs/architecture.md).

## 🚀 Getting Started

This section will be updated as the project progresses. Currently, the focus is on setting up the foundational code structure.

### Prerequisites

*   Go 1.20+
*   Docker & Docker Compose
*   Kubernetes (e.g., Minikube, Kind, or a cloud provider's K8s service)
*   `make`
*   `protoc` (Protocol Buffer Compiler) and Go plugins (`protoc-gen-go`, `protoc-gen-go-grpc`)

### Installation & Setup (Conceptual)

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/turtacn/huizhu.ai.git
    cd huizhu.ai
    ```

2.  **Install Go dependencies:**
    ```bash
    go mod tidy
    ```

3.  **Generate Protobuf/gRPC code:**
    ```bash
    make generate-proto
    ```

4.  **Set up configuration:**
    *   Copy template configuration files from `configs/` (e.g., `cp configs/user_service.template.yaml configs/user_service.yaml`).
    *   Update the copied configuration files with your environment-specific settings (database DSNs, API keys, etc.).

5.  **Build services:**
    ```bash
    make build # Builds all services
    # or
    make build-user-service # Builds a specific service
    ```

6.  **Run services (locally using `make` or Docker Compose):**
    *   **Using `make` (for individual service development):**
        ```bash
        make run-user-service
        make run-apiserver
        # ... and so on for other services
        ```
    *   **Using Docker Compose (for a local multi-service environment - `docker-compose.yml` to be added):**
        ```bash
        docker-compose up -d
        ```

7.  **Deployment (Kubernetes - YAMLs in `deployments/kubernetes/`):**
    ```bash
    kubectl apply -f deployments/kubernetes/namespace.yaml
    kubectl apply -f deployments/kubernetes/postgres-deployment.yaml # Example dependency
    kubectl apply -f deployments/kubernetes/user-service-deployment.yaml
    # ... deploy other services and ingress
    ```

## 🛠️ Development

### Directory Structure

The project follows a layout inspired by the [Standard Go Project Layout](https://github.com/golang-standards/project-layout).

````

huizhu.ai/
├── api/             # API definitions (Protobuf, OpenAPI specs)
├── cmd/             # Main applications for each service
├── configs/         # Configuration file templates and defaults
├── deployments/     # Dockerfiles, Kubernetes YAMLs, etc.
├── docs/            # Project documentation
├── internal/        # Private application and library code (not importable by others)
│   ├── apiserver/   # API Gateway implementation
│   ├── biz/         # Business logic (domain, application services) for each microservice
│   ├── constant/    # Project-level constants
│   ├── data/        # Data access layer (repository implementations)
│   ├── server/      # Common gRPC/HTTP server setup
│   ├── types/       # Project-level shared data types (enums)
│   └── worker/      # Background job/task implementations
├── pkg/             # Public library code, shareable with external applications
│   ├── auth/
│   ├── cache/
│   ├── config/
│   ├── errors/
│   ├── logger/
│   ├── metrics/
│   ├── model/       # Common data models (e.g., pagination)
│   ├── queue/
│   ├── storage/
│   ├── tracing/
│   ├── transport/   # gRPC/HTTP transport helpers
│   └── utils/
├── scripts/         # Utility scripts (build, generate, run)
├── test/            # Test files (integration, E2E)
├── third\_party/     # Third-party helper tools, forked code
├── tools/           # Supporting tools for this project (e.g., code generators)
├── web/             # Frontend projects (React/Vue)
├── go.mod
├── go.sum
└── Makefile

````

### Coding Guidelines

*   Follow standard Go idioms and best practices.
*   Write clear, maintainable, and well-tested code.
*   Use `pkg/errors` for error handling.
*   Use `pkg/logger` for structured logging.
*   Adhere to the defined API contracts in `api/proto/`.
*   Write unit tests for business logic and data access layers.
*   Write integration tests for service interactions.

### Running Tests

```bash
make test         # Run all unit tests
make test-coverage # Run tests with coverage report
# make test-e2e     # (To be implemented) Run end-to-end tests
````

### Linting

```bash
make lint
```

(Requires `golangci-lint` to be installed)

## 🤝 Contributing

Contributions are welcome! As the project is in its early stages, the primary focus is on establishing the core architecture and functionality.

If you'd like to contribute, please:

1. **Fork the repository.**
2. **Create a new branch** for your feature or bug fix: `git checkout -b feature/your-feature-name` or `bugfix/issue-tracker-id`.
3. **Make your changes**, adhering to the coding guidelines.
4. **Add tests** for your changes.
5. **Ensure all tests pass** (`make test`).
6. **Ensure code is linted** (`make lint`).
7. **Commit your changes** with clear and descriptive commit messages.
8. **Push your branch** to your fork: `git push origin feature/your-feature-name`.
9. **Open a Pull Request** to the `main` branch of the original repository.

Please discuss significant changes by opening an issue first.

## 📄 License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.

## 📞 Contact

* **Project Lead**: \[Your Name/Organization] - \[[your.email@example.com](mailto:xudotu@gmail.com)]
* **GitHub Issues**: For bugs, feature requests, and discussions.

---

*This README is a living document and will be updated as the project evolves.*

