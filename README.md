# 🌐 Spring Boot Static Website on Kubernetes

A production-style setup for deploying a Spring Boot application with a Thymeleaf frontend on Kubernetes using Docker. This project includes CI/CD (optional Jenkins), namespace-based isolation, metrics via Spring Actuator, and external access via NodePort.

---

## 📦 Tech Stack

- Java 17 + Spring Boot 2.2
- Thymeleaf templating
- Docker
- Kubernetes (Minikube / Docker Desktop)
- Jenkins (optional CI/CD)


---

## 📁 Directory Structure

```
spring-using-kubernetes/
├── Dockerfile                 # Docker build config
├── Jenkinsfile                # Jenkins CI/CD pipeline
├── kuber-deployment.yml      # Kubernetes Deployment + Service
├── pom.xml                   # Maven configuration
├── src/
│   ├── main/
│   │   ├── java/com/siddu/webapp/
│   │   │   └── SpringBootWebApplication.java
│   │   └── resources/
│   │       ├── templates/    # HTML (index.html, etc.)
│   │       ├── static/       # CSS/JS/image assets
│   │       └── application.properties
│   └── test/java/...
```

---

## 🚀 Features

- Spring Boot backend with static frontend
- Docker container built via `Dockerfile`
- Kubernetes deployment with isolated namespace
- NodePort-based access to app
- Jenkinsfile included for CI/CD pipeline
- Health and metrics via `/actuator/*` endpoints

---

## 🔧 Prerequisites

- Java 11+
- Maven
- Docker
- Kubernetes (`minikube`, `kubectl`)
- (Optional) Jenkins server

---

## 🧰 Setup Guide

### 1. Clone the Project

```bash
git clone https://github.com/im-faix/spring-using-kubernetes.git
cd spring-using-kubernetes
```

---

### 2. Build Spring Boot Project

```bash
./mvnw clean package
```

---

### 3. Build Docker Image

```bash
docker build -t faixan01/spring-webapp:5 .
```

---

### 4. Create Kubernetes Namespace

```yaml
# apply directly using:
kubectl apply -f - <<EOF
apiVersion: v1
kind: Namespace
metadata:
  name: webapp
EOF
```

Or create using CLI:

```bash
kubectl create namespace webapp
```

---

### 5. Deploy to Kubernetes

```bash
kubectl apply -f kuber-deployment.yml
```

---

### 6. Check Resources

```bash
kubectl get all -n webapp
```

---

### 7. Access the App in Browser

```bash
minikube service spring-webapp-service -n webapp
```

Or use:

```
http://<minikube-ip>:30036
```

You should see the rendered HTML UI via Thymeleaf (`index.html`).

---

## 📄 application.properties Example

```properties
server.port=9090
spring.application.name=spring-webapp

management.endpoints.web.exposure.include=*
management.endpoint.health.show-details=always
```

---

## 📊 Spring Actuator Endpoints

```bash
curl http://localhost:9090/actuator/health
curl http://localhost:9090/actuator/prometheus
```

---




## 🧪 Run Tests

```bash
./mvnw test
```

---

## ⚙️ Kubernetes Manifest Explained (`kuber-deployment.yml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spring-webapp-deployment
  namespace: webapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: spring-webapp
  template:
    metadata:
      labels:
        app: spring-webapp
    spec:
      containers:
        - name: spring-webapp
          image: faixan01/spring-webapp:5
          ports:
            - containerPort: 9090
---
apiVersion: v1
kind: Service
metadata:
  name: spring-webapp-service
  namespace: webapp
spec:
  type: NodePort
  selector:
    app: spring-webapp
  ports:
    - port: 9090
      targetPort: 9090
      nodePort: 30036
```

---

## 🧼 Cleanup Resources

```bash
kubectl delete namespace webapp
```

---

## 🎯 What You’ll Learn

- How to build a full-stack Spring Boot web app
- Dockerizing with custom `Dockerfile`
- Running inside Kubernetes namespace with NodePort
- Exposing health and metrics with Spring Actuator
- Automating with Jenkins pipelines

---

## 📝 License

This project is licensed under the [MIT License](./LICENSE).  
Feel free to use, modify, and distribute for personal or commercial purposes.
© 2025 [Mohammed Faizan](https://github.com/im-faix)
