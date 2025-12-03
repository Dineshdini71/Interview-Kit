# 45 Minutes Before DevOps Interview – Quick Q&A Notes

---

## 🟧 AWS Questions

---

❓ **Q. What is an AMI?**  
✅ **A.** AMI stands for **Amazon Machine Image**. It mainly contains:
- Operating system
- Configuration files  
It is used to launch multiple virtual machines (EC2 instances) across any AWS Region.

---

❓ **Q. What is an EC2 instance and what are the types?**  
✅ **A.** EC2 is a **virtual machine** in AWS that represents a physical server where you deploy applications.  

**Types of EC2 instances:**
- General Purpose  
- Compute Optimized  
- Memory Optimized  
- Accelerated Computing  
- Storage Optimized  

---

❓ **Q. What are the different types of storage in AWS?**  
✅ **A.**

### 1️⃣ Block Storage
- **EBS (Elastic Block Store)**
- **Instance Store**

**Challenges with Instance Store:**
- Temporary storage – data is lost if you stop/terminate the instance  
- Fixed size and type  
- Only available for selected instance types  
- Cannot be detached and attached to other EC2 instances  

**Advantages of EBS:**
- Persistent (permanent) storage  
- Can be increased up to **16 TB**  
- Available in different types (SSD, HDD)  
- Can be attached to any EC2 instance in the same AZ  
- Can be detached and attached to other instances  

### 2️⃣ File Storage
- **EFS (Elastic File System)** – mainly used for **Linux**  
- **FSx** – mainly used for **Windows** and other file systems (e.g., Lustre, NetApp ONTAP)

### 3️⃣ Object Storage
- **S3 (Simple Storage Service)**  
- **Glacier** (S3 Glacier for archival storage)

---

❓ **Q. What are the different types of Load Balancers in AWS?**  
✅ **A.**
- **Application Load Balancer (ALB)**  
  - Works on **Layer 7** (HTTP/HTTPS)  
  - Can also support HTTP/2 and WebSockets  

- **Network Load Balancer (NLB)**  
  - Works on **Layer 4** (TCP/UDP)  
  - Supports high performance and low latency  
  - Supports **TCP and UDP** (good for streaming, real-time apps)

> ALB does **not** support UDP. That is handled by NLB.  
> NLB doesn’t support features like HTTP → HTTPS redirection and WAF integration like ALB.

---

❓ **Q. What are the different types of Auto Scaling?**  
✅ **A.**

- **Vertical Scaling (Scaling Up/Down):**  
  - Increasing resources (CPU/RAM) of the **same server**  
  - Example: changing from `t2.micro` to `t2.large`  
  - Instance needs a **stop/start** cycle  
  - Good for: DB servers, file servers, some app servers  

- **Horizontal Scaling (Scaling Out/In):**  
  - Adding or removing **multiple servers** automatically  
  - Used with **Auto Scaling Groups** behind Load Balancers  
  - Good for: stateless application servers

---

❓ **Q. What are the different Route 53 routing policies?**  
✅ **A.**

- **Latency-based Routing:**  
  - Routes traffic to the region with the **lowest latency** for the user.

- **Failover Routing:**  
  - Primary and secondary (standby) setup  
  - If primary fails, traffic is routed to secondary.

- **Weighted Routing:**  
  - You assign weights (e.g., 70–30) to different endpoints.  
  - Used for load sharing, A/B testing, gradual migration etc.

- **Geolocation Routing:**  
  - Routes traffic based on the **user’s location** (e.g., users from India → India endpoint, US → US endpoint).  
  - Used for compliance and local content.

- **Multi-Value Answer Routing:**  
  - Returns multiple IPs for a record.  
  - Client can pick any of them (basic load distribution).

---

❓ **Q. What is a CDN?**  
✅ **A.** CDN stands for **Content Delivery Network**.  
A large portion of internet content is delivered via CDNs.

