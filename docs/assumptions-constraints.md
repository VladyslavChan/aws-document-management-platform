Assumptions & Constraints

1. Purpose

This document defines the assumptions, constraints, and open questions for the AWS Infrastructure for a Document Management Platform project.

The project is based on the infrastructure requirements of DocuWorks GmbH.

Some technical details of the existing environment are not provided in the project scenario. Therefore, assumptions are necessary to support the initial architecture design.

These assumptions will be reviewed and updated during the implementation phase.

⸻

2. Confirmed Project Information

The following information is provided in the project requirements.

ID	Information	Description
F-01	Company	DocuWorks GmbH
F-02	Current infrastructure	One physical application server
F-03	Existing storage	Application data and documents stored on the same server
F-04	Backup process	Manual backups
F-05	Employees	10–20
F-06	Document volume	50–100 documents per day
F-07	Average document size	Less than 10 MB
F-08	Application load	Low to moderate
F-09	Business growth	Gradual growth expected
F-10	Project duration	Four weeks

The company wants a reliable, secure, and cost-conscious AWS solution without unnecessary complexity.

⸻

3. Technical Assumptions

The following assumptions are proposed for the initial architecture design.

They are not confirmed properties of the existing infrastructure.

A-01: Application Environment

The application is assumed to be compatible with a Linux-based environment.

For the project demonstration, a simple Python web application may be developed if the original application is not available.

A-02: Containerization

Docker will be used to package the demonstration application.

Containerization should provide a consistent application environment across development and AWS deployment.

Two deployment approaches will be evaluated:

* Docker with Docker Compose on Amazon EC2.
* Container deployment using Amazon ECS with AWS Fargate.

Docker is an additional project design choice and is not explicitly required by the original project brief.

A-03: Application Database

PostgreSQL is proposed for storing structured application data.

Possible records include:

* Document metadata.
* Document processing status.
* Processing results.
* Document storage references.

The database technology used by the existing company is unknown.

PostgreSQL must therefore be considered a proposed solution rather than a confirmed existing dependency.

A-04: Document Storage

Amazon S3 is proposed for storing uploaded documents separately from the application environment.

The application is expected to communicate with S3 using AWS SDK or supported S3 API operations.

Compatibility with the existing application must be verified before an actual migration.

A-05: Persistent Storage

Important application data must not depend on the temporary filesystem of a Docker container.

Possible persistent storage approaches include:

* Amazon EBS for a database running on EC2.
* Amazon RDS for a managed PostgreSQL database.
* Amazon S3 for uploaded document files.

The final storage design will depend on the selected architecture.

A-06: AWS Region

The AWS Region eu-central-1 (Frankfurt) is proposed for the project.

This is a preliminary choice based on the company’s location in Germany.

Any specific data residency, legal, or compliance requirements must be verified separately.

A-07: Availability

A single-Availability-Zone deployment may be sufficient for the initial demonstration.

The company does not require a complex enterprise architecture in the project scenario.

However, a Single-AZ design introduces availability risks that must be documented.

A-08: Application Access

The application is expected to be accessed by a small number of authorized employees through a web browser.

The exact authentication system and user permission model are not specified.

Authentication and authorization requirements must be clarified before production use.

A-09: Infrastructure Deployment

Terraform will provision and manage the AWS infrastructure.

Docker will package and run the application, while Terraform will manage infrastructure resources.

Application deployment and infrastructure provisioning are separate responsibilities.

A-10: Monitoring and Logging

Amazon CloudWatch is proposed for basic infrastructure monitoring and application logging.

The exact monitoring configuration will be defined during implementation.

A-11: Backup and Recovery

The architecture will include a backup and recovery mechanism for important persistent data.

Possible mechanisms include:

* EBS snapshots or database-aware backups.
* RDS automated backups.
* S3 Versioning and appropriate data protection policies.

Backup frequency, retention periods, RPO, and RTO are not specified in the project scenario.

These values require further definition.

⸻

4. Project Constraints

Constraints are known conditions that limit the design and implementation.

