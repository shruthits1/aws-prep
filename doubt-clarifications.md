### **📌 AWS Prep Notes**
#### **1️⃣ Messaging & Event-Driven Architectures**
- **SQS vs. SNS**: Use SQS for **queuing**, ensuring messages are processed by one consumer at a time. Use SNS for **broadcasting**, delivering messages to multiple subscribers simultaneously.
- **SQS & SNS Together**: SNS can fan out messages to multiple SQS queues for asynchronous processing.
- **Comparisons**:
  - **Kafka** (event streaming) ~ **Kinesis** in AWS.
  - **Storm** (real-time processing) ~ **Kinesis Analytics** in AWS.
  - **Spark** (batch & stream processing) ~ **AWS Glue/EMR**.
#### **difference between amazon kinesis and sqs and sns**
Amazon Kinesis, SQS (Simple Queue Service), and SNS (Simple Notification Service) are all AWS messaging services, but they serve different purposes:

- **Amazon Kinesis** is designed for real-time data streaming. It allows multiple consumers to process the same data stream and enables re-reading of messages if needed. It's ideal for use cases like real-time analytics, log processing, and event-driven architectures.
- **Amazon SQS** is a message queuing service that provides asynchronous communication between distributed systems. It ensures reliable message delivery but does not support a publish-subscribe model. Messages are stored in a queue until they are processed by consumers.
- **Amazon SNS** is a pub-sub messaging service that allows messages to be sent to multiple subscribers simultaneously. It supports various delivery methods, including email, SMS, and Lambda functions. SNS is useful for broadcasting notifications and event-driven architectures.

#### **when to use SQS and when to use SNS**
- Use SQS when you need a reliable message queue for asynchronous processing. It ensures that messages are delivered to one consumer at a time, making it ideal for decoupling microservices, handling background tasks, and processing workloads at scale.
- Use SNS when you need a publish-subscribe model where messages are sent to multiple subscribers simultaneously. SNS is great for broadcasting notifications, event-driven architectures, and real-time updates.

####**Is MQ similar to SQS**

Key Differences

| Feature | Amazon SQS | Amazon MQ | 
| ---- | ---- | ---- |
| Message Model | Simple queue | Message broker | 
| Protocol Support | AWS API | AMQP, JMS, MQTT | 
| Use Case | Decoupling microservices | Migrating existing messaging systems | 
| Scalability | Highly scalable | Supports complex messaging patterns | 
  
  
#### **can SNS read from SQS**
No, Amazon SNS cannot directly read messages from an SQS queue. However, you can set up an Amazon SQS queue to subscribe to an SNS topic, allowing SNS to send messages to SQS. This is useful for fan-out messaging, where multiple consumers can process the same message asynchronously.
If you need SNS to process messages from SQS, you would typically use an AWS Lambda function that polls SQS, processes the messages, and then publishes them to an SNS topi

Amazon SQS is designed for one-to-one messaging, meaning each message is delivered to only one consumer. However, when combined with Amazon SNS in a fan-out pattern, multiple SQS queues can subscribe to the same SNS topic. Each queue receives a copy of the message, and from there, multiple consumers can process messages from different queues independently.
So while each individual SQS queue delivers a message to only one consumer, SNS allows multiple queues to receive the same message—essentially enabling fan-out messaging with multiple consumers.
  
  
#### **2️⃣ Networking & Security**
- **CloudFront vs. Global Accelerator vs. Route 53**:
  - **CloudFront** (CDN) accelerates content delivery.
  - **Global Accelerator** optimizes **network routing** for low-latency global access.
  - **Route 53** manages **domain resolution & traffic routing**.
- **VPN vs. VPC**:
  - **VPN** secures traffic over the internet.
  - **VPC** provides **isolated** cloud networking.
- **Security Protocols**:
  - **TLS vs. mTLS**: TLS ensures secure **communication**, while mTLS ensures **mutual authentication**.
  - **SSH vs. Session Manager**: AWS **Session Manager** is a more secure alternative to SSH.
