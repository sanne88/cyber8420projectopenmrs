### Part-1: Threat Modeling and Report:
### 1.1 Data Flow Diagrams:
### Sensitive Data Access DFD:
![Sensitive Data Access](/DataFlow/SensitiveDataFlow.png)
![Sensitive Data Access Threat Model Report](/DataFlow/SensitiveDataAccessDFD.htm)

### Part-2: Observation:

### 2.1 Open MRS Design Issues:

### Sensitive Data Access Flow:

The OpenMRS system involves various external actors, such as patients, staff, doctors, and administrators, each interacting with the system to access or store diverse sensitive data. OpenMRS functions as the central process responsible for managing all requests and responses from these external users. During appointment visits, critical and sensitive patient data, including demographics, vitals, reports, and pharmacy information, is stored or accessed, constituting a vulnerable data flow. External entities connect with the OpenMRS API process over the internet boundary, while the OpenMRS API process interacts with the SQL database across the database boundary. Additionally, the OpenMRS system records all interactions initiated by external actors or the database in the log files.

Below is the list of issues observed in the threat model report:
Total	36
Needs Investigation	10
Mitigation Implemented	26

In summary the issues that needed attention can be categorised as below

Spoofing attacks:

While OpenMRS utilizes OAuth 2 as its authentication mechanism, the absence of Multifactor Authentication (MFA) raises concerns as MFA can provide an additional layer of security, enhancing the overall resilience of the system and thereby preventing the Spoofing attacks on the system. A thorough examination did not reveal concrete evidence of MFA implementation.

Denial of Service Attack: 

Additionally, there is a lack of evidence regarding the existence of a comprehensive log monitoring and alarm system within the system. Such a system is crucial for various scenarios, including incident response, user activity monitoring, and overall system performance monitoring. Implementing robust log monitoring ensures timely detection of suspicious activities and excessive usage of system resources contributing to a proactive security stance.This is helpful in preventing the DOS attack that happens because of potential process crash.

Denial of Service Attack on SQL Database : 

The existence of the SPIKE ticket, as documented in https://wiki.openmrs.org/display/docs/Support+for+Clustering, provides clear indications of a roadmap in progress for both the web app server , but not enough information on the database clustering. While Docker deployments for the database cluster have been identified, additional investigation is required to understand the specifics of the distributed architecture implementation.
Implementing a distributed architecture by setting up a cluster of database nodes helps in mitigating the Denial Of Service attack on database. This involves having multiple database servers working together to share the load and provide redundancy.Utilize a load balancer to evenly distribute incoming database requests across the cluster nodes. This ensures that no single node becomes a bottleneck and enhances the overall performance and availability of the database.

### 2.2 Team Reflection:



