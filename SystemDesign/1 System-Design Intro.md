# System-Design
System design refers to the phase in the software development life cycle where the overall structure and architecture of a software system are planned and defined. This phase comes after the requirements analysis and involves translating the high-level requirements into a detailed design that can be used for implementation.
Key aspects of system design in software engineering include- 

1. **Architecture Design**
2. **Data Design**
3. **Interface Design**
4. **Component-level Design**
5. **Security Design**
   
The output of the system design phase is typically a detailed design document or set of documents that serve as a guide for the implementation team. This document **provides a roadmap for developers to follow** when building the software system, ensuring that **it meets the specified requirements and is scalable, maintainable, and robust**.


 
 ### Concepts to Understand

| **Topic**                                                | **Short Description**                                                                                                                                      |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Browser**                                              | Client application used to request, render, and interact with web pages via HTTP/HTTPS.                                                                    |
| **URL**                                                  | Uniform Resource Locator – an address that identifies resources on the internet.                                                                           |
| **DNS Resolution**                                       | Process of translating a domain name (e.g., google.com) into its corresponding IP address.                                                                 |
| **Proxy – Forward vs Reverse**                           | Forward proxy sits between client and server (hides client), reverse proxy sits in front of servers (hides backend, adds load balancing/security).         |
| **API Gateway**                                          | Entry point for APIs in microservices, handling routing, authentication, rate-limiting, and monitoring.                                                    |
| **Firewalls**                                            | Security systems that control incoming/outgoing network traffic based on rules.                                                                            |
| **Security – Authentication/Authorization, SSO, Tokens** | Authentication verifies identity, authorization checks permissions, SSO enables one login across apps, tokens (JWT, OAuth) enable secure stateless access. |
| **Load Balancer**                                        | Distributes traffic across multiple servers to improve availability and scalability.                                                                       |
| **Rate Limiting**                                        | Restricts number of requests per user/service to prevent abuse.                                                                                            |
| **Throttling**                                           | Intentionally slowing down requests when usage exceeds limits.                                                                                             |
| **Load Shedding**                                        | Dropping low-priority traffic under heavy load to protect system stability.                                                                                |
| **Monolith vs Microservice**                             | Monolith = single large codebase, Microservice = small independent services communicating over APIs.                                                       |
| **Service Registry & Discovery**                         | Mechanism for microservices to register themselves and discover other services dynamically.                                                                |
| **Databases – SQL, NoSQL, Vector**                       | SQL = relational structured data, NoSQL = unstructured/scalable, Vector DB = optimized for AI/semantic search.                                             |
| **Transactions**                                         | A unit of work in databases ensuring ACID (atomicity, consistency, isolation, durability).                                                                 |
| **Caching**                                              | Storing frequently accessed data in memory to reduce latency and load.                                                                                     |
| **CDN**                                                  | Content Delivery Network – distributes cached content across global servers for faster delivery.                                                           |
| **Webservices – REST, GraphQL, gRPC**                    | REST = resource-based, GraphQL = flexible queries, gRPC = high-performance binary RPC.                                                                     |
| **Message Queues**                                       | Middleware (Kafka, RabbitMQ) enabling async communication between services.                                                                                |
| **Distributed Systems**                                  | Systems with multiple nodes working together, appearing as a single coherent system.                                                                       |
| **Merkle Tree**                                          | Hash-based tree data structure used in blockchain and distributed systems for integrity verification.                                                      |
| **Consistent Hashing**                                   | Hashing strategy to evenly distribute data across nodes while minimizing rebalancing on node changes.                                                      |
| **Hinted Handoff**                                       | Temporary storage of writes by a replica when the intended node is unavailable (Cassandra technique).                                                      |
| **Big Data**                                             | Handling extremely large datasets using distributed processing (Hadoop, Spark).                                                                            |
| **Data Analytics**                                       | Process of extracting insights and patterns from raw data.                                                                                                 |
| **AI/ML**                                                | Artificial Intelligence and Machine Learning – systems that learn and make predictions from data.                                                          |
| **Generative AI**                                        | AI models that generate content (text, images, audio, code) rather than just analyzing data.                                                               |
| **LLM**                                                  | Large Language Models – advanced generative AI models trained on huge text corpora (e.g., GPT).                                                            |
| **Deployment Strategies**                                | Methods like Blue-Green, Canary, Rolling updates to release software with minimal downtime.                                                                |
| **Physical Datacenters**                                 | On-premises infrastructure hosting servers, networking, and storage.                                                                                       |
| **Cloud**                                                | On-demand infrastructure and services (IaaS, PaaS, SaaS) delivered over the internet.                                                                      |
| **Containers**                                           | Lightweight, isolated runtime environments (Docker, Kubernetes) for running applications.                                                                  |
| **CI/CD**                                                | Continuous Integration and Continuous Deployment – automating build, test, and release pipelines.                                                          |
| **Code Quality**                                         | Practices (linting, testing, reviews) ensuring reliable and maintainable software.                                                                         |
| **Observability**                                        | Ability to understand system state through logs, metrics, and traces.                                                                                      |
| **Performance Monitoring & Testing**                     | Tools and processes to measure system performance under load (JMeter, APM).                                                                                |
| **SLA Numbers (Nines)**                                  | Service Level Agreements represented as availability (e.g., 99.9% = 8h downtime/year).                                                                     |
| **Capacity Estimation**                                  | Predicting infrastructure/resources needed to handle expected traffic and workloads.                                                                       |


### Basic Concepts
* [[PACELC Theorem]]
* [[Scaling From 0 to 1 Billion]]
* [[SOLID]]
* [[GoF Design Patterns]]
* [[Common Patterns used in Cloud Systems]]
* [[Common Patterns used in Distributed systems]]
  
 * [Scaling from 0 to Billions of requests](https://youtube.com/playlist?list=PLq3uEqRnr_2H2B9kK2g9-7_-rn2uXMdRa&si=iqN8eMmBr53tUai0)
 * HLD vs LLD
 * How to architect for a greener future - [ref](https://www.infoq.com/articles/architect-software-for-greener-future/)
 * Read your writes: https://lnkd.in/dJYZ3Z_7

 * Asynchronous Processing with Futures: https://lnkd.in/d55-j-G8

 * Request Collapsing: https://lnkd.in/dFbJmwJC

 * What are threads: https://lnkd.in/dF4q9ez2

 * Ordering requests with threads: https://lnkd.in/dV_fqwW8

 * Event Bus Publishers and Subscribers: https://lnkd.in/dcciJ4vM

 *  What is a software client library: https://lnkd.in/dbYzNXeH

 *  Classes and package structuring: https://lnkd.in/dqagPDG9

 *  Dependency Inversion: https://lnkd.in/djU2uw4K

 * What is backpressure: https://lnkd.in/dksjQb7N

## Interview Tips

 * Functional vs Non-Functional Requirements
 * Mistakes to avoid while interviewing for System Design
 * Myths around System design interviews
   
## [[Understanding System Design of Popular Applications]]



 ## Desing Patterns
  
  * SOLID
  * GoF Design Patterns
  * Common Patterns used in Cloud Systems
  * Common Patterns used in Distributed systsems