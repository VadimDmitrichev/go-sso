# Go SSO | gRPC Identity Provider

![Go Version](https://img.shields.io/badge/Go-1.21-00ADD8?style=for-the-badge&logo=go)
![gRPC](https://img.shields.io/badge/gRPC-v1.58.1-244c5a?style=for-the-badge&logo=grpc)
![SQLite](https://img.shields.io/badge/SQLite-Database-003B57?style=for-the-badge&logo=sqlite)
![CI/CD](https://img.shields.io/badge/GitHub_Actions-Deployed-2088FF?style=for-the-badge&logo=github-actions)

A production-ready Single Sign-On (SSO) authentication microservice built in Go. Designed with Clean Architecture principles, it provides a centralized, secure gRPC API for user identity management, JWT generation, and application-specific access control.

## Key Features

* **gRPC First:** High-performance binary transport using Protocol Buffers.
* **Multi-Tenant JWT:** App-specific token generation (each registered client app gets its own secret for token validation).
* **Resilient Infrastructure:** Built-in gRPC interceptors for global panic recovery and structured logging.
* **Clean Architecture:** Strict separation of concerns (Transport → Service → Storage).
* **Secure by Design:** `bcrypt` password hashing and robust token TTL management.
* **Zero-Downtime Ready:** Implemented OS signal capturing for Graceful Shutdown.
* **Automated CI/CD:** GitHub Actions pipeline for building, testing, and automated VM deployment via SSH/systemd.

## Architecture

```mermaid
graph TD
    Client1[Microservice / API Gateway] -->|gRPC| AuthServer
    Client2[Web Backend] -->|gRPC| AuthServer
    
    subgraph SSO Service [go-sso]
        AuthServer[gRPC Server & Interceptors] --> AuthService[Auth Business Logic]
        AuthService --> JWT[JWT Generator]
        AuthService --> Bcrypt[Password Hasher]
        AuthService --> Storage[Storage Interface]
    end
    
    Storage --> SQLite[(SQLite DB)]