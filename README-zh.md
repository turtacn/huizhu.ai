# Huizhu.AI (慧助.AI)

[![许可证: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) [![文档](https://img.shields.io/badge/Docs-Huizhu.AI-orange)](docs/architecture.md)

**Huizhu.AI (慧助AI，原“智能业务伙伴”)** 是一个旨在通过人工智能驱动的解决方案，为企业客户的日常业务运营赋能的综合性平台。它将生成式人工智能（Generative AI）、智能运维（AIOps）和人工智能安全（AI Security）能力集成到一个统一的、对话式的交互界面中。

Huizhu.AI 的主要目标是通过自动化和增强各项业务职能中的任务，来提高运营效率、降低成本并改善决策制定。

## 客户痛点与价值主张

Huizhu.AI 致力于解决现代企业面临的若干关键挑战：

**客户痛点：**

* **运营效率低下**：企业在市场营销、客户服务、内部知识管理、IT 运营和安全等日常工作中，面临大量重复性、信息密集型任务。
* **缺乏专业工具**：许多公司缺乏专业的工具和 AI 助手，往往依赖昂贵的外包服务或高成本顾问。
* **营销与销售困境**：在专业的市场分析、创作引人入胜的营销内容以及获取及时的竞品洞察方面存在困难，导致营销活动效果不佳。
* **IT 复杂性与安全风险**：复杂的 IT 环境和频发的安全事件在缺乏专业人员的情况下难以管理，导致故障解决和风险缓解的延迟。
* **知识与协作障碍**：繁琐的内部流程、分散的文档以及低效的知识检索，阻碍了团队的生产力和协作效率。

**我们的价值主张：**

Huizhu.AI 提供一站式、对话式的智能业务运营支持平台，传递实实在在的价值：

* **慧营销 (Smart Marketing)**：
    * 自动化生成多样化的营销素材（文案、图片、短视频脚本）。
    * 一键式提供市场趋势报告和智能化的竞品分析。
    * 优化“数据 → 洞察 → 创意 → 营销活动投放”的完整生命周期。
* **慧运维 / 慧安全 (Smart Operations & Security)**：
    * 提供基于自然语言的 IT 故障诊断、日志关联分析以及可操作的根因分析。
    * 对安全事件进行智能解读，并提供清晰的处置建议。
    * 内置合规自查助手，用于生成报告和整改方案。
* **慧协同 (Smart Collaboration)**：
    * 基于企业自身的知识库实现智能问答。
    * 自动化文档和合同的比对与审查，高亮潜在风险。
    * 自动生成、归档和分发会议纪要。
    * 提供统一门户以追踪关键业务进展和管理待办事项。

## 主要功能特性

* **对话式 AI 助手**：通过自然语言交互访问平台所有功能。
* **统一业务门户**：一个集中的仪表盘，用于数据可视化、报告查阅和模块访问。
* **多场景营销支持**：AI 驱动的内容生成、市场分析和竞品洞察。
* **智能客户服务**：自动化应答、多轮对话支持，以及基于历史数据生成常见问题解答（FAQ）。
* **主动式 IT 运维**：对 IT 基础设施进行实时监控、自动告警和 AI 驱动的根因分析。
* **响应式 AI 安全**：自动检测安全事件（如异常登录、可疑流量），提供可操作的处置建议和合规报告。
* **增强型知识管理**：针对内部文档和会议记录的智能检索、文档比对和自动摘要。
* **多租户与权限控制**：企业客户间数据的安全隔离，以及精细化的基于角色的访问控制（RBAC）。

## 架构概览

Huizhu.AI 基于**微服务架构**构建，以确保可伸缩性、韧性和可维护性。关键架构层次包括：

* **用户交互层**：提供对话式 AI 助手和统一业务门户。
* **API 网关层**：作为所有客户端请求的单一入口。
* **核心智能服务层**：容纳主要的 AI 引擎（生成式 AI、AIOps、AI 安全、数据分析）。
* **领域服务层**：包含业务逻辑服务，如用户管理、知识库管理等。
* **基础设施层**：提供基础支持，如数据库、消息队列和可观测性工具。
* **数据与知识层**：管理 AI 模型训练和推理所需的各种数据源。

所有服务均设计为可容器化并通过 Kubernetes 进行编排。

欲了解系统架构、组件、设计决策和技术栈的详细信息，请参阅 [**Huizhu.AI 架构设计文档 (`docs/architecture.md`)**](docs/architecture.md)。

## 技术栈 (重点)

* **后端**：Go (Golang)
* **API 与通信**：gRPC (服务间)、RESTful API (通过 API 网关)、WebSockets
* **编排**：Kubernetes, Docker
* **数据库**：PostgreSQL/MySQL、Elasticsearch、Prometheus/Mimir、向量数据库 (如 Qdrant, Milvus)、MinIO/S3 兼容存储
* **消息队列**：NATS / Kafka / RabbitMQ
* **可观测性**：Prometheus, Grafana, OpenTelemetry, ELK Stack (或类似方案)
* **AI/ML**：LangChain (Go 版本)、Hugging Face 模型、OpenAI API 集成。

## 快速开始

本节将指导您完成 Huizhu.AI 的开发环境搭建。

### 环境准备

* **Go**：版本 1.21 或更高。
* **Docker & Docker Compose**：用于在本地管理服务和依赖。
* **Kubernetes 集群**：可选，用于部署 (例如 Minikube, Kind)。
* **`protoc` 编译器**：用于编译 Protocol Buffer 定义。
    * `protoc-gen-go`
    * `protoc-gen-go-grpc`

### 克隆仓库

```bash
git clone [https://github.com/turtacn/huizhu.ai.git](https://github.com/turtacn/huizhu.ai.git)
cd huizhu.ai
````

### 配置

1.  每个服务从 YAML 文件 (例如 `configs/defaults.yaml`, `configs/user_service.example.yaml`) 和环境变量加载配置。
2.  将默认/示例配置文件复制到您的本地配置 (例如 `configs/user_service.dev.yaml`)，并更新必要的参数，如数据库连接字符串、LLM API 密钥等。

### 构建

1.  **生成 gRPC Stub 代码**：如果修改了 `.proto` 文件：
    ```bash
    ./scripts/gen_proto.sh
    ```
2.  **构建服务**：
    ```bash
    ./scripts/build.sh
    ```
    或构建单个服务 (例如 `user_service`)：
    ```bash
    go build -o ./bin/user_service ./cmd/user_service/main.go
    ```
3.  **构建 Docker 镜像** (以 `user_service` 为例)：
    ```bash
    docker build -f deployments/dockerfiles/Dockerfile.user_service -t turtacn/huizhu-user-service:latest .
    ```

### 本地运行 (开发)

1.  **使用 Docker Compose** (推荐，需要 `docker-compose.yml` 文件)：
    ```bash
    docker-compose up -d
    ```
2.  **手动运行单个服务**：
    确保依赖项已运行。设置必要的环境变量。
    ```bash
    ./bin/user_service
    # 或: go run ./cmd/user_service/main.go
    ```

### 在 Kubernetes 上运行

1.  将 Docker 镜像推送到您的 K8s 集群可访问的容器镜像仓库。
2.  应用 `deployments/kubernetes/services/` 目录下的 manifests 文件：
    ```bash
    kubectl apply -f deployments/kubernetes/services/user-service.yaml
    # ... 以此类推，部署其他服务
    ```

## 运行测试

```bash
./scripts/run_tests.sh
# 或:
# go test -v ./...
```

## 项目结构

  * `api/proto/`: Protocol Buffer 定义。
  * `cmd/`: 各微服务的主程序入口。
  * `configs/`: 配置文件和加载逻辑。
  * `deployments/`: Dockerfile 和 Kubernetes manifests。
  * `docs/`: 项目文档，包括[架构设计文档](https://www.google.com/search?q=docs/architecture.md)。
  * `internal/`: 项目内部私有代码。
  * `pkg/`: 可被外部项目导入的公共库 (例如客户端 SDK)。
  * `scripts/`: 辅助脚本。
  * `test/`: 集成测试和端到端测试。

更多详情，请参阅[架构设计文档](https://www.google.com/search?q=docs/architecture.md)。

## 贡献指南

我们欢迎对 Huizhu.AI 的任何贡献！无论是修复错误、开发新功能还是改进文档，您的帮助都至关重要。

请阅读我们的 [**贡献指南 (`CONTRIBUTING.md`)**](https://www.google.com/search?q=CONTRIBUTING.md) (待创建)，其中包含关于开发流程、编码标准以及如何提交拉取请求的详细信息。

请使用 GitHub **Issue Tracker** 报告错误和建议功能。

## 许可证

本项目基于 **Apache License 2.0** 许可证授权。详情请参阅 `LICENSE` 文件 (待创建)。

## 联系与支持

  * **问题反馈**: 对于错误、功能请求或遇到的问题，请在 [GitHub 上提交 Issue](https://www.google.com/search?q=https://github.com/turtacn/huizhu.ai/issues)。
  * **讨论**: 对于一般性问题或讨论，请使用 [GitHub Discussions 标签页](https://www.google.com/search?q=https://github.com/turtacn/huizhu.ai/discussions) (如果已启用)。
