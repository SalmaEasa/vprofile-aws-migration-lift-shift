# Route 53 Private Hosted Zone Records

To ensure decoupled communication between tiers, I implemented a **Private Hosted Zone** in Route 53 associated with the project VPC.

| Record Name | Type | Value (Private IP) | Description |
| :--- | :--- | :--- | :--- |
| **db01.vprofile.in** | A | `10.0.x.x` | MySQL Database |
| **mc01.vprofile.in** | A | `10.0.x.x` | Memcached Instance |
| **rmq01.vprofile.in** | A | `10.0.x.x` | RabbitMQ Broker |

### Why use DNS instead of IPs?
1. **Maintainability:** If a backend instance is replaced, we only update the DNS record in Route 53.
2. **Consistency:** The `application.properties` file remains the same across different environments.
3. **Internal Security:** DNS resolution stays within the VPC and is not exposed to the public internet.