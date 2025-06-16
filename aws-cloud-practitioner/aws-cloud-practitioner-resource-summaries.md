# AWS Cloud Practitioner Essentials

## *Coursera*

## *Glossary: [AWS Glossary \- AWS Glossary](https://docs.aws.amazon.com/glossary/latest/reference/glos-chap.html)*

# Resource Summaries

## What is Cloud Computing? 

📌 Source: [What is Cloud Computing? \- Cloud Computing Services, Benefits, and Types \- AWS](https://aws.amazon.com/what-is-cloud-computing/)  
Cloud computing means accessing IT resources (like servers, storage, and databases) over the internet.  
 You pay only for what you use, instead of buying and maintaining physical infrastructure.  
 Amazon Web Services (AWS) is one of the major providers of cloud computing services.  
Who Uses the Cloud  
Cloud is used by all types of organizations across industries:

* Healthcare: Personalized treatments  
* Finance: Real-time fraud detection  
* Gaming: Hosting large-scale online games  
* Other use cases include backups, disaster recovery, virtual desktops, software testing, big data analytics, and customer-facing applications.

Benefits of Cloud Computing  
1\. Agility  
 Access to a wide range of technologies like ML, IoT, databases, etc.  
 Fast deployment allows quick experimentation and faster time-to-market.  
2\. Elasticity  
 Scale up or down instantly based on demand.  
 No need for upfront over-provisioning.  
3\. Cost Savings  
 Pay-as-you-go pricing model.  
 Avoid upfront costs of buying and maintaining data centers.  
 Lower prices due to economies of scale.  
4\. Global Deployment  
 Deploy applications in multiple geographic locations in minutes.  
 Reduces latency and improves user experience.

Types of Cloud Computing  
1\. Infrastructure as a Service (IaaS)  
 Gives access to core IT resources: compute, storage, and networking.  
 Provides maximum flexibility and control.  
2\. Platform as a Service (PaaS)  
 Abstracts the underlying hardware and OS.  
 Focus only on application deployment and management.  
 Reduces operational workload like patching and capacity planning.  
3\. Software as a Service (SaaS)  
 Complete software applications managed by the provider.  
 User only focuses on using the app, not maintaining it.

Customer Examples

* BMW: Reduced translation time by over 75%  
* Coca-Cola: Improved analytics productivity by 80%  
* Epic Games: Supports 350 million+ players  
* Netflix: Streams to over 200 million users globally

What Are Cloud Services  
Cloud services are on-demand IT services provided over the internet.  
 They replace traditional methods of buying and configuring physical infrastructure.  
 Allow for faster development, innovation, and scaling.  
 Include services for AI/ML, analytics, networking, databases, security, and more.

What Are Cloud Managed Services  
Cloud services are also known as cloud managed services because AWS handles all underlying infrastructure.  
 Includes hardware, operating systems, and data center operations.   
AWS manages patching, monitoring, disaster recovery, and load balancing.  
Makes cloud technology accessible to individuals and organizations without large budgets.

Example Use Cases for Cloud Services  
1\. Compute  
 Provision scalable compute power with various CPU types (Intel, AMD, Arm).  
 Options for Mac instances, high-speed networking, and multiple pricing models.  
2\. Databases & Storage  
 Secure, scalable storage: file, block, and object.  
 Managed SQL and NoSQL database options.  
 Simplified scaling, backup, and maintenance.  
3\. AI/ML  
 Tools for chatbots, predictive analytics, image recognition, NLP, and more.  
 Infrastructure supports both training and inference tasks efficiently.  
4\. Networking & Content Delivery  
 Globally deliver content and applications with high availability.  
 Efficient traffic management and hybrid cloud connectivity.  
5\. Security & Compliance  
 Manage permissions and identities.  
 Implement fine-grained security policies.  
 Automated compliance monitoring and data protection services.  
6\. Migration & Modernization  
 Helps move existing apps and data to AWS.  
 Automated tools for conversion, testing, and minimal-downtime migration.  
 Supports digital transformation and adoption of cloud-native technologies.

## Types of Cloud Computing

Cloud computing lets developers and IT teams avoid tasks like hardware procurement, maintenance, and capacity planning.  
It gives more time to focus on building and scaling applications.  
AWS organizes cloud computing into 3 service models and 3 deployment strategies.

Cloud Service Models  
1\. IaaS (Infrastructure as a Service)  
Provides building blocks like virtual machines, networking, and storage.  
Gives you maximum control and flexibility.  
Closest to traditional IT infrastructure.  
Example use: You manage the OS and apps, AWS manages the hardware.

2\. PaaS (Platform as a Service)  
Removes the need to manage servers or OS.  
You focus only on application logic and deployment.  
No worries about updates, patching, or capacity planning.  
Good for developers who want to code and deploy quickly.

3\. SaaS (Software as a Service)  
Fully functional apps delivered over the internet.  
Vendor handles everything — servers, updates, infrastructure.  
You just use the app (like Gmail, Dropbox).  
Great for end-users and businesses that want ready-made solutions.

Deployment Models  
1\. Cloud  
Entire application is built and hosted in the cloud.  
Either developed in the cloud or migrated from traditional infrastructure.  
Can use low-level infrastructure or higher-level services to simplify development.

2\. Hybrid  
Combines on-premise infrastructure with cloud services.  
Common in companies that want to extend existing systems into the cloud.  
Allows gradual migration and better integration with legacy systems.

3\. On-Premises  
Resources are hosted in the organization’s physical location.  
Often uses virtualization and management tools.  
Offers control and privacy but lacks cloud scalability and flexibility.  
Sometimes used where full cloud adoption isn’t possible.

## AWS Products \- Compute

Why Choose AWS Compute?  
AWS Compute powers millions of organizations like Netflix, Coca-Cola, Lyft, and Moderna, helping reduce infrastructure costs and accelerate innovation.  
AWS has been a Gartner Magic Quadrant Leader in Cloud Infrastructure & Platform Services for 13 consecutive years.

Key Benefits of AWS Compute  
1\. Right Compute for Any Workload  
EC2: Full control over virtual machines with choice of CPU, storage, and networking.  
Containers: Broadest choice of container services.  
Lambda: Run code in response to events from 150+ AWS and SaaS services.

2\. Faster Time to Market  
Nitro System: Enables rapid innovation and performance.  
Lightsail: Quick VPS setup with predictable pricing.  
Lambda: Serverless deployment \= no infrastructure management.

3\. Built-in Security  
More security features and certifications than any other cloud provider.  
Nitro Security Chip: Built-in protection at the hardware level.  
Complies with PCI-DSS, HIPAA, FedRAMP, GDPR, FIPS 140-2, and more.

4\. Cost Optimization  
Pay-as-you-go model with no long-term contracts.  
Save up to 90% with EC2 Spot Instances.  
Save up to 72% with Savings Plans across all compute types.

5\. Compute Anywhere  
AWS delivers services from cloud to edge to on-prem.  
Outposts, ECS Anywhere, EKS Anywhere support hybrid environments.  
Wavelength enables ultra-low latency for 5G applications.

Stats and Recognition  
750+ EC2 instances for nearly every use case.  
80% of all container apps in the cloud run on AWS.  
84% of Kubernetes workloads in the cloud run on AWS.  
108 Availability Zones in isolated, secure physical locations.

AWS Compute Services Overview  
Virtual Machines (Instances)  
EC2: Scalable, secure cloud-based compute.  
EC2 Spot: Use spare capacity for up to 90% savings.  
EC2 Auto Scaling: Auto-adjust capacity based on demand.  
Lightsail: Simple VPS with storage, DB, networking.  
AWS Batch: Fully managed batch processing.  
AWS HPC: Run High Performance Computing workloads.

Containers  
ECS: Manage Docker containers.  
EKS / EKS Anywhere: Fully managed Kubernetes.  
ECR: Store and deploy container images.  
Fargate: Run containers without managing servers.  
App Runner: Deploy web apps from source code easily.

Serverless  
AWS Lambda: Run code without provisioning servers. Pay only per execution.

Edge and Hybrid  
AWS Outposts: Extend AWS infra/services on-premises.  
AWS Snow Family: Process data in disconnected or rugged environments.  
AWS Wavelength: Build ultra-low-latency apps in telco data centers.  
VMware Cloud on AWS: Run vSphere workloads in hybrid cloud.  
AWS Local Zones: Low-latency access in specific regions.

Cost & Capacity Management  
Savings Plans: Save up to 72% on compute usage.  
Compute Optimizer: Suggests ideal resources for performance and cost.  
EC2 Image Builder: Automate secure image creation.  
Elastic Load Balancing: Distribute incoming traffic for scalability and fault tolerance.

AWS Nitro System  
AWS reimagined virtualization with Nitro, which breaks core functions into dedicated hardware and software for speed, security, and performance.  
Nitro Cards: Offload functions like VPC, EBS, and storage for faster I/O.  
Nitro Security Chip: Secures hardware and prevents admin access—even from AWS staff.  
Nitro Hypervisor: Lightweight hypervisor offering near bare-metal performance.

## AWS Compute Services 

Instances (Virtual Machines)  
Amazon EC2: Secure, resizable compute capacity (virtual servers). Full control, scalable in minutes, pay-as-you-go.  
EC2 Spot Instances: Use unused EC2 capacity at up to 90% discount. Ideal for flexible or fault-tolerant workloads.  
EC2 Auto Scaling: Automatically add/remove EC2 instances based on defined policies. Supports dynamic and predictive scaling.  
Amazon Lightsail: Simplified cloud platform with all-in-one VPS bundles (VM, storage, IP, DNS) for a fixed price.  
AWS Batch: Fully managed batch computing at scale. Provisions optimal instance types based on job needs.

Containers  
Amazon ECS: Fully managed container orchestration service.  
Amazon ECS Anywhere: Run containers on your own hardware with ECS management.  
Amazon ECR: Private Docker container registry to store, manage, and deploy images.  
Amazon EKS: Fully managed Kubernetes service.  
Amazon EKS Anywhere: Run EKS clusters on-premises.  
AWS Fargate: Serverless compute for containers. No cluster or server management needed.  
AWS App Runner: Deploy containerized web apps/APIs directly from code or image. Fully managed, auto-scaling, and HTTPS-enabled.

Serverless  
AWS Lambda: Run code without provisioning servers. Pay only for actual compute time. Scales automatically, integrates with many AWS services.

Edge and Hybrid Computing  
AWS Outposts: AWS infrastructure/services on-prem for consistent hybrid cloud experience.  
AWS Snow Family: Portable devices for data collection and processing in remote or disconnected areas.  
AWS Wavelength: Deploy ultra-low latency apps on 5G networks at the edge.  
VMware Cloud on AWS: Seamlessly migrate or extend VMware vSphere workloads to AWS.  
AWS Local Zones: Run latency-sensitive applications closer to end-users in metro areas.

Cost and Capacity Management  
AWS Savings Plans: Commit to consistent usage for 1 or 3 years to save up to 72% on compute.  
AWS Compute Optimizer: Recommends optimal EC2, Lambda, and Auto Scaling configurations to reduce cost and boost performance.  
AWS Elastic Beanstalk: Automatically deploy, scale, and manage web applications with full access to underlying resources.  
EC2 Image Builder: Automate building, testing, and patching of VM or container images. GUI-driven, secure, and easy to maintain.  
Elastic Load Balancing (ELB): Distribute traffic across EC2 instances, containers, or IPs.

Amazon EC2 Instance Types Overview  
On-Demand: No long-term commitment. Ideal for spiky, unpredictable, or testing workloads.  
Spot Instances: Up to 90% cheaper. Best for jobs that can be interrupted or have flexible timing.  
Reserved Instances: Up to 72% savings for committed usage. Convertible and Standard options available.  
Dedicated Hosts: Physical servers fully allocated to you. Useful for licensing or compliance needs.

Specialized EC2 Instances  
C7g: Compute-optimized (Graviton3). Ideal for gaming, video processing, HPC, and ad-serving.  
M7g: General-purpose (Graviton3). Ideal for microservices, app servers, and mid-size databases.  
R7g: Memory-optimized (Graviton3). Best for caching, analytics, and memory-heavy databases.  
Inf2: Optimized for deep learning inference. Based on Inferentia2 chips. Suited for LLMs and vision transformers.  
Trn1: Optimized for training generative AI models. Based on Trainium chips. Cost-effective for large models.

Amazon EC2 Auto Scaling Features  
Fleet management: Maintains availability and replaces unhealthy instances automatically.  
Dynamic scaling: Adjusts capacity based on live usage metrics.  
Predictive scaling: Uses machine learning to forecast usage and scale ahead of time.

Amazon EC2 Image Builder Features  
GUI for creating, testing, and deploying secure VM/container images.  
Eliminates manual scripts and pipelines.

AWS maintains security baselines and patches.  
Free to use (only pay for compute/storage during the build process).

Amazon Lightsail Highlights  
Best for beginners or small projects.  
Fixed monthly pricing.  
Bundled with compute, storage, data transfer, static IP, DNS.

Amazon Linux 2023 (AL2023)  
Latest AWS Linux OS, replacing Amazon Linux 2\.  
Preconfigured security: SELinux (permissive), IMDSv2, kernel live patching.  
Deterministic upgrades via version-locked repositories.

Optimized for Graviton instances.  
5 years of support per major version, released every 2 years.

AWS App Runner Details  
Fully managed container deployment from code or image.  
No infra management. Auto-scaling, load balancing, HTTPS included.  
Good for devs with minimal ops experience.

AWS Batch Key Features  
Run large-scale batch jobs without managing servers.  
Automatically picks optimal compute resources (e.g., CPU/memory-heavy).  
Supports EC2, Fargate, and Spot to reduce costs.  
Focus on data analysis, not infra.

AWS Elastic Beanstalk Highlights  
Upload code → AWS handles deployment, scaling, health monitoring.  
Supports many languages (Java, .NET, PHP, Python, Node.js, Docker, etc.).  
Retain full control of resources if needed.

Amazon ECS Modes Comparison  
Fargate Launch Type:  
No servers/clusters to manage.  
Serverless, simple, pay for CPU/memory used.  
Ideal for small teams and microservices.

EC2 Launch Type:  
You manage EC2 instances (patching, scaling, etc.).  
More customization and control.  
Better for complex apps and compliance-heavy workloads.

