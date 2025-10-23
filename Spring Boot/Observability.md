**Three Pillars of Observability**

1. Logging: Detailed information about individual things that are ongoing in your application (printing logs for an application)
2. Metrics: Aggregated information about application features, over a period of time such as application's CPU, network & memory.
3. Tracing: Sampled information across multiple services (how long each service call took. A single trace displays the operation as it moves from one node to another in a distributed system)

---

**Problem with basic logging**

1. Doesn't Scale
  - Too much work and knowledge required
  - Logs can pile up and consume your system

2. Low Usability
  - Search is limited/difficult
  - In concurrent environment, intermixed logs from same request
  - Process of reconstructing transactions is tedious but possible

---

**Centralized logging**

<img width="800" height="350" alt="image" src="https://github.com/user-attachments/assets/e5581566-70dd-4dff-bfd0-d9ae74e6ee55" />

---

**Spring Boot Logging**

<img width="650" height="500" alt="image" src="https://github.com/user-attachments/assets/2c3e8146-87cc-4a98-9c7a-1c23f03ee6a6" />

Centralized Logging Tools: Tools like Spring Boot Actuator, Spring Boot Admin, and Spring Cloud Config help manage and monitor log levels and formats across multiple Spring applications. Spring Cloud Sleuth adds trace IDs to logs for better correlation across services.

---
