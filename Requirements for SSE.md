### Part1: Use/Misuse Case Analysis

To generate use cases and misuse cases for our system of interest, we've undertaken an in-depth analysis of the openMRS application software. This analysis involved the identification of the core system features along with the associated actors and enabling systems. We've identified six main features as the focal points of our analysis:

1. **Login/Authentication**: This fundamental feature ensures secure access to the system, authenticating users and safeguarding sensitive information.

2. **Data Management**: The system's data management capability plays a pivotal role in handling and preserving patient information efficiently and securely.

3. **Patient Sharing Records**: This feature allow patients to share their own personal health related records to another authorised person.

4. **API Access**: The application's API access functionality allows for integration with external systems and services, necessitating robust security measures.
5. **Feature5**:
6. **Feature6**:

For each of these features, we have created comprehensive use case and misuse case diagrams. These diagrams not only outline the existing functionalities of the system but also incorporate essential security considerations. These security measures are designed to preemptively address potential vulnerabilities and possible attacks, thereby ensuring the integrity and confidentiality of the system's data and operations.

### 1.1 Login Usecase:

Usecase: This openMRS application user (Super Admin/Pharamcy User/ Patient) wants to login to the system.

Misusecase: A rogue user can crack the credentials of an user and gain access to the system.

Diagram:

![Login usecase](/Usecase_Diagrams/Login.png)

Description:

The assessment for the login use case within our system places a strong emphasis on security and robust credential protection. In this use case, the process of logging in involves a multi-layered security approach. Firstly, when a user's password is entered, it undergoes encryption, which incorporates an added layer of security by introducing a random salt before hashing the password. This method significantly strengthens the password and serves as a vital deterrent against brute force and rainbow attacks.

Furthermore, to mitigate the risk of dictionary attacks on commonly used passwords, the system has an option to enforce a stringent password policy, ensuring that users create strong and unique passwords. Making this non-optional for a production environment can add more robustness to password protection. To fortify the password update process, a secret question is employed to validate the user's identity. Currently, one secret question is utilized, but it is advisable to incorporate multiple questions for added security.

To safeguard the login process during data transmission, the use of HTTPS and TLS encryption protocols is enforced. These measures are pivotal in preventing man-in-the-middle attacks, guaranteeing the confidentiality and integrity of user credentials as they traverse the network. In essence, this comprehensive security approach ensures that the login use case is fortified against various forms of unauthorized access and cyber threats, bolstering the overall security posture of our system.

### Data Management

**Actor** : Super Admin
**Use Case** :
1.  As a Admin , i want to merge the patient reports.
2.  As an Admin i want to manage the system configurations.
   
**Mis Actor** : Rob , The Rogue Admin

**Mis Use Case** :
1.  As a rouge superadmin ,i want to save unauthorized, falsified, or confidential patient data into the system, without the patient's consent or any legitimate medical reason.
2.  As a user i want to gain access to elevate privliges and collect data.

**Diagram** :
![Data Management usecase](/Usecase_Diagrams/DataManagement.png)

**Assessment** :

The core component of OpenMRS plays a pivotal role in the system, enabling superadmins to manage system configurations, merge patient reports, and oversee patient data. However, this critical functionality also presents a potential vulnerability, as rogue administrators could exploit it for data tampering or unauthorized data collection.

To counter these threats, the current system incorporates role-based access control to restrict unauthorized actions. Additionally, it employs AES data encryption to secure data during transmission across networks. The system also features logging and auditing mechanisms to detect unusual activities and alert relevant users.

The system permits superadmins to upload or download data, which could pose a security risk. To address this, implementing a global data privacy policy that requires user consent before any data upload or download activity can mitigate data collection attacks.

Furthermore, an extra layer of security can be introduced to the system, requiring additional approval for critical actions, such as managing system configurations or attempting to gain superadmin access. Implementing a multifactor access code can help prevent data tampering attacks in such scenarios.

### 1.3 Patient Sharing Records:
**Use Case:**
A patient shares his/her own personal records with his/her own friend.

**Misuse Case:**
Rob, the thief can manipulate the invite link received by an innocent user to steal his/her sensitive information.

**Diagram** :
![Sharing Records](/Usecase_Diagrams/SharingRecords.png)

**Assessment:**
In this scenario where a patient intends to share their personal medical records with a trusted friend, the process involves initiating a new relationship within the system. This involves providing essential information about the friend, including their name, email address, and the nature of their relationship with the patient. Subsequently, an invite link is generated, embedding a secure shared token, and sent to the designated friend.

Upon the friend clicking the provided link, it triggers the opening of a login window, preloaded with the shared token. At this point, two actions become possible: the friend can either log into the system if they are already an existing user, or they can proceed with the sign-up process. In either case, the shared token must be authenticated before any access to the system is granted.