Example:  
If you are in India and accessing a US website, it may be slow.  
A CDN (like **CloudFront**) stores a **cached version** of the content in multiple geographical locations called **Edge Locations (PoPs – Points of Presence)**.  
This reduces latency.

---

❓ **Q. How do you clear cache in CloudFront?**  
✅ **A.**
1. Go to **CloudFront** in AWS console  
2. Select **Distributions**  
3. Choose your distribution  
4. Go to **Invalidations**  
5. Create an invalidation with the path:  
   - `/*` (to clear all cached objects)

---

❓ **Q. What are the different types of databases we have in AWS?**  
✅ **A.**

- **Relational Databases (RDBMS):**  
  - MySQL, Oracle, MSSQL, PostgreSQL, MariaDB, Aurora  

- **NoSQL Databases:**  
  - DynamoDB  
  - DocumentDB  

- **In-Memory Databases:**  
  - ElastiCache (Redis, Memcached)  

- **Data Warehouse:**  
  - Redshift  

---

❓ **Q. What is Systems Manager?**  
✅ **A.**  
AWS **Systems Manager** is used to **centrally manage** and automate operational tasks across AWS resources.  

Key feature:  
- **Parameter Store** – store configuration values and secrets (securely).

---

❓ **Q. What is the difference between AWS CloudTrail and AWS Config?**  
✅ **A.**

- **AWS CloudTrail:**  
  - Tracks **API calls and events**  
  - Logs activities like login from console, CLI, SDKs, third-party tools (Terraform, Packer, etc.)

- **AWS Config:**  
  - Tracks **configuration changes** on AWS resources  
  - Shows history and compliance of resource configurations  
  - Gives a **consolidated view** of how a resource changed over time

---

❓ **Q. What is Elastic Beanstalk?**  
✅ **A.**  
Elastic Beanstalk is a **PaaS** service for deploying and scaling web applications and services.  
You upload your code and Elastic Beanstalk automatically handles:
- Provisioning
- Load balancing
- Auto scaling
- Health monitoring

---

❓ **Q. What is Blue-Green Deployment?**  
✅ **A.**  
- **Blue environment** – current production application  
- **Green environment** – new version of the application  

Process:
1. Deploy new version to **Green**  
2. Test thoroughly  
3. Switch traffic from **Blue** to **Green** via Load Balancer / Route 53  
4. If any issue, roll back by redirecting traffic to **Blue**

---

## 🟦 DevOps & Git Questions

---

❓ **Q. What branching strategy do you follow?**  
✅ **A.**  
We follow a **feature branching strategy** with long-lived branches:

- **main** – always points to **production-ready code**  
- **integration** (or develop) – main working branch where all features merge  

Process:
1. Create a **feature branch** from `integration`  
2. Develop code and run CI pipeline  
3. Raise a **Pull Request (PR)** to `integration`  
4. Once tested in QA / pre-prod / prod-like environment, merge to `main`  
5. Tag the release (e.g., `v1.0`)  
6. Resolve conflicts by sitting with developers if needed

---

❓ **Q. What is Git and why is it used?**  
✅ **A.**  
Git is a **distributed version control system** used to:
- Track changes in code  
- Collaborate with multiple developers  
- Maintain history of every change

---

❓ **Q. Explain the difference between Git and GitHub.**  
✅ **A.**  
- **Git:** Version control tool installed on your machine.  
- **GitHub:** Cloud-based hosting platform for Git repositories (collaboration, PRs, issues, etc.).

---

❓ **Q. What is a repository in Git?**  
✅ **A.**  
A **repository (repo)** is like a folder containing:
- Project files  
- Full history of changes (commits)

---

❓ **Q. What does the `git init` command do?**  
✅ **A.**  
`git init` creates a **new Git repository** in the current directory by setting up internal `.git` metadata.

---

❓ **Q. What is a commit in Git?**  
✅ **A.**  
A **commit** is a **snapshot** of your project at a particular point in time.

---

❓ **Q. How do you check the status of your repository?**  
✅ **A.**  
Use:  
```bash
git status
