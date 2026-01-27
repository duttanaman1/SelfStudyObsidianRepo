
Mastercard[^9]

1. Architected a greenfield, high-consistency data platform for [[PII]] data using [[Cassandra]] (LWT) and optimized partitioning strategies, achieving enhanced read/write performance while meeting 100% enterprise consistency requirements[^1][^2]
2. Spearheaded end-to-end performance engineering[^11] by identifying system bottlenecks via [[JProfiler]][^10] and redesigning critical paths using [[CompletableFutures]], [[Blocking Queues]], and optimized Data Structures, resulting in a 40% increase in throughput validated through [[Gatling]] load tests in [[BlazeMeter]][^12].[^3]
3. Strengthened platform reliability and security by integrating [[Enclave]]-based cryptographic services ([[DCaaS]]), implemented multi-level [[Rate limiting]] ([[Bucket4j]]) as a backpressure mechanism, and safely adopted [[Java 21]] [[Virtual Threads]] [^13]after rigorous performance evaluation.
4. Elevated system reliability and performance through [[Caffeine]] caching strategies, designed database retry mechanism using [[Spring AOP]], and enabled application monitoring via [[Micrometer]]-based Actuator metrics exposed to [[Dynatrace]], with quality assurance enforced through automated Integration Testing in [[Jenkins]] CI.
5. Designed and scaled cloud-native microservices, improving throughput and resilience using [[Spring Boot]], [[Kafka]]-based event streaming, [[Database]] transactional persistence, and [[SAGA]]-orchestrated workflows.
6. Modernized legacy applications, boosting throughput and responsiveness by optimizing ETL/ELT [^7]pipelines and introducing asynchronous processing and concurrency APIs; Contributed to [[Micro frontend]]-based [[UI modernization]] in [[React]].
7. Delivered sprint commitments by owning estimates, participating in planning and backlog grooming, collaborating with stakeholders on requirements, conducting thorough code reviews, and producing [[low-level designs]] and documentation.
8. Raised platform quality and security to enterprise standards, eliminating [[Checkmarx]] vulnerabilities and [[SonarQube]] code smells, enforcing 95% unit test coverage via [[TDD]], and resolving production issues in compliance with [[Mastercard]] standards.
   
Barclays[^8]
   
9. Built backend and UI features for a customer acquisition platform, modernizing the system by extracting [[domain-driven design]] backend services and refactoring legacy UI into a [[feature-driven]] [[React]] architecture to support scalable onboarding of new enterprise clients.[^4]
10. Designed event-driven and asynchronous processing using [[ActiveMQ]] and [[Apache Camel]] 3.0–based orchestration, improving routing efficiency and reducing coupling between core processing and client-specific extensions.[^5]
11. Owned platform reliability and delivery by introducing [[Docker]]-based deployments with versioned application snapshots, reducing deployment time by 40%, and eliminating security vulnerabilities and code smells, enforcing 95% unit test coverage via [[TDD]], and resolving production issues per release standards.[^6]

[^1]: SDP is a greenfield project (new project) to be used for internal team purposes. There are many teams in mastercard that had to deal with PII/Sensitive data and they where using CaaS to mask the data and store in their own DB. This has increased load to CaaS a lot. And CaaS team is also pivoting to DCaaS (enclave). SDP is using DCaaS to store the data from multiple services providing high Consistency and high performance acting as dataware house. Since all the sensitive data is in database of SDP and it is to be used 24 hours by all other services: availability is a concern. Since the data are sensitive and should not be lost during tokenization, consistency is concern. Since SDP needs to be scaled as demand from services increases, Partition Tolerance is a concern.

[^2]: SDP uses Cassandra LWT mechanism to store vast amount of data. Cassandra database tables are defined in proper clustering order. Cassandra read/write threads (default 32) is increased to optimal level to increase performance. Multiple Race conditions, retries and error scenarios are handled to achieve virtual Transactional like behavior to ensure 100% consistency

[^3]: Redesigned the whole code base by using Concurrency APIs, Blocking Queues and NavigableMap/SortedSet data structures. Suing Gatling Scripts with different injection types tested the load stress and performance through 200 minutes of rigirous testing in Blazemeter


[^4]: The platform was used to onboard enterprise clients, and every new client came with custom requirements across both backend workflows and UI behavior. Initially, adding new clients meant touching tightly coupled backend code and making fragile UI overrides.
	
	My work started with delivering new client features end-to-end — backend APIs, business logic, and corresponding UI flows. While doing this repeatedly, I identified that the legacy structure was slowing us down and increasing risk.
	
	On the backend, I began isolating well-defined responsibilities from the legacy codebase and extracting them into domain-driven services so that client-specific logic could evolve without impacting core processing.
	
	On the UI side, the existing legacy UI structure made changes brittle. I refactored it into a feature-driven React architecture where each feature was isolated, making client-specific changes easier and safer.
	
	**Outcome:**  
	New enterprise clients could be onboarded faster, changes were isolated, and both backend and UI became more scalable and maintainable.

