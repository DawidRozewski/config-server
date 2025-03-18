# Config Server

This is the centralized configuration server for all microservices in this project. By default, it serves configuration properties from the local **/config** directory.
Each microservice fetches its configuration from this repository dynamically.

# 🏗️ About This Project

This project is designed as a **skeleton for microservices architecture**, providing a foundation for learning, testing new solutions, and experimenting with different approaches to software design. The focus is on demonstrating how microservices interact rather than delivering a full production-ready system.

## 🔍 Purpose

This project is intended for:
- **Testing new configurations and refactoring code**
- **Experimenting with different architectural patterns**
- **Understanding how microservices communicate in a distributed system**
- **Enhancing observability using monitoring tools like Prometheus and Grafana**

## ⚡ Get Started

Feel free to explore, modify, and improve the project to fit your needs! 🚀

## 🛠️ Technology Stack

- **Java 21**
- **Spring Boot 3.4.3**
- **Spring Cloud Config Server 2024**
- **Maven 3.14.0**
- **Docker**

## 📌 Microservices Using This Config Server

The following microservices retrieve their configurations from this server:

1. **Eureka Server** - Service discovery.
2. **Kafka Producer Service** - Produces messages to Kafka.
3. **Kafka Consumer Service** - Consumes messages from Kafka.
4. **API Gateway Service** - Routes requests and provides authentication.

## 🚀 Running the Config Server

```sh
mvn spring-boot:run
```

It will be available at: [http://localhost:8888](http://localhost:8888)

## 📋 Order of Microservice Startup

To avoid issues, **you must start the supporting services first** using `docker-compose`.  
Then, follow this order:

1️⃣ **Start Supporting Services (`docker-compose` must be running! ⬇️)**  
2️⃣ **Config Server** (must be first)  
3️⃣ **Eureka Server** (service discovery)  
4️⃣ **Kafka Producer Service**  
5️⃣ **Kafka Consumer Service**  
6️⃣ **API Gateway Service**

## 🐳 Running Supporting Services (`docker-compose`)

Before running microservices, **you must start the required services** with:

```sh
docker-compose up -d
```

This will start the following containers:

- **Keycloak** (authentication and authorization)
- **Kafka** (message broker)
- **Kafka-UI** (Kafka management UI)
- **Prometheus** (metrics collection)
- **Grafana** (monitoring dashboards)
- **MongoDB** (NoSQL database)

### 📊 Preconfigured Monitoring & Alerts

✅ **Prometheus** comes with **example alerts** defined in `alert.rules.yml`.  
✅ **Grafana** automatically **imports a preconfigured dashboard** for monitoring Prometheus metrics.

📢 **Explore and customize them to fit your needs!** 🚀


### 💡 But Why Isn’t docker-compose used for Microservices?

This project is intentionally not configured to run microservices with docker-compose.

Why? Because it’s meant to be a learning experience. Instead of providing a ready-made solution, you’re encouraged to configure it yourself.

If you want a challenge, try adding microservices to `docker-compose.yml` yourself! 🚀

### 🧐 Things to Consider:
- **How can we ensure microservices start in the correct order when using `docker-compose.yml`?**
- **What if one of the services fails to start?**
- **How would you handle configuration changes dynamically without restarting services?**


## 🔄 Refactoring Suggestions

Looking to enhance this project? Here are some key areas for improvement:

1. **Enhance Security for the Config Server**
   - Implement authentication and authorization to **restrict access to sensitive configurations**.
   - Consider using **Spring Security with OAuth2 or Keycloak** for centralized access control.

2. **Introduce Environment-Specific Configuration**
   - Utilize **separate Git branches or repositories** for `dev`, `staging`, and `prod` environments.
   - This ensures a **clear separation of configurations** and minimizes deployment risks.

3. **Implement a Failover Strategy**
   - Add **redundancy and caching mechanisms** to handle scenarios where the Git repository becomes unavailable.
   - Consider using **Spring Cloud Config’s failover options** or a **fallback file-based configuration**.