- **Secrets Management**:
  - **KMS vs. Secrets Manager vs. Parameter Store**:
    - KMS encrypts data & manages keys.
    - Secrets Manager stores and rotates secrets.
    - Parameter Store stores configuration values **hierarchically**.

####**How they work together and their key differences**

| Service | Purpose | How It Works | 
| ---- | ---- | ---- |
| Amazon CloudFront | Content Delivery Network (CDN) | Caches and delivers static & dynamic content from edge locations worldwide to reduce latency. | 
| AWS Global Accelerator | Network Performance Optimization | Routes traffic through AWS’s global network for lower latency and improved availability. Uses static IPs for consistent routing. | 
| Amazon Route 53 | DNS Management | Provides domain name resolution, traffic routing, and health checks for applications. | 

How They Work Together
- CloudFront + Route 53: Route 53 can direct users to a CloudFront distribution, ensuring fast content delivery.
- Global Accelerator + Route 53: Route 53 can resolve domain names to Global Accelerator’s static IPs, improving failover and performance.
- CloudFront + Global Accelerator: CloudFront caches content, while Global Accelerator optimizes network routing for dynamic applications.

If you need fast content delivery, use CloudFront. If you need low-latency global routing with failover where traffic can switch from unhealthy to healthy endpoints, use Global Accelerator. And if you need domain resolution and traffic management, use Route 53.
  
**Note**
  - Fast Content Delivery refers to how quickly static and dynamic content (like images, videos, and web pages) is delivered to users
  - Low Latency refers to the delay between a request and its response.

####**when to use amazon key management service and aws secrets manager**
Amazon Key Management Service (KMS) and AWS Secrets Manager serve different purposes, but they often work together to enhance security:
If you need **encryption for data**, use **KMS**. If you need **secure storage and automatic rotation of credentials**, use **Secrets Manager**

**When to Use AWS KMS**
- **Encryption & Key Management**: KMS is used to create, manage, and control encryption keys for securing data.
- **Data Encryption**: Encrypts data in services like S3, EBS, RDS, and DynamoDB.
- **Access Control**: Provides fine-grained permissions for key usage.
- **Automatic Key Rotation**: Supports periodic key rotation for enhanced security.

**When to Use AWS Secrets Manager**
- **Secure Storage of Secrets**: Stores sensitive information like database credentials, API keys, and passwords.
- **Automatic Secret Rotation**: Helps rotate credentials without manual intervention.
- **Integration with AWS Services**: Works with RDS, Lambda, and other AWS services.
- **Access Control & Auditing**: Manages access permissions and logs secret usage.

**How They Work Together**
Secrets Manager **encrypts secrets using KMS**, ensuring secure storage. When retrieving a secret, Secrets Manager **decrypts it using KMS keys** before providing access.

**Why Use Parameter Store Instead of Secrets Manager?**
If you need secure storage for secrets with automatic rotation, go with Secrets Manager. If you need structured configuration management, Parameter Store is the better choice.

| Feature | Parameter Store | Secrets Manager | 
| ---- | ---- | ---- |
| Use Case | Configuration data & basic secrets | Sensitive secrets (passwords, API keys) | 
| Secret Rotation | Manual | Automatic | 
| Cost | Lower | Higher | 
| Access Control | IAM policies | IAM + fine-grained policies | 
| Hierarchy Support | Yes | No | 

- AWS Certificate Manager (ACM) generates and manages SSL/TLS certificates. When you request a public certificate, ACM creates a public/private key pair.
- The private key is securely stored and protected using AWS KMS.
- For imported certificates, you generate the key pair externally, and ACM stores the certificate and private key while using KMS for encryption.
#### **S3 SSE vs KMS SSE**
- SSE-KMS enables encryption at rest while allowing granular access control through IAM policies and KMS key permissions.
- SSE-S3 lacks key-level access controls, making SSE-KMS the more secure option

####**Different protocols for different layers**

