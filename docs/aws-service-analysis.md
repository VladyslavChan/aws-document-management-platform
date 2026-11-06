AWS Service Analysis

Project: AWS Infrastructure for a Document Management Platform
Company: DocuWorks GmbH
Phase: Week 1 — Infrastructure Analysis & Architecture Design

1. Purpose

The purpose of this document is to evaluate AWS services for migrating the document management application of DocuWorks GmbH to the cloud.

The analysis focuses on:

* Compute and application deployment.
* Persistent application data storage.
* Document and object storage.
* Networking.
* Security and access control.
* Backup and recovery.
* Monitoring and logging.
* Cost efficiency.

Two possible architectures are considered:

Architecture A — EC2-Based Solution

A simple infrastructure using Amazon EC2, Docker Compose, PostgreSQL with Amazon EBS, and Amazon S3.

Architecture B — Managed Container Solution

A container-based infrastructure using Amazon ECS with AWS Fargate, Amazon RDS PostgreSQL, Amazon ECR, and Amazon S3.

The final architecture will be selected during Week 2.

⸻

2. Compute Services

2.1 Amazon EC2

Purpose: Provide a virtual server for hosting the application.

Amazon EC2 allows the company to run a Linux-based application environment in AWS.

In Architecture A, EC2 will host Docker containers for:

* The Python web application.
* The PostgreSQL database.

Docker Compose will be used to configure and run these containers.

Advantages:

* Direct control over the operating system.
* Supports Docker and Docker Compose.
* Suitable for small workloads.
* Flexible instance configuration.
* Relatively simple to implement using Terraform.

Disadvantages:

* Requires operating system maintenance.
* Requires manual database administration.
* A single instance creates a Single Point of Failure.
* Scaling and recovery require additional configuration.

Decision: Selected for Architecture A because it provides a simple and flexible environment for a small application.

2.2 Amazon ECS with AWS Fargate

Purpose: Run containerized applications without managing EC2 instances.

Amazon ECS is a container orchestration service. AWS Fargate provides the compute capacity required to run containers.

In Architecture B, the application will run as an ECS Task using a Docker image.

Advantages:

* No direct EC2 server administration.
* Supports containerized applications.
* Application containers can be replaced independently of persistent data.
* Integrates with IAM, CloudWatch, and load balancing.
* Supports future scaling.

Disadvantages:

* More AWS services must be configured.
* Requires understanding of ECS Tasks, Services, and networking.
* May cost more than a small EC2 deployment.
* Requires additional infrastructure for external access and container image retrieval.

Decision: Selected for Architecture B as an alternative managed container deployment approach.

2.3 Other Compute Alternatives

AWS Lambda

Lambda is suitable for event-driven and short-running functions.

It is not selected as the primary application hosting solution because the project focuses on a traditional web application and persistent storage integration.

Lambda may be considered later for background document processing.

⸻

3. Containerization

3.1 Docker

Docker will be used to package the application and its dependencies into a container image.

This provides a consistent runtime environment for local development and AWS deployment.

Docker is an additional project design choice.

3.2 Docker Compose

In Architecture A, Docker Compose will manage the application and PostgreSQL containers on EC2.

The containers will communicate through an internal Docker network.

The PostgreSQL container will use a persistent directory located on an EBS volume.

Important database data must not be stored only inside the container’s temporary filesystem.

3.3 Amazon ECR

Amazon Elastic Container Registry (ECR) provides a registry for Docker container images.

In Architecture B, ECR will store the application image used by ECS Fargate.

The ECS Task Execution Role will provide the permissions required to retrieve the image.

Decision: Docker will be used in both architectures, while Docker Compose and ECS provide different deployment and management approaches.

⸻

4. Persistent Application Storage

4.1 Amazon EBS

Purpose: Provide persistent block storage for EC2.

Amazon EBS provides storage volumes that can be attached to EC2 instances.

In Architecture A, a separate EBS data volume will store PostgreSQL database files.

The EBS volume will be mounted on the EC2 host and made available to the PostgreSQL container through a Docker bind mount.

Advantages:

* Persistent block storage.
* Suitable for PostgreSQL database files.
* Supports encryption.
* Supports snapshots.
* Can be detached and reattached to compatible EC2 instances in the same Availability Zone.

Disadvantages:

* Tied to an Availability Zone.
* Requires database administration.
* Does not automatically provide high availability.
* Volume deletion settings must be managed carefully.

Decision: Selected for Architecture A to provide persistent storage independently of the Docker container lifecycle.

4.2 Amazon RDS

Purpose: Provide a managed relational database.

In Architecture B, Amazon RDS for PostgreSQL will store application data.

Examples include:

* Document metadata.
* Document processing status.
* Processing results.
* Document storage references.

