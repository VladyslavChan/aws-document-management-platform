Business & Technical Requirements

1. Project Overview

Project: AWS Infrastructure for a Document Management Platform
Company: DocuWorks GmbH
Project Duration: 4 weeks
Technologies: AWS, Terraform, Linux, Python, Git/GitHub

DocuWorks GmbH is a small company that provides document-processing services to its customers.

The company currently operates an internal application on a single physical server. Employees use this application to upload, view, download, process, and search for customer documents.

The company plans to migrate its application to Amazon Web Services (AWS).

The goal is to design a simple, secure, reliable, and cost-conscious cloud infrastructure that supports the company’s daily operations and allows future growth.

⸻

2. Current Infrastructure Analysis

2.1 Current Environment

The existing infrastructure consists of a single physical server hosting the application and its data.

Application data and uploaded documents are stored on the same server.

Infrastructure configuration and backup operations are performed manually.

2.2 Identified Problems

ID	Problem	Description	Impact
P-01	Single Point of Failure	The application depends on one physical server.	A server failure may interrupt business operations.
P-02	Limited Scalability	The server has limited computing and storage capacity.	Future business growth may be difficult.
P-03	Shared Storage	Application data and documents are stored together.	Storage management and recovery are more complicated.
P-04	Manual Backups	Backups are performed manually.	Important data may not be backed up consistently.
P-05	Manual Configuration	Infrastructure is configured manually.	Recreating the environment may be time-consuming and error-prone.
P-06	Availability Risk	There is no described automatic failover mechanism.	Employees may temporarily lose access after a server failure.

2.3 Main Infrastructure Challenge

The main challenge is the dependency on a single physical server.

The new infrastructure should separate application components from persistent data and document storage, improve backup and recovery capabilities, and simplify infrastructure management.

⸻

3. Business Requirements

The new AWS solution must support the company’s business operations.

ID	Business Requirement	Priority
BR-01	Migrate the existing application to AWS.	High
BR-02	Allow employees to access the application over the network.	High
BR-03	Support uploading, viewing, downloading, processing, and searching documents.	High
BR-04	Protect important application data and uploaded documents.	High
BR-05	Provide a reliable backup and recovery mechanism.	High
BR-06	Reduce dependency on the existing physical server.	High
BR-07	Keep infrastructure and operational costs reasonable.	High
BR-08	Support gradual business growth.	Medium
BR-09	Avoid unnecessary architectural complexity.	High

⸻

4. Functional Requirements

Functional requirements describe what the application and infrastructure must do.

ID	Functional Requirement
FR-01	Employees must be able to access the application over the network.
FR-02	Employees must be able to upload documents.
FR-03	Employees must be able to view and download stored documents.
FR-04	Employees must be able to process documents and save processing results.
FR-05	Employees must be able to search for previously processed documents.
FR-06	Application data must remain available after an application server restart or replacement.
FR-07	Uploaded documents must be stored separately from the application environment where appropriate.
FR-08	The infrastructure must support backup and recovery of important data.
FR-09	Administrators must be able to manage the AWS infrastructure securely.
FR-10	Basic monitoring and logging must be available.
FR-11	Infrastructure must be reproducible using Terraform.

⸻

5. Non-Functional Requirements

5.1 Availability and Reliability

The application should remain available during normal business operations.

The infrastructure must provide a documented recovery approach in case of server failure.

A complex high-availability architecture is not required for the initial version.

5.2 Data Persistence

Application data and documents must not be lost during normal application restarts.

Persistent data must remain accessible or recoverable when the application server is replaced.

5.3 Security

The infrastructure must implement basic security controls:

* Restrict administrative access to authorized users.
* Apply least-privilege IAM permissions.
* Limit unnecessary public network access.
* Protect stored application data and documents.
* Use appropriate encryption mechanisms.
* Protect credentials and sensitive configuration.
* Prevent unauthorized access to document storage.

5.4 Backup and Recovery

