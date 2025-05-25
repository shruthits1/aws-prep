## **Question** 
A company recently migrated to AWS and wants to implement a solution to protect the traffic that flows in and out of the
production VPC. The company had an inspection server in its on-premises data center. The inspection server performed
specific operations such as traffic flow inspection and traffic filtering. The company wants to have the same functionalities in
the AWS Cloud.
Which solution will meet these requirements?
**Options**
A. Use Amazon GuardDuty for traffic inspection and traffic filtering in the production VPC.
B. Use Traffic Mirroring to mirror traffic from the production VPC for traffic inspection and filtering.
C. Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC.
D. Use AWS Firewall Manager to create the required rules for traffic inspection and traffic filtering for the production VPC

**Answer** C
Option A: Amazon GuardDuty is a threat detection service, not a traffic inspection or filtering service.
Option B: Traffic Mirroring is a feature that allows you to replicate and send a copy of network traffic from a VPC to another VPC or on-premises location. It is not a service that performs traffic inspection or filtering.
Option D: AWS Firewall Manager is a security management service that helps you to centrally configure and manage firewalls across your accounts. It is not a service that performs traffic inspection or filtering.
AWS Network Firewall is a **stateful managed firewall service** that provides filtering for both inbound and outbound network traffic. 
It allows you to create rules for traffic inspection and filtering, which can help protect your production VPC. This includes filtering traffic going to and coming from an internet gateway, NAT gateway, or over VPN
or AWS Direct Connect.