ID	Constraint	Impact
C-01	Four-week project duration	The implementation must remain manageable.
C-02	Small company	Infrastructure should match a workload of 10–20 employees.
C-03	Limited budget	Avoid unnecessary expensive AWS resources.
C-04	AWS required	The final infrastructure must run on AWS.
C-05	Terraform required	Infrastructure must be reproducible using Terraform.
C-06	Persistent storage required	Important data must survive application restarts or replacement.
C-07	Separate document storage	Document storage must be evaluated independently from application storage.
C-08	Backup and recovery required	At least one recovery operation must be demonstrated.
C-09	Basic security required	Administrative and data access must be restricted.
C-10	Monitoring required	Basic monitoring and logging must be available.
C-11	Cost-conscious testing	Infrastructure should be deployed for testing and destroyed afterward.

4.1 Cost Constraints

The project should consider available AWS credits and applicable Free Tier conditions.

Potential costs include:

* Compute resources.
* Block and database storage.
* S3 storage and requests.
* Network resources and data transfer.
* Monitoring and backup storage.

AWS costs must be reviewed before deployment.

Resources must be cleaned up after the final demonstration.

4.2 Complexity Constraints

The solution should not use additional AWS services only to make the architecture appear more advanced.

Service selection must be justified by actual business and technical requirements.

4.3 Security Constraints

No AWS access keys, database passwords, or application secrets may be committed to GitHub.

Access to AWS resources must follow the principle of least privilege.

Production-like use of customer documents requires additional consideration of data protection and access control.

⸻

5. Open Questions

The following questions cannot be answered from the current project scenario.

ID	Question	Why It Matters
Q-01	Which operating system is currently used?	Determines application migration compatibility.
Q-02	Which programming language and framework does the existing application use?	Affects deployment and containerization.
Q-03	Which database technology is currently used?	Affects database migration.
Q-04	How much historical document data exists?	Affects storage sizing and migration costs.
Q-05	What are the required RPO and RTO values?	Determines the backup and recovery strategy.
Q-06	What authentication system is currently used?	Affects application security design.
Q-07	Are there specific document retention or compliance requirements?	Affects storage and security policies.
Q-08	Is planned maintenance downtime acceptable?	Affects availability requirements.
Q-09	Can the existing application be modified to use Amazon S3?	Determines application integration complexity.

These open questions should be considered during architecture selection.

⸻

6. Initial Risks

ID	Risk	Proposed Mitigation
R-01	Unexpected AWS costs	Review pricing, configure billing alerts, and destroy test resources.
R-02	Loss of persistent data	Use suitable persistent storage and test backup recovery.
R-03	Single Point of Failure	Document availability limitations and recovery procedures.
R-04	Unauthorized access	Apply IAM, network restrictions, and application access controls.
R-05	Docker container data loss	Store important data outside the container’s temporary filesystem.
R-06	Application incompatibility	Validate application requirements before migration.
R-07	Configuration errors	Use Terraform and version-controlled configuration.
R-08	Incomplete backup recovery	Perform and document at least one recovery test.

⸻

7. Architecture Design Considerations

Two possible AWS architectures will be evaluated.

Architecture Option A

A simpler solution based on:

* Amazon EC2.
* Docker and Docker Compose.
* PostgreSQL with persistent EBS storage.
* Amazon S3.
* IAM and Security Groups.
* CloudWatch.
* Backup and recovery mechanisms.

This option prioritizes simplicity and cost control.

Architecture Option B

A more managed and container-oriented solution based on:

* Amazon ECS with AWS Fargate.
* Docker container images.
* Amazon ECR.
* Amazon RDS PostgreSQL.
* Amazon S3.
* Application Load Balancer.
* IAM and Security Groups.
* CloudWatch.
* Backup and recovery mechanisms.

This option prioritizes separation of responsibilities and reduced infrastructure administration.

Both architectures are proposals for evaluation, not final implementation decisions.

⸻

8. Summary

The project scenario provides the main business requirements but does not specify all technical details of the existing environment.

The proposed assumptions provide a starting point for evaluating suitable AWS services and designing alternative architectures.

Docker will be included as an additional technology for application containerization.

Persistent application data and uploaded documents must remain independent of the application container lifecycle.

The final architecture will be selected during Week 2 after comparing cost, security, reliability, storage durability, operational complexity, and future growth.

All assumptions should be reviewed before production deployment.