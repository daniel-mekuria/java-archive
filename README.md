# java-archive

A Java backend service handling verification, data persistence, and API contracts. Built with Spring Boot, Maven, and PostgreSQL.

## Overview

A legacy enterprise backend service that provides verification workflows, REST APIs, and database persistence. Originally built as a verification server, now maintained as part of the archived infrastructure.

## Structure

```
verification-server-api/      - API contracts and DTOs
verification-server-service/  - Business logic and service layer
verification-server-boot/     - Spring Boot application bootstrap
plugins/                      - Plugin modules
sql/                          - Database migrations and schemas
pom.xml                       - Maven parent POM
docker-compose.yml            - Development environment
```

## Features

- **RESTful API** — versioned endpoints with OpenAPI documentation
- **Persistence layer** — JPA/Hibernate with PostgreSQL
- **Authentication** — OAuth2 resource server with JWT validation
- **Audit logging** — immutable audit trail for all state changes
- **Plugin system** — extensible verification strategies via plugins
- **Database migrations** — versioned SQL scripts for schema evolution

## Getting Started

### Prerequisites

- Java 17+
- Maven 3.8+
- PostgreSQL 14+

### Build

```bash
./mvnw clean package -DskipTests
```

### Run

```bash
# Start dependencies
docker-compose up -d

# Run the application
./mvnw spring-boot:run
```

The API will be available at `http://localhost:8080`.

### Database setup

```bash
# Apply migrations
psql -f sql/001_init_schema.sql
psql -f sql/002_add_verification_tables.sql
```

## Configuration

```yaml
# application.yml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/verification
    username: app
    password: ${DB_PASSWORD}
  jpa:
    hibernate:
      ddl-auto: validate

security:
  oauth2:
    resourceserver:
      jwt:
        jwk-set-uri: ${JWK_SET_URI}
```

## API Endpoints

| Method | Path | Description |
|---|---|---|
| `POST` | `/api/v1/verifications` | Create a verification request |
| `GET` | `/api/v1/verifications/{id}` | Get verification status |
| `POST` | `/api/v1/verifications/{id}/approve` | Approve a verification |
| `GET` | `/api/v1/audit` | Query audit log |

## Development

```bash
./mvnw test              # run tests
./mvnw verify            # run integration tests
./mvnw dependency:tree   # inspect dependencies
```

## License

See `LICENSE`
