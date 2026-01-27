### 1. Strategy-Specific Rollbacks

How you roll back depends heavily on how you deployed in the first place.

- **Blue-Green Rollback (Instant):**
    
    - **Mechanism:** Since you have two identical environments, you simply flip the load balancer back to the "Blue" (old) environment.
    - **Speed:** Near-instant.
    - **Best for:** High-risk infrastructure changes or major version jumps.

- **Canary Rollback (Surgical):**
    
    - **Mechanism:** If the "Canary" group (e.g., 5% of users) experiences high error rates, you stop the rollout and divert those users back to the stable version.
    - **Speed:** Fast, and it limits the "blast radius" to only a few users.
    - **Best for:** Testing new features in a real-world environment.
- **Rolling Rollback (Staggered):**
    
    - **Mechanism:** If you are updating servers one by one and the new version fails, you must stop and replace the updated servers with the old version image.
    - **Speed:** Moderate (depends on how many servers were already updated).

---

### 2. Feature Flags: The "Kill Switch"

Feature flagging is now the first line of defense in modern deployments. It decouples **Deployment** (moving code to servers) from **Release** (turning features on for users).

- **How it works:** The code for the new feature is deployed but wrapped in an `if` statement.
- **Rollback:** If the feature breaks, you don't redeploy code. You simply toggle the flag to **OFF** in a dashboard (like LaunchDarkly or Split).
- **Benefit:** The "rollback" happens in milliseconds without touching the underlying infrastructure.

### 3. Database

- **Versioned Scripts**
	- Using tools like **Liquibase** or **Flyway**, every "Up" migration has a corresponding "Down" script.
	- Small schema changes (e.g., adding a non-required column).
- **Point-in-Time Recovery (PITR)**
	- Restoring the entire database to a snapshot taken exactly 1 minute before deployment.
	- Critical data corruption or catastrophic migration failures.
- **Expand and Contract**
	- (Discussed previously) Avoids the need for a "hard" database rollback by keeping both old and new schemas compatible.
	- Zero-downtime environments