Advantages:

* Managed database infrastructure.
* Automated backup support.
* Monitoring integration.
* Less operating system and database infrastructure administration.
* Optional Multi-AZ deployment.

Disadvantages:

* Additional ongoing costs.
* More network and security configuration.
* Less operating system-level control.
* Single-AZ deployment still has availability limitations.

Decision: Selected for Architecture B because it separates database management from application deployment.

4.3 Other Storage Alternatives

Amazon EFS

EFS provides a shared network file system.

It may be useful when multiple application instances require access to the same filesystem.

It is not initially selected because S3 is more appropriate for object storage in the proposed application design.

Amazon DynamoDB

DynamoDB is a managed NoSQL database.

It is not selected because PostgreSQL is proposed for structured application data with relational queries.

⸻

5. Document Storage — Amazon S3

Purpose: Store uploaded customer documents separately from the application environment.

Amazon S3 is an object storage service.

The application will store document files in an S3 bucket.

Examples include:

* PDF documents.
* Images.
* Uploaded customer files.
* Generated processing results, where appropriate.

5.1 Application Integration

The application will communicate with Amazon S3 using the AWS SDK.

Typical operations include:

* PutObject — upload a document.
* GetObject — retrieve a document.
* DeleteObject — delete an object when authorized.

The application database will store references to S3 objects.

Example:

* Document ID: 105
* Filename: contract.pdf
* S3 object key: documents/105/contract.pdf
* Status: uploaded

The file content is stored in S3, while the structured metadata is stored in PostgreSQL.

5.2 Advantages

* Separates documents from the application server.
* Supports gradual storage growth.
* High object durability.
* Supports encryption and access control.
* Supports versioning.
* Can be accessed through AWS APIs.

5.3 Disadvantages

* Requires application integration with S3.
* Storage, requests, and data transfer may incur costs.
* S3 is not a replacement for a relational database.
* Versioning alone is not a complete independent backup strategy.

Decision: Selected for both architectures as the primary document storage service.

⸻

6. Networking

6.1 Amazon VPC

Amazon Virtual Private Cloud (VPC) provides an isolated virtual network for AWS resources.

Both architectures will use a VPC.

The network design will include the required subnets, routing, and security controls.

6.2 Architecture A Networking

The initial Architecture A proposal includes:

* One VPC.
* A public subnet.
* An Internet Gateway.
* An EC2 instance.
* A Security Group.

The application will be accessible through a controlled web endpoint.

PostgreSQL will not be directly exposed to the public internet.

6.3 Architecture B Networking

The Architecture B proposal includes:

* One VPC.
* Public subnets for an Application Load Balancer.
* Subnets for ECS Fargate Tasks.
* Private database subnets for Amazon RDS.
* Security Groups controlling communication.

The preferred production design places application tasks in private subnets.

Private tasks require an appropriate outbound access solution, such as NAT or relevant VPC endpoints, to access required AWS services.

A lower-cost demonstration configuration using public task networking may also be evaluated, with restricted Security Groups.

The final networking approach will be selected during Week 2.

6.4 Application Load Balancer

An Application Load Balancer (ALB) is proposed for Architecture B.

It will receive incoming web traffic and forward requests to healthy ECS Tasks.

ALB introduces additional costs and complexity.

It is not required for the initial Architecture A design.

⸻

7. Security Services

7.1 AWS IAM

AWS Identity and Access Management (IAM) controls access to AWS resources.

IAM roles will be used to provide AWS permissions to application workloads.

Architecture A:

An EC2 Instance Role will allow the application to access the required AWS resources.

Architecture B:

An ECS Task Role will provide permissions to the application.

An ECS Task Execution Role will support operations required to start the task, such as pulling images from ECR.

IAM permissions should follow the principle of least privilege.

7.2 Security Groups

Security Groups control allowed network traffic.

Proposed controls include:

* Allow only required web traffic to the application.
* Do not publicly expose the PostgreSQL database port.
* Restrict database connections to authorized application components.
* Limit administrative access.

7.3 Data Protection

The infrastructure should use:

* Encryption at rest.
* HTTPS for application access.
* Private S3 buckets.
* S3 Block Public Access.
* Secure handling of database credentials.
* Least-privilege IAM roles.

7.4 Application Authentication

AWS IAM permissions do not replace application user authentication.

The application must separately authenticate and authorize employees before allowing access to company documents.

The exact authentication mechanism has not yet been specified.

⸻

8. Backup and Recovery

8.1 Architecture A

The proposed backup strategy includes:

* Database-aware PostgreSQL backups.
* EBS snapshots where appropriate.
* Amazon S3 Versioning.
* Documented EC2 recovery procedures.

Database backups must be consistent and restorable.

