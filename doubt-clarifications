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

      
#### **3️⃣ Databases & Scaling**
- **RDS vs. Aurora**:
  - **RDS** supports multiple engines, requires **manual scaling**.
  - **Aurora** is **auto-scaling**, highly available, and optimized for **MySQL/PostgreSQL**.
- **Scaling in RDS**:
  - **Vertical Scaling**: Change **instance type** for more power.
  - **Horizontal Scaling**: Add **read replicas** for better read performance.
- **Storage Autoscaling**: Expands storage automatically when needed.

