### Part-1: Threat Modeling and Report:
### 1.1 Data Flow Diagrams:
### Sensitive Data Access DFD:
![Sensitive Data Access](/DataFlow/SensitiveDataFlow.png)
[Sensitive Data Access Threat Model Report](https://htmlpreview.github.io/?https://github.com/sanne88/cyber8420projectopenmrs/blob/main/DataFlow/SensitiveDataAccessDFD.html)

### Authentication DFD:
![Authentication](/DataFlow/Authentication-DFD.png)
[Authentication Threat Model Report](https://htmlpreview.github.io/?https://github.com/sanne88/cyber8420projectopenmrs/blob/main/DataFlow/Authentication-report.htm)

### Part-2: Observation:

### 2.1 Open MRS Design Issues:

### Sensitive Data Access Flow:

The OpenMRS system involves various external actors, such as patients, staff, doctors, and administrators, each interacting with the system to access or store diverse sensitive data. OpenMRS functions as the central process responsible for managing all requests and responses from these external users. During appointment visits, critical and sensitive patient data, including demographics, vitals, reports, and pharmacy information, is stored or accessed, constituting a vulnerable data flow. External entities connect with the OpenMRS API process over the internet boundary, while the OpenMRS API process interacts with the SQL database across the database boundary. Additionally, the OpenMRS system records all interactions initiated by external actors or the database in the log files.

Below is the list of issues observed in the threat model report:
Total	36
Needs Investigation	10
Mitigation Implemented	26

In summary, the issues that needed attention can be categorized as below

Spoofing attacks:

While OpenMRS utilizes OAuth 2 as its authentication mechanism, the absence of Multifactor Authentication (MFA) raises concerns as MFA can provide an additional layer of security, enhancing the overall resilience of the system and thereby preventing Spoofing attacks on the system. A thorough examination did not reveal concrete evidence of MFA implementation.

Denial of Service Attack: 

Additionally, there is a lack of evidence regarding the existence of a comprehensive log monitoring and alarm system within the system. Such a system is crucial for various scenarios, including incident response, user activity monitoring, and overall system performance monitoring. Implementing robust log monitoring ensures timely detection of suspicious activities and excessive usage of system resources contributing to a proactive security stance. This is helpful in preventing the DOS attack that happens because of potential process crashes.

Denial of Service Attack on SQL Database : 

The existence of the SPIKE ticket, as documented in https://wiki.openmrs.org/display/docs/Support+for+Clustering, provides clear indications of a roadmap in progress for both the web app server, but not enough information on the database clustering. While Docker deployments for the database cluster have been identified, additional investigation is required to understand the specifics of the distributed architecture implementation.
Implementing a distributed architecture by setting up a cluster of database nodes helps in mitigating the Denial Of Service attack on the database. This involves having multiple database servers working together to share the load and provide redundancy. Utilize a load balancer to distribute incoming database requests across the cluster nodes evenly. This ensures that no single node becomes a bottleneck and enhances the overall performance and availability of the database.

### Authentication Data Flow:

Report Summary:
* Not Applicable: 14
* Needs Investigation: 9
* Mitigation Implemented: 38
* Total: 61

Authentication Data Flow involves three services: the client/user interface, the API server, and the database server. Some threats are common to both DFDs, as they involve API and database interactions. All significant attacks have been mitigated by OpenMRS, while some threats need investigation.

Denial of Service Attack:
The availability of all three services when affected by crashing depends on the nature of implementation and requires further investigation post-deployment.

Spoofing:
The trust boundary between services is established through secure configurations, and an acceptable authentication mechanism is used for access.

Elevation of Privilege:
OpenMRS ensures proper authorization and role-based access, but the lack of Multi-factor authentication is a drawback.

In summary, the authentication data flow is secured using acceptable mechanisms such as encryption and role-based access.

### Project Board

[Project Board](https://github.com/users/sanne88/projects/1)

### 2.2 Team Reflection:

Initially, a few of the team members had questions about what to deliver in the data flow diagram and how much detail was needed. There was some confusion about the data flow vs control flow. The weekly team check-in with Dr. Gandi helped to clarify the deliverable. First drafts of DFDs were developed and reviewed during Zoom meetings and tasks were defined and assigned to the team members to complete the assignment. The team kept revising the DFDs for the system of interest Open MRS Core.

Sahithi and Surya initiated the Internal team meeting brainstormed with other team members and identified the data flow use cases. They worked on the Sensitive data access flow of the system.  Gopinath and Vidya worked on generating the threat modeling report for authentication data flow. Gopinath also reviewed the data access DFD and suggested a few possible considerations.   