EBS snapshots should not be treated as a complete database recovery strategy without considering PostgreSQL consistency.

8.2 Architecture B

The proposed backup strategy includes:

* RDS automated backups.
* RDS point-in-time recovery where configured.
* Amazon S3 Versioning.
* Documented restoration procedures.

8.3 Recovery Requirements

For both architectures, the project must demonstrate at least one recovery operation.

The recovery test must verify that important application data can be restored.

Backup retention, recovery objectives, and detailed restoration procedures will be defined during implementation.

⸻

9. Monitoring and Logging

Amazon CloudWatch

Amazon CloudWatch is proposed for both architectures.

The monitoring solution should provide:

* Infrastructure metrics.
* Application logs.
* Resource health information.
* Basic alarms where appropriate.

Architecture A:

Monitor EC2 resource health and application logs.

Architecture B:

Monitor ECS Tasks, RDS metrics, and application logs.

Detailed monitoring will be configured during implementation.

⸻

10. Infrastructure as Code — Terraform

Terraform will be used to provision the AWS infrastructure.

It will manage resources such as:

* VPCs and subnets.
* Security Groups.
* EC2 instances.
* EBS volumes.
* S3 buckets.
* IAM roles.
* ECS and RDS resources where required.
* Monitoring resources.

Docker will manage application packaging.

Terraform will manage AWS infrastructure provisioning.

The final implementation should support:

* terraform init
* terraform plan
* terraform apply
* terraform destroy

Secrets and credentials must not be committed to GitHub or included in public Terraform state files.

⸻

11. Preliminary Service Comparison

Component	Architecture A	Architecture B
Compute	EC2	ECS Fargate
Containers	Docker Compose	ECS Tasks
Container images	Local/deployed image	Amazon ECR
Database	PostgreSQL on EC2	Amazon RDS PostgreSQL
Database storage	Amazon EBS	Managed RDS storage
Documents	Amazon S3	Amazon S3
Networking	VPC + public subnet	VPC + ALB + application/database subnets
Access control	IAM + Security Groups	IAM + Security Groups
Database backup	PostgreSQL backups / EBS snapshots	RDS automated backups
Document protection	S3 Versioning	S3 Versioning
Monitoring	CloudWatch	CloudWatch
Infrastructure	Terraform	Terraform

⸻

12. Cost Considerations

The company requires a cost-conscious solution.

Architecture A — Main Cost Factors

* EC2 instance usage.
* EBS storage.
* EBS snapshots.
* S3 storage and requests.
* Public IPv4 usage.
* CloudWatch usage.
* Data transfer.

Architecture B — Main Cost Factors

* Fargate compute.
* RDS instance and storage.
* Application Load Balancer.
* ECR storage.
* S3 storage and requests.
* CloudWatch usage.
* Networking and data transfer.
* NAT or VPC endpoint costs, if used.

Architecture A is expected to be simpler and potentially less expensive for the initial workload.

Architecture B provides more managed infrastructure but may introduce higher ongoing costs.

Actual prices must be calculated using the selected AWS Region, resource sizes, deployment duration, and applicable AWS credit conditions.

No final cost estimate has been approved at this stage.

⸻

13. Preliminary Evaluation

Criterion	Architecture A	Architecture B
Initial complexity	Lower	Higher
Server management	Customer-managed	Fargate-managed compute
Database administration	Self-managed PostgreSQL	Managed RDS
Application replacement	Requires EC2/application recovery	ECS Task replacement
Document durability	Amazon S3	Amazon S3
Database persistence	Amazon EBS	Amazon RDS
Expected cost	Potentially lower	Potentially higher
Future scalability	Possible with additional design	More flexible container deployment
Availability	Single EC2 is a SPOF	Depends on task count and RDS configuration

Preliminary Assessment

Architecture A appears suitable for an initial small-scale implementation because of its simplicity and potential cost advantages.

Architecture B offers stronger separation between application runtime and database management.

However, no final architecture decision is made during Week 1.

The decision will be documented during Week 2 after comparing costs, reliability, security, storage durability, operational complexity, and recovery requirements.

⸻

14. Conclusion

The analysis identifies suitable AWS services for the document management platform.

The central design principle is the separation of:

1. Application runtime.
2. Persistent application data.
3. Uploaded document storage.

Docker will provide a consistent application runtime.

Amazon EBS or Amazon RDS will provide persistent database storage, depending on the architecture.

Amazon S3 will store uploaded documents.

IAM, Security Groups, CloudWatch, and backup mechanisms will support security, observability, and recoverability.

Both proposed architectures meet the main project objectives at the conceptual design level.

The next step is to document Architecture Option A and Architecture Option B in detail, create architecture diagrams, and select the final implementation during Week 2.