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
