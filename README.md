# Employee Management System

A microservices-based Employee Management System built with Spring Boot, featuring separate services for employee management, department management, and authentication.

## System Architecture

### Microservices
1. **Employee Service** (Port: 8081)
   - Employee CRUD operations
   - Employee search and filtering
   - Reporting functionality

2. **Department Service** (Port: 8082)
   - Department CRUD operations
   - Department hierarchy management
   - Budget tracking

3. **Authentication Service** (Port: 8083)
   - User authentication
   - Role-based access control
   - JWT token management

4. **API Gateway** (Port: 8080)
   - Request routing
   - Load balancing
   - Rate limiting

### Technical Stack
- Backend: Spring Boot 3.x
- Database: MySQL
- Security: Spring Security with JWT
- Documentation: SpringDoc OpenAPI
- Build Tool: Maven
- Container: Docker
- Orchestration: Kubernetes
- Service Discovery: Eureka
- Circuit Breaker: Resilience4j

## Local Development Setup

### Prerequisites
- JDK 17 or later
- Maven 3.8+
- Docker and Docker Compose
- MySQL 8.0+

### Building the Application

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd employee-management
   ```

2. **Build all services**
   ```bash
   ./mvnw clean package -DskipTests
   ```

3. **Build Docker images**
   ```bash
   docker-compose build
   ```

### Running Locally


1. **Using Maven (for development)**
   ```bash
   # Start services individually
   cd employee-service
   ./mvnw spring-boot:run

   cd ../department-service
   ./mvnw spring-boot:run

   cd ../auth-service
   ./mvnw spring-boot:run

   cd ../api-gateway
   ./mvnw spring-boot:run
   ```

### Running Tests
```bash
./mvnw clean verify
```

## Deployment

### Kubernetes Deployment

1. **Create Kubernetes secrets**
   ```bash
   kubectl create secret generic mysql-credentials \
     --from-literal=username=root \
     --from-literal=password=your-password
   ```

2. **Apply Kubernetes configurations**
   ```bash
   kubectl apply -f k8s/
   ```


## API Documentation

After starting the services, access the Swagger UI:
- Employee Service: http://localhost:8081/swagger-ui.html
- Department Service: http://localhost:8082/swagger-ui.html
- Auth Service: http://localhost:8083/swagger-ui.html


## Database Schema

### Employee Service
```sql
CREATE TABLE employees (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    department_id BIGINT,
    position VARCHAR(100),
    salary DECIMAL(10,2),
    hire_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### Department Service
```sql
CREATE TABLE departments (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    manager_id BIGINT,
    parent_department_id BIGINT,
    budget DECIMAL(15,2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### Auth Service
```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(200) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    role VARCHAR(20) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```


## License

This project is licensed under the MIT License - see the LICENSE file for details.

### Minimum Tasks (Infra Creation)
- Provision infrastructure either on self managed K8s / EKS using terraform 
- Set up a simple GitHub Actions or Jenkins pipeline to run Terraform commands (terraform fmt, validate, and plan).
- Automate Terraform execution with a pipeline trigger on push to a specific branch.
- Create Dockerfiles 
- Set up multi-stage builds
- push docker file to docker repository
- Configure environment variables
- Set up health check endpoints
- Configure basic logging

### Target Tasks (Deployment)
- Refactor the Terraform code to use modules for reusability.
- Store Terraform state in an S3 bucket with DynamoDB for state locking.
- Write Deployment.yaml and service.yaml files for deployment on k8s.
- Use helm for deplyment using gitops
- Handle environment variables using k8s secrets

### Stretch 
- Setup Argo CD for automatic deplyoment 
- Setup Promethues and grafana 
- setup basic alerts