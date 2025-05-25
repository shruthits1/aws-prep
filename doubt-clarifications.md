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
| ---- } ---- | ---- |
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
| ---- } ---- | ---- |
| Amazon CloudFront | Content Delivery Network (CDN) | Caches and delivers static & dynamic content from edge locations worldwide to reduce latency. | 
| AWS Global Accelerator | Network Performance Optimization | Routes traffic through AWS’s global network for lower latency and improved availability. Uses static IPs for consistent routing. | 
| Amazon Route 53 | DNS Management | Provides domain name resolution, traffic routing, and health checks for applications. | 

How They Work Together
- CloudFront + Route 53: Route 53 can direct users to a CloudFront distribution, ensuring fast content delivery.
- Global Accelerator + Route 53: Route 53 can resolve domain names to Global Accelerator’s static IPs, improving failover and performance.
- CloudFront + Global Accelerator: CloudFront caches content, while Global Accelerator optimizes network routing for dynamic applications.

If you need fast content delivery, use CloudFront. If you need low-latency global routing, use Global Accelerator. And if you need domain resolution and traffic management, use Route 53.
  
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
| ---- } ---- | ---- |
| Use Case | Configuration data & basic secrets | Sensitive secrets (passwords, API keys) | 
| Secret Rotation | Manual | Automatic | 
| Cost | Lower | Higher | 
| Access Control | IAM policies | IAM + fine-grained policies | 
| Hierarchy Support | Yes | No | 

- AWS Certificate Manager (ACM) generates and manages SSL/TLS certificates. When you request a public certificate, ACM creates a public/private key pair.
- The private key is securely stored and protected using AWS KMS.
- For imported certificates, you generate the key pair externally, and ACM stores the certificate and private key while using KMS for encryption.

####**Different protocols for different layers**

| Protocol | Purpose | Use Case | Layer |
| ---- } ---- | ---- |
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
      
#### **3️⃣ Databases & Scaling**
- **RDS vs. Aurora**:
  - **RDS** supports multiple engines, requires **manual scaling**.
  - **Aurora** is **auto-scaling**, highly available, and optimized for **MySQL/PostgreSQL** and Fault-Tolerant Storage where data is stored in 6 copies across three Availability Zones (AZs).

- **Scaling in RDS**:
  - **Vertical Scaling**: Change **instance type** for more power.
  - **Horizontal Scaling**: Add **read replicas** for better read performance.
- **Storage Autoscaling**: Expands storage automatically when needed.

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


