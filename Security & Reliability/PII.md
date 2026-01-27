* What are PII data? Examples
  **PII (Personally Identifiable Information)** is **any data that can identify a specific individual**, either **directly** or **when combined with other data**. Card Number, IP address, Financial Transactions, etc
* Source of PII data. Examples
  In the finance domain, PII originates from customer onboarding, transactions, digital channels, third-party integrations, and regulatory/compliance systems.
  KYC platforms, Core banking systems, Ledger databases, **CRM**
* How PII data are masked ? 
	* a) Logging: 
		* Log redaction / masking
		* Structured logging with allow-list
	* b) Database:
		* Static Data Masking (SDM): Naman Dutta -> User_9328
		* Dynamic Data Masking (DDM):
		  ``` SELECT mask_pan(pan) AS pan FROM customer WHERE role != 'ADMIN'; ```
		* Tokenization
		* Encryption
	* c) backend code
		* DTO-level masking: Mask before sending response.
		* Role-based access control (RBAC): Only authorized roles see full PII.
		* Serialization filters: Jackson @JsonIgnore / @JsonView
		* Input validation & PII tagging
	* d) API Gateway
	* e) Messaging (Kafka / MQ)
	* f) Analytics / Data Warehouse
	* g) Non-Prod Environments