| Protocol | Purpose | Use Case | Layer |
| ---- | ---- | ---- |
| SSL (Secure Sockets Layer) | Encrypts data between client & server | Deprecated, replaced by TLS | Application Layer |
| TLS (Transport Layer Security) | Secure communication over the internet | HTTPS, email encryption, VPNs | Application Layer |
| mTLS (Mutual TLS) | Both client & server authenticate each other | Zero-trust security, API authentication | Application Layer |
| SSH (Secure Shell) | Secure remote access & file transfer | Server administration, tunneling | Transport Layer |
| VPN | Encrypts all traffic | remote access, secure browsing, and protecting data from interception | Network Layer |


- SSL vs. TLS: TLS is the modern replacement for SSL, offering stronger encryption and security improvements.
- TLS vs. mTLS: TLS ensures secure communication, while mTLS adds mutual authentication, requiring both client and server to verify each other.
- TLS vs. SSH: TLS secures web traffic (HTTPS), while SSH is used for secure remote access to servers.
- mTLS vs. SSH: mTLS is used for API security, while SSH is used for server management.

####**is vpn similar to VPC on aws**
**Key Differences**
| Feature | VPN | VPC |
|---------|----|----|
| **Purpose** | Secure remote access to AWS resources | Isolated cloud network for AWS services |
| **Scope** | Connects external networks to AWS | Defines private networking within AWS |
| **Use Case** | Remote access for employees, hybrid cloud | Hosting applications, databases, and services |
| **Security** | Encrypts traffic over the internet | Controls network segmentation & access |

### **How They Work Together**
- **AWS Site-to-Site VPN** connects an **on-premises network** to a **VPC**, allowing secure communication.
- **AWS Client VPN** enables **individual users** to securely access AWS resources inside a VPC.

So, while **VPN provides secure connectivity**, **VPC defines a private cloud environment**.  

#### **Compare all DBs on AWS**

AWS offers a variety of **managed database services**, each designed for different workloads. Here's a comparison of the major AWS database services:

### **Comparison of AWS Database Services**

| Database Type | AWS Service | Best For | Key Features |
|--------------|------------|----------|--------------|
| **Relational (SQL)** | **Amazon RDS** | Traditional applications needing SQL databases | Supports MySQL, PostgreSQL, SQL Server, Oracle, MariaDB; automated backups & scaling |
|  | **Amazon Aurora** | High-performance SQL workloads | 5x faster than MySQL, 2x faster than PostgreSQL; auto-scaling storage |
| **NoSQL (Key-Value)** | **Amazon DynamoDB** | High-speed, scalable applications | Serverless, single-digit millisecond latency, auto-scaling |
| **Document Database** | **Amazon DocumentDB** | MongoDB-compatible workloads | Fully managed, scalable document database |
| **Graph Database** | **Amazon Neptune** | Social networks, fraud detection | Supports **RDF, Property Graph**, optimized for graph queries |
| **In-Memory Database** | **Amazon ElastiCache** | Caching, session storage | Supports **Redis & Memcached**, ultra-low latency |
|  | **Amazon MemoryDB** | High-performance Redis workloads | Durable, highly available Redis-compatible database |
| **Time-Series Database** | **Amazon Timestream** | IoT, DevOps monitoring | Optimized for time-series data, automatic scaling |
| **Wide-Column Database** | **Amazon Keyspaces** | Apache Cassandra workloads | Fully managed, scalable, serverless Cassandra-compatible database |
| **Ledger Database** | **Amazon QLDB** | Immutable transaction records | Cryptographically verifiable ledger, ideal for financial applications |
| **Data Warehousing** | **Amazon Redshift** | Analytics, business intelligence | Columnar storage, massively parallel processing (MPP) |

  
#### **3️⃣ Databases & Scaling**
- **RDS vs. Aurora**:
  - **RDS** supports multiple engines, requires **manual scaling**.
  - **Aurora** is **auto-scaling**, highly available, and optimized for **MySQL/PostgreSQL** and Fault-Tolerant Storage where data is stored in 6 copies across three Availability Zones (AZs).
 
- **Scaling in RDS**:
  - **Vertical Scaling**: Change **instance type** for more power.
  - **Horizontal Scaling**: Add **read replicas** for better read performance.
- **Storage Autoscaling**: Expands storage automatically when needed.