[^5]: As client requirements increased, synchronous request chains became fragile — failures or delays in client-specific processing started impacting core flows.
	
	To fix this, I helped redesign parts of the system to be event-driven. Core processing would complete its responsibility and publish events instead of directly invoking downstream client logic.
	
	ActiveMQ was used to enable asynchronous communication, ensuring that client-specific extensions did not block or destabilize core flows. For orchestration and routing, I introduced Apache Camel to define routing rules declaratively instead of hardcoding complex conditional logic.
	
	This allowed workflows to be extended or modified with minimal code changes and significantly reduced coupling between services.
	
	**Outcome:**  
	The system became more resilient, routing logic became easier to reason about, and client-specific extensions could scale independently.

[^6]: After feature delivery and modernization, the biggest risks shifted to **releases, quality, and production stability**. Deployments were slow, rollbacks were risky, and quality issues accumulated over time.
	
	I took ownership of platform reliability and delivery. I introduced Docker-based deployments with versioned application snapshots so that specific versions could be deployed, tested, or rolled back reliably across environments. This significantly reduced deployment time and release risk.
	
	In parallel, I focused on raising engineering standards. I systematically eliminated security vulnerabilities and code smells, enforced high unit test coverage using TDD practices, and cleaned up fragile areas of the codebase.
	
	I also handled production issues and releases, ensuring fixes met company release standards and did not regress existing clients.
	
	**Outcome:**  
	Releases became faster and safer, code quality and security improved to enterprise standards, and the platform became more stable in production.

[^7]: We had CSV-based data ingestion through REST APIs. I focused on optimizing the extract stage by improving the UI upload flow, introducing multipart handling, enabling GZip compression, and adding early validation to filter out bad data before transformation. In the transform stage, I simplified complex logic, removed redundant checks, and introduced fail-fast paths to reduce processing overhead. Finally, I worked with downstream services to ensure they could correctly consume the newly transformed data. Overall, this improved throughput, responsiveness, and system stability.

[^8]: “I worked in a core platform team where we onboarded new enterprise clients. My role spanned backend modernization, UI refactoring, and DevOps ownership. On the backend, I helped break a legacy monolith into domain-driven microservices, designed REST and ActiveMQ-based async flows, and built orchestration logic using Apache Camel. On the frontend, I led React modernization by replacing legacy AEM blocks with reusable feature-driven components. Later, I moved into DevOps ownership—handling Java 11 upgrades, Jenkins restructuring, Docker-based deployments, and improving platform quality by fixing SonarQube and Veracode issues. I also acted as SME for releases and production support for modernized flow”

[^9]: “I currently work in a core platform team focused on building and scaling a high-consistency data platform for sensitive PII workloads. My role spans greenfield system design, performance engineering, reliability, and security ownership. I architected key parts of the platform using Cassandra with LWTs to meet strict consistency guarantees, and worked extensively on data modeling and partitioning to balance correctness with performance.
	
	I led performance engineering initiatives by profiling production-like workloads using [[JProfiler]] and redesigning critical paths with asynchronous and concurrent processing, which resulted in significant throughput improvements validated through Gatling and [[BlazeMeter]].
	
	From a reliability and security standpoint, I integrated enclave-based cryptographic services, implemented [[Rate limiting]] and backpressure using Bucket4j, and evaluated and adopted Java 21 virtual threads after performance validation. I also strengthened observability and resilience through caching, retries, and metrics, with monitoring exposed to Dynatrace and quality enforced via CI pipelines.
	
	In parallel, I designed and scaled cloud-native microservices using Spring Boot, Kafka, PostgreSQL, and SAGA-based workflows, while owning end-to-end delivery including estimation, planning, and cross-team execution. I also contributed to modernizing legacy systems by improving ETL/ELT pipelines and supporting micro-frontend-based UI modernization in React.”

[^10]: “I use JProfiler to identify real runtime bottlenecks—CPU hotspots, memory leaks, GC pressure, and thread contention. I attach it to the JVM, run representative load, analyze hotspots and retained objects, fix inefficient code paths, and re-profile to validate improvements. Compared to logs or metrics, [[JProfiler]] gives exact method-level visibility.”

[^11]:  I used Gatling to generate controlled traffic and identify latency and throughput limits. Prometheus helped me correlate those symptoms with system metrics like CPU, GC, and thread pool saturation. When metrics didn’t align with expected resource usage, I attached [[JProfiler]] to identify JVM-level issues like blocking async calls and lock contention. I refactored the code to be non-blocking and validated improvements through repeated Gatling runs and stable Prometheus metrics.

[^12]: “We used [[BlazeMeter]] to run distributed Gatling tests at production scale. Local load tests weren’t sufficient, so BlazeMeter allowed us to simulate real concurrency patterns. The detailed reports helped us identify latency and throughput issues, which we then correlated with Prometheus metrics and JProfiler profiling to optimize JVM and async execution. BlazeMeter validated that our fixes held under real-world load.”

[^13]: “Migrated backend services to Java 21 [[Virtual Threads]], enabling scalable thread-per-request processing and improving concurrency under high I/O workloads without adopting reactive programming. Java 21 Virtual Threads let us keep the simple thread-per-request model while scaling like reactive systems, making them ideal for I/O-heavy microservices
