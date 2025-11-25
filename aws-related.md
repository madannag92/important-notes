https://github.com/swoodford/aws -> read this about sh scripting for aws
----------------------------------
What is the difference between a load balancer and a Cloud NAT gateway?

Load Balancer:
Distributes incoming traffic across multiple servers or services.
Primarily used for load distribution, redundancy, and high availability.
Works at various layers (L4 for TCP, L7 for HTTP/HTTPS).

Cloud NAT Gateway:
Provides outbound internet access for instances in a private network without
exposing them to inbound traffic.
Used for secure, private instances that need internet access without being directly
accessible from the internet.
----------------------------------------
If your using load balancing in 2 availability zones den which load balancer you should use?

Both ALB and NLB support multi-AZ deployment and will distribute traffic across the two Availability Zones, providing high availability. The choice depends on the type of traffic and features we need.

*For web applications and HTTP-based services, ALB is typically preferred.

*For high-performance, low-latency applications, NLB would be more appropriate.
---------------------------------------
*What is 3 tier architecture in aws

3-Tier Architecture in AWS refers to a classic application design pattern where the application is divided into three logical layers, each with its own responsibility. This pattern is widely used for scalability, security, and maintainability.

✅ The Three Tiers
Presentation Layer (Front-End)

Purpose: Handles the user interface and client-side logic.
AWS Services:
Amazon S3 (for hosting static websites)
Amazon CloudFront (CDN for faster delivery)
Elastic Load Balancer (ELB) (distributes traffic to web servers)
EC2 or AWS Elastic Beanstalk (for dynamic web apps)
Uses:
Public Subnet: Place web servers or load balancers in a public subnet.
AWS WAF (Web Application Firewall): Protect against common web exploits (SQL injection, XSS).
TLS/SSL: Use AWS Certificate Manager for HTTPS.

Application Layer (Business Logic)
Purpose: Processes requests, applies business rules, and interacts with the database.
AWS Services:
Amazon EC2 (compute instances for app servers)
AWS Lambda (serverless functions for logic)
Elastic Beanstalk (managed app deployment)
Uses:
Private Subnet:App servers should not be directly accessible from the internet.
Security Groups:Allow traffic only from the Presentation Layer (e.g., ELB SG).
IAM Roles:Grant least privilege for accessing other AWS services.

Data Layer (Database)
Purpose: Stores and retrieves application data.
AWS Services:
Amazon RDS (Relational Database Service)
Amazon Aurora (high-performance relational DB)
Amazon DynamoDB (NoSQL database)
Amazon ElastiCache (caching layer for performance)
Uses:
Private Subnet:Database should never be in a public subnet.
Security Groups:Allow traffic only from the Application Layer.
Encryption:Enable RDS encryption at rest and TLS in transit.
Backup & Recovery: Use RDS automated backups and snapshots.

---------------------------------------
what is difference between inline and managed policies in AWS IAM?

Inline Policies
Tightly coupled with a single IAM identity (user, group, or role).
Embedded directly into the identity.
Not reusable: You can't attach the same inline policy to multiple identities.
Use case: When you want a one-to-one relationship between the policy and the identity, often for fine-grained control or temporary permissions.
Example: A policy that gives a specific user access to a unique S3 bucket.

🔹 Managed Policies
There are two types:

AWS Managed Policies: Created and maintained by AWS.
Customer Managed Policies: Created and managed by you.
Reusable: Can be attached to multiple users, groups, or roles.
Easier to manage: Centralized updates apply to all attached identities.
Version control: You can update and track changes to customer managed policies.
Use case: When you want to apply consistent permissions across multiple identities.

| Feature      | Inline Policy                      | Managed Policy                      |
|--------------|------------------------------------|-------------------------------------|
| Attachment   | One identity only                  | Multiple identities                 |
| Reusability  | ❌ No                              | ✅ Yes                              |
| Management   | Embedded in identity               | Separate, centralized               |
| Versioning   | ❌ No                              | ✅ Yes (for customer managed)        |
| Best for     | Specific, tightly scoped needs     | Broad, reusable permissions         |