>> Note: configuring DynamoDB in on-demand mode allows dynamic scaling based on workload without specifying auto scalaing group. This won't happen if running dynamoDB in provisioned mode.
>> Note: S3 **object lock** eensures immutability where even admin cannot override. It is preferred for compliance purpose where principle is **write once read many** This can't be done via ACL as admins can override that.
>> Note: Aurora database cloning allows rapid creation of full copy of database without downtime. 

#### **Choosing between RDS and Aurora**

| Feature | Amazon RDS | Amazon Aurora | 
| ------| -----------| -----------|
| Database Engines | MySQL, PostgreSQL, MariaDB, SQL Server, Oracle | MySQL, PostgreSQL | 
| Performance | Standard | Up to 5x faster than MySQL | 
| Storage Scaling | Manual | Auto-scaling up to 128 TB | 
| Replication | Up to 5 read replicas | Up to 15 read replicas | 
| Failover | Manual (unless Multi-AZ enabled) | Automatic | 
| Availability | Multi-AZ option | Highly available with 6 copies of data | 
| Backup Impact | Can affect performance | Continuous backups with no performance impact | 

#### **Why would we use S3 for static content and dynamoDB for dynamic/user generated**
**S3** 
- **Optimized for Large Files** like images, videos, and HTML files,
- **Cost-Effective**, Can handle **millions of requests per second** without performance issues. Works with **CloudFront CDN** to deliver content efficiently worldwide.

**DynamoDB**
- DynamoDB provides **low-latency** access to structured data. **Flexible Queries**, **Auto Scaling**, Works well with **Lambda** for real-time updates.


>> **Note** for unpredictable pattern, always go with intelligent tiering

#### **ensuring RPO and RTO for dynamo DB**
- DynamoDB point-in-time recovery (PITR) allows automatic backups that can be restored to any point within the last 35 days, making it ideal for meeting the 15-minute RPO requirement.
- It enables quick recovery within the 1-hour RTO window, ensuring minimal downtime and efficient rollback in case of data corruption

#### **Difference between DMS and Datasync**

| Feature | AWS DataSync | AWS DMS |
|---------|------------|--------|
| **Purpose** | Transfers files & objects | Migrates databases |
| **Data Type** | Unstructured (files, objects) | Structured (databases) |
| **Use Case** | Moving large datasets between storage systems | Migrating databases with minimal downtime |
| **Real-Time Sync** | No (scheduled or manual transfers) | Yes (continuous replication) |
| **Supported Sources** | On-premises storage, S3, EFS, FSx | MySQL, PostgreSQL, Oracle, SQL Server, MariaDB |
| **Target Destinations** | S3, EFS, FSx, on-premises storage | RDS, Aurora, Redshift, DynamoDB |

#### **IAM Roles vs IAM Policies**
IAM **roles** and **policies** are closely related but serve different purposes. Let’s break it down:

### **IAM Roles vs. IAM Policies**

| Feature | IAM Role | IAM Policy |
|---------|---------|------------|
| **Purpose** | Defines **who** can assume permissions | Defines **what** permissions are granted |
| **Attached To** | Users, services, applications | Users, groups, roles, or resources |
| **Authentication** | Temporary credentials (assumed by entities) | No authentication, just permission rules |
| **Use Case** | Allow services to access AWS resources securely | Control access to AWS resources |
| **Examples** | When an **AWS service** (like Lambda, EC2, or ECS) needs access to another AWS resource. When **users from another AWS account** need temporary access. When using **federated authentication** | When defining **permissions** for users, groups, or roles. When restricting access to **specific AWS resources**. When enforcing **least privilege access** |

**How They Work Together**
- **Roles** define **who** can assume permissions.
- **Policies** define **what** those permissions allow.
- A role **without a policy** has **no permissions**.
- A policy **without a role** is **unused**.
**Example** If an EC2 instance needs access to S3, you:
- Create an IAM role with an S3 access policy.
- Attach the role to the EC2 instance.
- The instance can now access S3 without needing hardcoded credentials.

