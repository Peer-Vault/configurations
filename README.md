# Peer Vault - Configurations

## 🚀 Overview
This repository serves as the centralized store for all configuration files (`.yml`) used by the Peer Vault microservices. It is designed to be consumed by the **Config Server**, which then distributes these configurations dynamically to individual services like User Service, File Service, Notification Service, and Gateway Server.

---

## ✨ Purpose
The primary purpose of this repository is to provide a single source of truth for application configurations, enabling:
* **Centralized Management:** All service configurations are managed in one place.
* **Dynamic Updates:** Changes pushed to this repository can be dynamically refreshed by the Config Server (if configured), allowing for zero-downtime configuration updates.
* **Environment-Specific Configurations:** Supports different configurations for various environments (e.g., `application-dev.yml`, `application-prod.yml` or service-specific profiles).
* **Version Control:** All configuration changes are tracked via Git, providing a complete history and rollback capabilities.

---

## 🏗 Repository Structure
The repository is expected to contain `.yml` files, with each file typically corresponding to a specific microservice or a shared configuration.

Example structure:

configurations/
├── application.yml                 # Global default configurations (optional)
├── user-service.yml                # Configuration for the User Service
├── file-service.yml                # Configuration for the File Service
├── notification-service.yml        # Configuration for the Notification Service
├── gateway-server.yml              # Configuration for the Gateway Server
└── ... (other service-specific or profile-specific files)


Each `.yml` file contains Spring Boot properties specific to that application or profile.

---

## 🎯 Usage
1.  **Config Server Integration:** The Peer Vault **Config Server** is configured to clone and read configurations from this GitHub repository.
    * Ensure the `spring.cloud.config.server.git.uri` property in the Config Server's `application.yml` points to this repository's URL: `https://github.com/Peer-Vault/configurations.git`.

2.  **Microservice Consumption:** Individual Peer Vault microservices (e.g., User Service, File Service) are configured to fetch their configurations from the Config Server upon startup.
    * Each microservice's `bootstrap.yml` or `application.yml` should include:
        ```yaml
        spring:
          config:
            import: optional:configserver:http://localhost:8071/ # Adjust port if Config Server runs on a different port
        ```
    * The `spring.application.name` property in each microservice's `application.yml` should match the name of its corresponding `.yml` file in this repository (e.g., `userservice` for `user-service.yml`).

3.  **Updating Configurations:**
    * To update a configuration, modify the relevant `.yml` file in this repository.
    * Commit and push your changes to the `main` branch (or `default-label` configured in the Config Server).
    * The Config Server will pick up these changes. For dynamic refresh, microservices might need to expose `/actuator/refresh` endpoint and be triggered.

---

## 🤝 Contributing
Contributions and suggestions for improving configuration management or adding new service configurations are welcome! Feel free to open issues or submit pull requests.

 
---

## 👨‍💻 Author
Developed and maintained by the Peer Vault team.

---

🌟 Star this repo if you find it helpful! 🌟