----------------------------------------
AWS Load Balancer Interview Questions🌟

👨‍💻1.What are the different types of Load Balancers provided by AWS?
AWS offers three types of Load Balancers:
👉Application Load Balancer (ALB): Best for HTTP/HTTPS traffic with advanced routing.
👉Network Load Balancer (NLB): Optimized for TCP/UDP traffic with ultra-low latency.
👉Classic Load Balancer (CLB): Older generation, ideal for simple use cases.

🚦2.When would you choose ALB over NLB or CLB?
💡Use ALB for HTTP/HTTPS traffic when:
✔You need features like path-based or host-based routing.
✔You want integration with services like Lambda or Web Sockets.
💡Use NLB for:
✔High-performance, low-latency traffic.
✔Protocols like TCP/UDP or when handling millions of requests/sec.

📂3.What is a Target Group in ALB/NLB, and how does it work?
A Target Group is a set of resources (like EC2 instances or containers) that receive traffic from a Load Balancer.
🔍ALB: Routes traffic to targets based on listener rules (e.g., path /api).
🔍NLB: Distributes traffic to targets without inspecting packets.

🔄4.What is Cross-Zone Load Balancing?
Cross-Zone Load Balancing distributes traffic evenly across all registered targets in all availability zones, regardless of the request origin.
💡This improves performance and reduces bottlenecks in multi-AZ setups.

🔑5.What is SSL/TLS Termination, and how is it implemented in AWS ELB?
SSL/TLS Termination occurs when the Load Balancer handles encryption/decryption, offloading this task from backend servers.
🛠Implementation: Upload your SSL certificate to ELB via ACM (AWS Certificate Manager). ELB manages secure traffic on behalf of your application.

✅6.How does AWS Auto Scaling integrate with Elastic Load Balancing (ELB)?
Auto Scaling works with ELB to:
✔Automatically add or remove instances based on demand.
✔Register and deregister instances with the Load Balancer dynamically.

💻7.How does path-based routing work in an Application Load Balancer?
Path-based routing directs traffic to specific Target Groups based on the request URL.
💡For example:
✔/app1 → Target Group 1
✔/app2 → Target Group 2

🔒8.How can you secure your Load Balancer?
✔Enable HTTPS with SSL/TLS certificates.
✔Use Security Groups to allow traffic only from trusted IPs.
✔Implement WAF (Web Application Firewall) for additional protection.

📊9.What are the health checks in Elastic Load Balancer, and how do they work?
ELB uses health checks to monitor the status of registered targets.
💡It sends requests (e.g., HTTP GET) at regular intervals to a specific path (e.g., /health check).
❌Unhealthy targets: Removed from routing until they recover.

📡10.How do you monitor and troubleshoot issues with an AWS Load Balancer?
✔Use Cloud Watch for metrics like request count, latency, and 4xx/5xx errors.
✔Enable Access Logs for detailed traffic insights.
✔Check Target Group health for any failed instances.


🌐 𝙒𝙝𝙚𝙣 𝙎𝙝𝙤𝙪𝙡𝙙 𝙔𝙤𝙪 𝙐𝙨𝙚 𝘼𝙇𝘽 𝙫𝙨. 𝘼𝙋𝙄 𝙂𝙖𝙩𝙚𝙬𝙖𝙮 + 𝘼𝙇𝘽 𝙛𝙤𝙧 𝙈𝙞𝙘𝙧𝙤𝙨𝙚𝙧𝙫𝙞𝙘𝙚𝙨 𝘾𝙤𝙢𝙢𝙪𝙣𝙞𝙘𝙖𝙩𝙞𝙤𝙣?