####**Difference between AWS Config and AWS CloudTrail**
- AWS Config is designed for tracking configuration changes in AWS resources, helping with compliance, governance, and security audits.
- AWS CloudTrail records history of API calls made to AWS services, allowing the company to audit changes and troubleshoot security events.

#### **AWS Shield vs Guard duty vs AWS Shield with Route 53 vs Amazon Inspector**
Great question! These AWS security services serve different purposes, but they can work together to enhance security across your cloud environment.

### **Comparison of AWS Shield, GuardDuty, Route 53 with Shield, and Amazon Inspector**
| Service | Purpose | Key Features | Use Case |
|---------|---------|--------------|----------|
| **AWS Shield** | **DDoS protection** | Protects against Layer 3/4 attacks (Shield Standard) and Layer 7 attacks (Shield Advanced) | Protects **CloudFront, ALB, Route 53, and AWS Global Accelerator** from DDoS attacks |
| **AWS GuardDuty** | **Threat detection** | Uses machine learning to detect anomalies in AWS logs (CloudTrail, VPC Flow Logs, DNS logs) | Identifies **compromised instances, unauthorized access, and malicious activity** |
| **AWS Shield + Route 53** | **DDoS protection for DNS** | Shield Advanced integrates with Route 53 to protect against DNS-based DDoS attacks | Ensures **high availability** of DNS services during attacks |
| **Amazon Inspector** | **Vulnerability assessment** | Scans EC2 instances, Lambda functions, and container workloads for security risks | Detects **misconfigurations, outdated software, and security vulnerabilities** |

### **When to Use Each?**
- **Use AWS Shield** if you need **DDoS protection** for web applications and AWS services.
- **Use GuardDuty** if you need **real-time threat detection** for AWS accounts and workloads.
- **Use Shield with Route 53** if you need **DDoS protection for DNS services**.
- **Use Amazon Inspector** if you need **automated security assessments** for workloads.

>> **AWS Systems Manager Session Manager provides secure, remote access to EC2 instances without needing SSH keys or a bastion host, reducing operational overhead.**

#### **IAM services**
> IAM roles are better suited for service-to-service authentication, not direct user authentication.

| Service | Best For | Key Features | Use Case |
|---------|---------|--------------|----------|
| **AWS IAM (Identity & Access Management)** | Managing permissions for AWS resources | Role-based access control (RBAC), policies, least privilege access | Secure access to AWS services for users, groups, and roles |
| **AWS IAM Identity Center (formerly AWS SSO)** | Centralized access management for multiple AWS accounts | Single sign-on (SSO), integration with external identity providers | Managing workforce access across AWS accounts |
| **IAM Access Analyzer** | Identifying unintended access to AWS resources | Detects overly permissive policies, security auditing | Ensuring compliance and security best practices |
| **AWS Directory Service** | Managing users with Active Directory | Supports **Microsoft AD, Simple AD, and AD Connector** | Integrating AWS with on-premises Active Directory |
| **IAM Roles Anywhere** | Secure access for workloads outside AWS | Provides temporary credentials for external applications | Allowing non-AWS systems to authenticate securely |
| **Amazon Cognito** | User authentication for applications | Supports **OAuth, OpenID Connect, and SAML** | Managing user sign-ups, logins, and identity federation |
| **Amazon Verified Permissions** | Fine-grained access control for applications | Policy-based authorization, integrates with Cognito | Enforcing access rules within custom applications |


#### **Why use AWS EMR**
Amazon Elastic MapReduce.
- Amazon EMR is a managed big data processing service that helps run distributed processing frameworks like Apache Spark, Hadoop, and Presto.
- It simplifies big data analytics on AWS by automating cluster provisioning, scaling, and management.
- EMR is widely used for data transformations, analytics, machine learning, and log processing at scale.

#### **AI ML services**
AWS offers a wide range of **AI and ML services**, covering everything from **pre-trained AI models** to **custom machine learning workflows**. Here's a comparison of the major AWS AI/ML services:

