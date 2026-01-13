# 🛡️ Security Group Specifications (Least Privilege)

The infrastructure is secured using nested Security Groups. Instead of using hardcoded IP addresses for internal traffic, I used **Security Group IDs** as sources to ensure that only authorized tiers can communicate.

| Security Group | Port / Protocol | Source | Purpose |
| :--- | :--- | :--- | :--- |
| **vprofile-alb-sg** | 80 (HTTP) | `0.0.0.0/0` | Public access to the Load Balancer. |
| **vprofile-app-sg** | 8080 (HTTP) | `vprofile-alb-sg` | Traffic only allowed from the ALB. |
| | 22 (SSH) | `Your-Public-IP` | Secure management access. |
| **vprofile-backend-sg** | 3306 (MySQL) | `vprofile-app-sg` | Database access for the App tier. |
| | 11211 (Memcached) | `vprofile-app-sg` | Caching access for the App tier. |
| | 5672 (RabbitMQ) | `vprofile-app-sg` | Messaging access for the App tier. |
| | 22 (SSH) | `vprofile-app-sg` | Jump-host access from the App tier. |
| | All Traffic | `Self` | **Backend Sync:** Allows DB, MQ, and MC to communicate with each other. |

---

### 💡 Implementation Note
By referencing Security Group IDs rather than static IPs, the infrastructure remains dynamic. If the Auto Scaling Group launches new Tomcat instances, they are automatically granted access to the backend without any manual firewall updates.