The solution must provide a reliable backup mechanism.

The backup strategy must define:

* Which data is backed up.
* How frequently backups are created.
* Where backup data is stored.
* How backups can be restored.
* How the system recovers after an application server failure.

At least one recovery operation must be demonstrated during the implementation phase.

5.5 Performance and Scalability

The infrastructure must support the initial workload of the company.

Parameter	Expected Usage
Number of employees	10–20
Documents per day	50–100
Average document size	Less than 10 MB
Application workload	Low to moderate
Expected growth	Gradual

The infrastructure should allow future capacity improvements without requiring a complete redesign.

5.6 Cost Efficiency

The infrastructure must be suitable for a small company with a limited budget.

AWS services should be selected based on actual requirements rather than unnecessary complexity.

The project must consider AWS Free Tier eligibility, available AWS credits, and potential ongoing costs.

5.7 Monitoring and Logging

The infrastructure must provide basic monitoring and logging.

It should be possible to monitor resource health, identify application or infrastructure errors, and investigate failures.

5.8 Infrastructure as Code

All final AWS infrastructure resources must be managed using Terraform.

Terraform should support:

* Infrastructure provisioning.
* Reproducible configuration.
* Infrastructure updates.
* Resource cleanup.

5.9 Containerization

The application will be packaged using Docker
to provide a consistent and reproducible
runtime environment.

The architecture must keep persistent application
data and uploaded documents separate from
the container's temporary filesystem.
⸻

6. Data Storage Requirements

The new architecture must consider two main categories of data.

6.1 Application Data

Application data includes information required for the application to operate, such as:

* Document metadata.
* Processing status.
* Processing results.
* Application records.

This data requires persistent storage and backup capabilities.

6.2 Document Storage

Document storage contains uploaded customer files, including PDF documents, images, and other supported file formats.

Document storage should be separated from the application environment where appropriate.

The storage solution must support secure document upload, retrieval, and recovery.

6.3 Storage Persistence

The selected storage solution must ensure that important data survives application restarts.

The architecture must also explain how persistent data can be accessed or restored after an application server replacement.

The selection of appropriate AWS storage services will be documented in the AWS Service Analysis.

⸻

7. Migration Requirements

The migration to AWS must consider the following components:

* Existing application and its configuration.
* Application data.
* Uploaded documents.
* Document metadata and processing results.
* Required network connectivity.
* Security and access configuration.

The existing database technology, total data volume, and detailed application dependencies are not specified in the project scenario.

These details must be treated as unknowns and documented in the Assumptions & Constraints file.

⸻

8. Proposed Acceptance Criteria

The following tests will be used to evaluate the implemented AWS solution.

Test	Expected Result
Application Access	The application can be accessed by an authorized user.
Document Upload	A test document can be uploaded successfully.
Document Retrieval	An uploaded document can be downloaded.
Data Persistence	Application data remains available after a restart.
Server Replacement	Persistent data can be reconnected or recovered.
Backup and Recovery	Important test data can be restored from a backup.
Security	Unauthorized access to protected resources is restricted.
Monitoring	Basic application or infrastructure metrics and logs are available.
Terraform Deployment	Infrastructure can be provisioned using Terraform.
Terraform Cleanup	Project infrastructure can be removed after testing.

These criteria will be validated during the implementation and testing phases.

⸻

9. Summary

The current infrastructure of DocuWorks GmbH depends on a single physical server, has limited capacity, and uses manual backup and configuration processes.

The proposed AWS infrastructure must provide:

* Reliable application hosting.
* Persistent application data.
* Appropriate document storage.
* Backup and recovery capabilities.
* Basic security controls.
* Monitoring and logging.
* Reproducible infrastructure using Terraform.
* Cost-conscious operation.

These requirements will be used to evaluate two alternative AWS architectures.

The final architecture will be selected during Week 2 based on cost, security, reliability, storage durability, operational complexity, and future growth.