### **Comparison of AWS AI & ML Services**
| Category | AWS Service | Best For | Key Features |
|----------|------------|----------|--------------|
| **ML Model Development** | **Amazon SageMaker** | Building, training, and deploying ML models | Supports **custom models, AutoML, and foundation models** |
| **Pre-Trained AI Models** | **Amazon Bedrock** | Accessing foundation models (FMs) | Supports **Anthropic, Meta, Cohere, and AWS Titan models** |
| **Computer Vision** | **Amazon Rekognition** | Image and video analysis | Detects **faces, objects, text, and inappropriate content** |
| **Natural Language Processing (NLP)** | **Amazon Comprehend** | Text analysis and sentiment detection | Extracts **entities, key phrases, and language insights** |
| **Speech & Audio AI** | **Amazon Polly** | Text-to-speech conversion | Supports **lifelike speech synthesis** |
|  | **Amazon Transcribe** | Speech-to-text conversion | Converts **audio files into text** |
| **Chatbots & Conversational AI** | **Amazon Lex** | AI-powered chatbots | Supports **voice and text interactions** |
| **Document Processing** | **Amazon Textract** | Extracting text from documents | Reads **scanned PDFs, invoices, and forms** |
| **Personalization & Recommendations** | **Amazon Personalize** | AI-driven recommendations | Creates **customized user experiences** |
| **Forecasting & Predictions** | **Amazon Forecast** | Time-series forecasting | Uses **ML to predict trends and demand** |
| **Anomaly Detection** | **Amazon Lookout for Metrics** | Detecting anomalies in data | Identifies **unexpected patterns in business metrics** |
| **Code Optimization** | **Amazon CodeGuru** | AI-powered code reviews | Detects **security vulnerabilities and inefficiencies** |
| **AI-Powered Search** | **Amazon Kendra** | Intelligent search engine | Provides **semantic search across documents** |

- **Use Polly & Transcribe** for **speech AI**.
- **Use Lex** for **chatbots and conversational AI**.
- **Use Textract** for **document processing**.
- **Use Personalize** for **recommendation systems**.
- **Use Forecast** for **predictive analytics**.
- **Use Lookout for Metrics** for **anomaly detection**.
- **Use CodeGuru** for **AI-powered code reviews**.
- **Use Kendra** for **intelligent search**.

#### **Disaster recovery services**
**Disaster Recovery Services**
- **AWS Elastic Disaster Recovery (AWS DRS)**: Provides **fast recovery** for on-premises and cloud-based applications.
- **AWS Backup**: Centralized backup management for **EC2, RDS, DynamoDB, S3, and more**.
- **Amazon S3 Cross-Region Replication (CRR)**: Replicates objects across AWS regions for **data redundancy**.

**Failover Services**
- **Route 53 DNS Failover**: Automatically redirects traffic to a healthy endpoint during failures.
- **AWS Global Accelerator**: Routes traffic to the nearest healthy AWS endpoint for **low-latency failover**.
- **Amazon RDS Multi-AZ**: Provides automatic failover for **database instances**.

**Replication Services**
- **Amazon S3 Replication**: Supports **same-region and cross-region replication** for data redundancy.
- **Amazon Aurora Global Database**: Replicates data across multiple AWS regions for **high availability**.
- **AWS Database Migration Service (DMS)**: Helps migrate and replicate databases across AWS.

#### **Amazon AppFlow flow**
Amazon AppFlow is a fully managed integration service that enables secure data transfer between SaaS applications (like Salesforce, SAP, and Google Analytics) and AWS services (like S3 and Redshift) without requiring custom code.
How Amazon AppFlow Works
- Automates Data Transfers: Moves data between SaaS apps and AWS services on-demand or on a schedule.
- Supports Large-Scale Transfer for high-volume data 
- Encrypts data at rest and in transit.
- Works with AWS Glue, Redshift, and SageMaker for data processing.

#### **Server name indicator & its uses**
Server Name Indication (SNI) is an extension of the TLS (Transport Layer Security) protocol that allows a client (like a web browser) to specify the hostname it wants to connect to during the TLS handshake

When a client connects to a server using HTTPS, it includes the hostname in the TLS handshake. The server then presents the correct SSL certificate for that hostname, ensuring a secure connection.