Rob, the thief may attempt to manipulate the invite link to gain unauthorized access to sensitive information. The use of shared tokens prevents such unauthorized access by requiring the friend to possess and validate the token. Moreover, introducing an expiration time for the invite link enhances security further, ensuring that the link becomes inactive after a specified period.

Additionally, depending solely on a single factor, such as email, for validation is considered less robust. To bolster security even more, the implementation of multi-factor authentication is recommended, providing an additional layer of protection against potential attacks initiated by individuals like Rob.

### 1.4 API User :
**Use Case**
The primary function of this API use case is to efficiently fetch or retrieve information regarding patients and staff.

**Misuse Case**
There exists a potential threat in the form of Rob, an ill-intentioned thief. Rob's capabilities include manipulating the system and deploying various attack techniques with the intention of accessing unauthorized transactions, confidential patient records, and other sensitive data.

**Diagram**
![API User Usercase](/Usecase_Diagrams/API_User.png)

**Assessment:**
Assessment: In the delineated scenario, when an API user attempts to either fetch or store data related to financial transactions, staff particulars, or patient details, several protective measures are already in place. These measures encompass:

Parameterized Queries: These serve as the system's primary defence mechanism, safeguarding data against malicious input or injections.
Availability, Checksums, and Hashes: These mechanisms ensure data integrity and system uptime, verifying that the fetched or stored information remains uncompromised.

To bolster security further, additional enhancements are being incorporated:

Encoding URI Parameter: Integrated primarily to fortify parameterized queries, this feature prevents the potential confusion or misinterpretation of characters within URLs.
Such a step is crucial in averting certain attack vectors that exploit URL vulnerabilities.

Rate Limiting: By restricting the frequency of requests to the API within specified time frames, this feature helps in preventing overloads or potential denial-of-service (DoS) attacks.

Secure Hash Functions: These cryptographic functions ensure that data, especially sensitive data, is encrypted, thus safeguarding it from prying eyes or unauthorized access.

If Rob, or any other malicious actor, attempts to compromise the system using techniques like SQL injection, denial-of-service attacks, or inducing data corruption, our layered security approach—including Encoding URI Parameter, Rate Limiting, and Secure Hash Functions—will serve as a robust shield, significantly reducing the risk of breaches.


### Part 2: OSS project documentation review
We found the following documents:
- Installation Guides 
    - [Openmrs-core](https://github.com/openmrs/openmrs-core)
    - [All installation guides](https://wiki.openmrs.org/display/docs/All+Installation+Guides)
- Security related documentation
[Security Documentation](https://wiki.openmrs.org/display/docs/Security)

As an application that handles highly sensitive information, OpenMRS enforces strict security policies and practices. They have categorized their approach to security into three perspectives:
1. OpenMRS Community Tools:
These tools are managed by a community of OpenMRS support engineers and volunteers.

2. OpenMRS Production Implementation in the Real World:
This aspect deals with handling of OpenMRS production implementations.

3. OpenMRS Community Software Project:
This involves the actual community-maintained software through practices like code review, code scanning, role-based access control, etc.

OpenMRS has a dedicated security group that works on security incidents and addresses these issues. However, ensuring security is an ongoing practice. In case of any security concerns, there are two ways to report them:

1. Listing the incident on Github using Private Vulnerability Report Feature. [Learn more](https://wiki.openmrs.org/pages/viewpage.action?pageId=224526637#OpenMRSSecurity101:Policies&VulnerabilityManagement-2.AssessmentofIncident)
2. Emailing [security@openmrs.org](mailto:security@openmrs.org)

A few critical open issues that we noticed are:

|Ticket|Type|Description|Created|
|------|------|------|------|
|[TRUNK-6155](https://issues.openmrs.org/browse/TRUNK-6155)|Task|Upgrade Liquibase|2022-12-15|
|[TRUNK-6154](https://issues.openmrs.org/browse/TRUNK-6154)|Task|Few endpoints at the API are publicly available without any authorization|2022-12-14|
|[TRUNK-6043](https://issues.openmrs.org/browse/TRUNK-6043)|Bug|Blind time-based SQL injection vulnerability|2021-10-29|
|[TRUNK-6041](https://issues.openmrs.org/browse/TRUNK-6041)|Bug|Vulnerability in scripts to generate Liquibase file|2021-10-19|


The Security Group reviews the incidents and categorizes them according to severity. Depending on the severity, appropriate actions are taken for containment, eradication, and recovery. The affected audience is notified within 5 business days or without undue delay.

The documentation includes steps to secure the OpenMRS Application from various angles. Some instances where we found incomplete or no documentation include:
1. Managing user persmissions.
2. Maintaining Database backups.
3. Auditing and Logging.
4. Information about the third party dependency management.
5. Dockerization.
