# Weekly Project Status — Week 1

Project: AWS Infrastructure for a Document Management Platform
Company: DocuWorks GmbH
Student: Vladyslav Chan
Reporting Period: October 26–30, 2026
Project Phase: Infrastructure Analysis & Architecture Design
Status: Design documentation prepared; implementation not started

⸻

1. Weekly Objective

The objective of Week 1 was to analyze the existing infrastructure of DocuWorks GmbH, identify its main limitations, define business and technical requirements, and design two possible AWS architecture solutions.

The focus was on architecture planning rather than infrastructure implementation.

2. Work Completed

2.1 Current Infrastructure Analysis

Analyzed the company’s existing infrastructure and identified the following problems:

* A single physical server creates a Single Point of Failure.
* Application data and documents are stored on the same server.
* Server capacity is limited.
* Backups are performed manually.
* Infrastructure configuration is managed manually.
* Server failure may interrupt employee access.

2.2 Business and Technical Requirements

Documented the main requirements for:

* Application hosting and accessibility.
* Persistent application data.
* Separate document storage.
* Security and access control.
* Backup and recovery.
* Monitoring and logging.
* Cost efficiency.
* Infrastructure provisioning using Terraform.

2.3 Assumptions and Constraints

Identified known project constraints and documented technical assumptions.

The proposed technology stack includes:

* AWS.
* Terraform.
* Linux.
* Docker.
* Python / Flask.
* PostgreSQL.
* Amazon S3.

Docker was added as a project design choice to support consistent application deployment.

2.4 AWS Service Analysis

Evaluated AWS services for compute, storage, networking, security, backup, and monitoring.

Main services considered:

* Amazon EC2.
* Amazon ECS with AWS Fargate.
* Amazon EBS.
* Amazon RDS.
* Amazon S3.
* Amazon ECR.
* Amazon VPC.
* AWS IAM.
* Amazon CloudWatch.

The analysis includes advantages, disadvantages, alternatives, and technical trade-offs.

2.5 Architecture Option A

Designed an EC2-based architecture using:

* Amazon EC2.
* Docker Compose.
* Python / Flask application.
* PostgreSQL on persistent EBS storage.
* Amazon S3 for documents.
* IAM and Security Groups.
* CloudWatch.
* Backup and recovery mechanisms.

This option prioritizes simplicity and cost efficiency.

2.6 Architecture Option B

Designed a managed container-based architecture using:

* Amazon ECS with AWS Fargate.
* Docker container images.
* Amazon ECR.
* Amazon RDS PostgreSQL.
* Amazon S3.
* Application Load Balancer.
* VPC networking.
* IAM and Security Groups.
* CloudWatch.
* Backup and recovery mechanisms.

This option provides improved separation of application runtime and database administration but introduces greater complexity and potentially higher costs.

2.7 Architecture Diagrams

Prepared conceptual architecture diagrams for both proposed solutions.

The diagrams illustrate:

* Application access.
* AWS compute components.
* Network boundaries.
* Persistent application data.
* Document storage.
* Security components.
* Monitoring and backup relationships.

Final diagram review and alignment with official AWS Architecture Icons remain to be completed before final submission.

⸻

3. Week 1 Deliverables

Deliverable	Document	Status
Business & Technical Requirements	docs/business-technical-requirements.md	Draft prepared
Assumptions & Constraints	docs/assumptions-constraints.md	Draft prepared
AWS Service Analysis	docs/aws-service-analysis.md	Draft prepared
Architecture Option A	docs/architecture/option-a.md	Draft prepared
Architecture Option B	docs/architecture/option-b.md	Draft prepared
Architecture Diagram A	docs/architecture/diagram-a.png	Diagram prepared
Architecture Diagram B	docs/architecture/diagram-b.png	Diagram prepared

The files must be verified in the GitHub repository before marking the deliverables as submitted.

⸻

4. Preliminary Architecture Evaluation

Criterion	Option A	Option B
Compute	EC2	ECS Fargate
Container deployment	Docker Compose	ECS
Database	PostgreSQL on EC2	RDS PostgreSQL
Persistent database storage	EBS	RDS storage
Document storage	S3	S3
Initial complexity	Lower	Higher
Expected operating costs	Potentially lower	Potentially higher
Server administration	Required	Reduced
Future scaling	Requires additional configuration	Supports managed container scaling

Architecture Option A appears suitable for a small initial deployment.

Architecture Option B provides an alternative approach using managed services.

No final architecture decision has been made as part of Week 1.

⸻

5. Risks and Open Questions

The following items require further evaluation:

1. AWS Costs: Actual infrastructure costs depend on selected services, resource sizes, and deployment duration.
2. Application Compatibility: The existing application’s programming language and database technology are unknown.
3. Availability: A single EC2 instance or a single ECS Task does not provide full high availability.
4. Backup Strategy: Backup frequency, retention, RPO, and RTO have not been finalized.
5. Security: Application authentication and document-level authorization require further definition.
6. Network Complexity: Architecture B may require additional network components for private ECS Tasks.
7. Persistent Storage: Database persistence and recovery procedures must be tested during implementation.

⸻

6. Plan for Week 2

Week 2: November 2–6, 2026

The next phase focuses on selecting an architecture and building the basic AWS infrastructure.

Planned tasks:

1. Compare Architecture A and Architecture B.
2. Evaluate cost, security, reliability, storage durability, and operational complexity.
3. Select the final architecture.
4. Document the architecture decision and main trade-offs.
5. Create 3–5 Architecture Decision Records (ADRs).
6. Prepare the final architecture diagram.
7. Develop the Terraform configuration.
8. Provision the initial AWS networking, compute, and storage resources.
9. Configure basic infrastructure security.
10. Test basic connectivity and review AWS costs.

Infrastructure deployment will use Terraform.

Docker will be used for application containerization.

⸻

7. Weekly Summary

During Week 1, the main focus was understanding the business requirements and designing two possible AWS infrastructure solutions.

The proposed architectures separate application execution, persistent application data, and uploaded document storage.

Architecture A uses Amazon EC2 and Docker Compose with PostgreSQL on EBS.

Architecture B uses Amazon ECS Fargate with Amazon RDS PostgreSQL.

Both architectures use Amazon S3 for document storage.

The architecture analysis provides the foundation for selecting and implementing the final AWS infrastructure during Week 2.

Next Step: Architecture comparison, final architecture decision, and Terraform implementation.