In a microservices architecture, 𝗔𝗽𝗽𝗹𝗶𝗰𝗮𝘁𝗶𝗼𝗻 𝗟𝗼𝗮𝗱 𝗕𝗮𝗹𝗮𝗻𝗰𝗲𝗿 (𝗔𝗟𝗕) is often the go-to solution for routing incoming requests to the correct microservices based on their paths. But here's the key question: 𝘿𝙤 𝙮𝙤𝙪 𝙣𝙚𝙚𝙙 𝙖𝙣 𝘼𝙋𝙄 𝙂𝙖𝙩𝙚𝙬𝙖𝙮 𝙤𝙣 𝙩𝙤𝙥 𝙤𝙛 𝙖𝙣 𝘼𝙇𝘽?

The answer depends on how your microservice APIs are intended to be used:

🔒 𝗙𝗼𝗿 𝗜𝗻𝘁𝗲𝗿𝗻𝗮𝗹 𝗨𝘀𝗲
If the APIs provided by the microservices are solely for internal use (within your VPC or Account), there’s no need for an additional API Gateway. The ALB’s DNS endpoint is sufficient to access the APIs directly.

🔠 𝗪𝗵𝘆?
💵 𝗖𝗼𝘀𝘁-𝗲𝗳𝗳𝗶𝗰𝗶𝗲𝗻𝘁: Reduces operational costs by avoiding unnecessary layers.
⚡ 𝗟𝗼𝘄 𝗹𝗮𝘁𝗲𝗻𝗰𝘆: Enables faster communication with fewer hops.
🛠 𝗦𝗶𝗺𝗽𝗹𝗶𝗳𝗶𝗲𝘀 𝘆𝗼𝘂𝗿 𝗮𝗿𝗰𝗵𝗶𝘁𝗲𝗰𝘁𝘂𝗿𝗲: Removes operational complexity for internal traffic.

🌍 𝗙𝗼𝗿 𝗘𝘅𝘁𝗲𝗿𝗻𝗮𝗹 𝗨𝘀𝗲
If you’re exposing your microservices' APIs to external consumers (e.g., business partners, external apps), an API Gateway becomes essential. It provides:
🛡 𝗦𝗲𝗰𝘂𝗿𝗶𝘁𝘆: Authentication and authorization.
🚦 𝗧𝗿𝗮𝗳𝗳𝗶𝗰 𝗠𝗮𝗻𝗮𝗴𝗲𝗺𝗲𝗻𝘁: Rate limiting, throttling, and quota management.
🔁𝗧𝗿𝗮𝗻𝘀𝗳𝗼𝗿𝗺𝗮𝘁𝗶𝗼𝗻: Request and response transformation for better API control.
📊 𝗠𝗼𝗻𝗶𝘁𝗼𝗿𝗶𝗻𝗴 & 𝗢𝗯𝘀𝗲𝗿𝘃𝗮𝗯𝗶𝗹𝗶𝘁𝘆: Centralized logging and metrics via CloudWatch.

While API Gateway offers these benefits, remember that it adds operational complexity and cost. 𝗜𝗳 𝘆𝗼𝘂 𝗱𝗼𝗻’𝘁 𝗻𝗲𝗲𝗱 𝗶𝘁, 𝗮𝘃𝗼𝗶𝗱 𝘂𝘀𝗶𝗻𝗴 𝗶𝘁 𝘂𝗻𝗻𝗲𝗰𝗲𝘀𝘀𝗮𝗿𝗶𝗹𝘆.

⭐️ 𝗞𝗲𝘆 𝗧𝗮𝗸𝗲𝗮𝘄𝗮𝘆𝘀:
💵 𝗖𝗼𝘀𝘁 𝗘𝗳𝗳𝗶𝗰𝗶𝗲𝗻𝗰𝘆: Avoid API Gateway for internal traffic to save costs.
🔒 𝗘𝗻𝗵𝗮𝗻𝗰𝗲𝗱 𝗦𝗲𝗰𝘂𝗿𝗶𝘁𝘆: Use API Gateway to secure and manage external-facing APIs.
⚡ 𝗟𝗼𝘄𝗲𝗿 𝗟𝗮𝘁𝗲𝗻𝗰𝘆: Leverage ALB for faster communication between internal microservices.

