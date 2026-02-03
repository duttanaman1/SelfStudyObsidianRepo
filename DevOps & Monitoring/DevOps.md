Development + Operations

### **1. Core DevOps Principles (Mindset First)**

1. **CI/CD – Automate Software Delivery**: Continuous Integration (merge + test often) & Continuous Delivery/Deployment (automated, frequent releases).
	1. CI: Unit + Integration tests run on every commit.
	2. CD: Automated builds, deploys, and rollbacks with approvals if needed.
2. **Infrastructure as Code (IaC)**: Define infra (servers, DBs, networks, clusters) in code, versioned in Git.
	1. Declarative tools: [[Terraform]], [[CloudFormation]].
	2. Configuration management: [[Ansible]], Puppet, [[Chef]].
3. **Observability**: Visibility into _what_ the system is doing and _why_. Goes beyond metrics to **logs + metrics + traces**.
	1. - Metrics: [[Prometheus]], [[CloudWatch]], [[Micrometer]].
	2. Logs: [[ELK/EFK]], Loki.
	3. Traces: Jaeger, [[OpenTelemetry]].
4. **Shift-Left Security ([[DevSecOps]])**: Catch security issues early in dev instead of at production.
	1. Static code analysis ([[SonarQube]], [[Checkmarx]]).
	2. Dependency scanning ([[Snyk]], Trivy).
	3. Secrets scanning ([[Vault]], [[Git hooks]]).
5. **Fail Fast & Recover (Resilience)**: Design systems that expect failure and recover quickly.
	1. Deployment strategies: [[Blue-Green]], Canary, [[Feature flags]].
	2. [[Circuit Breaker]], retries, [[Rate limiting]].
	3. Chaos testing (Gremlin, [[Chaos Monkey]]).

### **2. Tools & Ecosystem (Hands-on Skills)**
- **Version Control & Workflow**: [[Git]], Git branching strategies (GitFlow, trunk-based).
- **CI/CD Pipelines**: [[Jenkins]], [[GitHub Actions]], GitLab CI, ArgoCD, Tekton.
- **Containerization**: [[Docker]] fundamentals, container lifecycle.
- **Orchestration**: [[Kubernetes]] (deployment, scaling, service mesh like Istio/Linkerd).
- **Infrastructure as Code**: [[Terraform]], [[Ansible]], Helm charts.
- **Cloud Platforms**: [[AWS]] (EKS, ECS, Lambda), Azure DevOps, [[GCP]] – focus on services you actually use.
- **Monitoring & Observability**: [[Prometheus]], [[Grafana]], [[ELK/EFK stack]], OpenTelemetry.
- **Security & Compliance**: [[SonarQube]], Snyk, Trivy, [[Vault]], [[Checkmarx]], SBOMs.
- **Artifact Management**: Nexus, Artifactory, Harbor, [[JFrog]]

### **3. Advanced Practices (Senior-Level Impact)**

- **Scalability & Reliability**: [[Blue-Green]], Canary, [[Feature flags]], [[Chaos Engineering]].
- **Cost Optimization**: Spot instances, right-sizing, FinOps alignment.
- **Service Mesh & API Gateway**: Istio, Envoy, [[Spring Cloud Gateway]].
- **Event-Driven Architectures**: [[Kafka]], RabbitMQ with DevOps pipelines.
- **Zero-Downtime Deployments**: Rolling upgrades, traffic shadowing.
- **Secrets Management**: Vault, KMS, SOPS, Sealed Secrets.
- **Policy as Code**: OPA (Open Policy Agent), Kyverno.
- **Multi-Cloud & Hybrid Deployments**.