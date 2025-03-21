# Config Server

This is the centralized configuration server for all microservices in this project. By default, it serves configuration properties from the local **/config** directory.
Each microservice fetches its configuration from this repository dynamically.

---

# 🏗️ About This Project

This project is designed as a **skeleton for microservices architecture**, providing a foundation for learning, testing new solutions, and experimenting with different approaches to software design. The focus is on demonstrating how microservices interact rather than delivering a full production-ready system.

---

## 🔍 Purpose

This project is intended for:
- **Testing new configurations and refactoring code**
- **Experimenting with different architectural patterns**
- **Understanding how microservices communicate in a distributed system**
- **Enhancing observability using monitoring tools like Prometheus and Grafana**

---

## 🛠️ Technology Stack

- **Java 21**
- **Spring Boot 3.4.3**
- **Spring Cloud Config Server 2024.0.0**
- **Maven 3.14.0**
- **Docker**

## 🛠️ Infrastructure Stack (Docker)
- **Keycloak**
- **Kafka**
- **Kafka UI**
- **Prometheus**
- **Grafana**
- **MongoDB**
---

## 📌 Microservices Interactions

The following microservices retrieve their configurations from this server:

1. **[Eureka Server](https://github.com/DawidRozewski/eureka-server)** - Service discovery.
2. **[Kafka Producer Service](https://github.com/DawidRozewski/kafka-producer-service)** - Produces messages to Kafka.
3. **[Kafka Consumer Service](https://github.com/DawidRozewski/kafka-consumer-service)** - Consumes messages from Kafka.
4. **[API Gateway Service](https://github.com/DawidRozewski/api-gateway-service)** - Routes requests and provides authentication.

---

## 🚀 Running the Config Server

```sh
mvn spring-boot:run
```

It will be available at: [http://localhost:8888](http://localhost:8888)

---

## 📋 Order of Microservice Startup

To avoid issues, **you must start the supporting services first** using `docker-compose`.  
Then, follow this order:

1️⃣ **Start Supporting Services (`docker-compose` must be running! ⬇️)**  
The following services interact with this Kafka producer:
1. **[Config Server](https://github.com/DawidRozewski/config-server)** 
2. **[Eureka Server](https://github.com/DawidRozewski/eureka-server)** 
3. **[Kafka Producer Service](https://github.com/DawidRozewski/kafka-producer-service)** 
4. **[Kafka Consumer Service](https://github.com/DawidRozewski/kafka-consumer-service)**
5. **[API Gateway Service](https://github.com/DawidRozewski/api-gateway-service)** 

---

## 🐳 Running Supporting Services (`docker-compose`)

Before running microservices, **you must start the required services** with:

```sh
docker-compose up -d
```

This will start the following containers:

| Service      | Description                          | URL                                                                   |
|-------------|--------------------------------------|-----------------------------------------------------------------------|
| **Keycloak** | Authentication and authorization   | [http://localhost:8085](http://localhost:8085) (Admin: `admin/admin`) |
| **Kafka** | Message broker                        | `localhost:9092` (internal)                                           |
| **Kafka-UI** | Kafka management UI                 | [http://localhost:8086](http://localhost:8086)                        |
| **Prometheus** | Metrics collection                 | [http://localhost:9090](http://localhost:9090)                        |
| **Grafana** | Monitoring dashboards               | [http://localhost:3000](http://localhost:3000) (Admin: `admin/admin`) |
| **MongoDB** | NoSQL database                       | `localhost:27017` (internal)                                          |

---

### 📊 Preconfigured Monitoring & Alerts

✅ **Prometheus** comes with **example alerts** defined in `alert.rules.yml`.  
✅ **Grafana** automatically **imports a preconfigured dashboard** for monitoring Prometheus metrics.

💡 I have prepared this as a baseline for monitoring microservices, so you can immediately see metrics and alerts in action. 
Feel free to explore and adjust the configuration to match your needs! 🎯

---

### 💡 But Why Isn’t docker-compose used for Microservices?

This project is intentionally not configured to run microservices with docker-compose.

Why? Because it’s meant to be a learning experience. Instead of providing a ready-made solution, you’re encouraged to configure it yourself.

If you want a challenge, try adding microservices to `docker-compose.yml` yourself! 🚀

### 🧐 Things to Consider:
- **How can we ensure microservices start in the correct order when using `docker-compose.yml`?**
- **What if one of the services fails to start?**
- **How would you handle configuration changes dynamically without restarting services?**

---

## 🔧 Configuration Server Setup

By default, the **Config Server** fetches configuration files from the following Git repository:  
🔎 [https://github.com/DawidRozewski/config-server](https://github.com/DawidRozewski/config-server)

However, you have multiple options to configure it according to your needs:

### 1️⃣ **Use the default configuration (recommended)**
Simply clone and run the project. The Config Server will fetch configuration files from the predefined Git repository.

### 2️⃣ **Use your own Git repository**
If you want full control over configuration, fork the existing repository or create a new one. Then, update `application.yml`:

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://your-git-repo-url.git
          clone-on-start: true
          default-label: main
          search-paths: demo/config
```

### 3️⃣ **Use local configuration files**
Instead of fetching files from Git, you can load them from your local machine. Modify `application.yml` and switch to `native` profile:

```yaml
spring:
  profiles:
    active: native
  cloud:
    config:
      server:
        native:
          search-locations: file:///{absolute-path-to-your-local-config-folder}
```

Then, start the Config Server with:

```sh
mvn spring-boot:run -Dspring-boot.run.profiles=native
```

### 4️⃣ **Disable Config Server (not recommended)**
If you want to run microservices **without** a Config Server, disable the external configuration by setting:

```yaml
spring.cloud.config.enabled=false
```

> ⚠ **Warning:** This is **not recommended**, as services will require manual configuration in their `application.yml` files.

---

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

4. **Is JSON the Best Serialization Format for Kafka?**
   - Currently, messages are serialized using JsonSerializer and JsonDeserializer.
   - Could AvroSerializer and AvroDeserializer be a better alternative?
   - What are the benefits of using Avro over JSON?
💡 Think about message size, schema evolution, and cross-language compatibility!
   
---

📢 **This project is a microservices skeleton meant for learning, experimentation, and testing new ideas.**  
Feel free to modify and extend it to fit your needs! 🚀