| Feature                | ALB (Application Load Balancer)                  | NLB (Network Load Balancer)                      |
|------------------------|--------------------------------------------------|--------------------------------------------------|
| OSI Layer              | Layer 7 (Application)                            | Layer 4 (Transport)                              |
| Protocols Supported    | HTTP, HTTPS, gRPC                                | TCP, UDP, TLS                                    |
| Routing Capabilities   | Host, path, and header-based routing             | IP/port-based, no advanced routing               |
| Target Types           | EC2, IP, Lambda                                  | EC2, IP, ALB                                     |
| Static IP              | No (can use with NLB in front for static IP)     | Yes                                              |
| Latency                | Low (sufficient for most apps)                   | Extremely low, designed for high-throughput apps  |
| TLS Termination        | Yes                                              | Yes                                              |
| WebSocket/gRPC         | Yes                                              | No (TCP only, unless proxies used)               |
| Client IP Preservation | Via X-Forwarded-For header                       | Yes (preserves source IP)                        |
| WAF Integration        | Yes                                              | No                                               |
| Authentication         | Yes (OIDC/SAML)                                  | No                                               |
| Use Cases              | Microservices, API gateways, complex HTTP apps   | Real-time, high-throughput, TCP/UDP workloads    |
| Pricing                | Higher (more features)                           | Generally lower, but depends on connections      |


-------------------------------------------------------------------------------------------

Security groups act as virtual firewalls at the instance level and are stateful: if inbound is allowed, return traffic is automatically allowed.

NACLs act at the subnet level and are stateless: both inbound and outbound rules must be set explicitly, and processing follows rule numbers in order (first match wins).

Security groups only support "allow" rules; NACLs support both "allow" and "deny".

Multiple security groups can be attached to one instance, but only one NACL per subnet.

Security groups provide flexible, stateful and granular instance-level control, while NACLs offer stateless, ordered, subnet-level filtering with allow and deny logic.

| Feature                  | Security Group                                              | Network ACL                                      |
|--------------------------|------------------------------------------------------------|--------------------------------------------------|
| Level of operation       | Instance (ENI)                                             | Subnet                                           |
| Type of rules            | Only allow                                                 | Allow and deny                                   |
| Statefulness             | Stateful (return traffic auto allowed)                     | Stateless (return traffic must be explicitly allowed) |
| Rule evaluation          | All rules evaluated                                        | Rules processed in order (lowest to highest rule number) |
| Default behavior         | All inbound denied, outbound allowed                       | All inbound/outbound allowed by default (customizable)  |
| Association              | Associated to ENI/instance                                 | Associated to subnet                             |

-----------------------------------------------------------------------------------------------
How do you enable secure communication between VPC components and S3 buckets without exposing them to the public internet?

To enable secure communication between VPC components and S3 buckets without exposing traffic to the public internet, use VPC endpoints for Amazon S3.

Key Mechanism: VPC Endpoints
AWS Gateway VPC Endpoint: Allows resources in a VPC (like EC2, Lambda) direct, private access to Amazon S3 without needing an internet gateway, NAT device, or public IPs.

Traffic between your VPC and S3 stays on AWS's internal network, never traversing the public internet.

VPC endpoint policies and S3 bucket policies can tightly restrict which resources and users are allowed to access buckets, enhancing security

-----------------------------------------------------------------------------------------------
Configure bucket policies to restrict access only to VPC endpoints

To restrict S3 bucket access only to traffic originating via specific VPC endpoints, use the aws:SourceVpce condition key in your bucket policy.

Sample Bucket Policy: Restrict to VPC Endpoint
Replace vpce-1a2b3c4d with your actual VPC endpoint ID, and the bucket name with yours.

example is below:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccessFromSpecificVPCEndpoint",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ],
      "Condition": {
        "StringNotEquals": {
          "aws:SourceVpce": "vpce-1a2b3c4d"
        }
      }
    }
  ]
}
