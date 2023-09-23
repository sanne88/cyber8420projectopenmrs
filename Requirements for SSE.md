### Part1: Use/Misuse Case Analysis

To generate use cases and misuse cases for our system of interest, we've undertaken an in-depth analysis of the openMRS application software. This analysis involved the identification of the core system features along with the associated actors and enabling systems. We've identified six main features as the focal points of our analysis:

1. **Login/Authentication**: This fundamental feature ensures secure access to the system, authenticating users and safeguarding sensitive information.

2. **Data Management**: The system's data management capability plays a pivotal role in handling and preserving patient information efficiently and securely.

3. **Patient Registration**: The patient registration feature is essential for capturing and organizing patient details, a critical aspect of healthcare record management.

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
3.  As a rouge superadmin ,i want to save unauthorized, falsified, or confidential patient data into the system, without the patient's consent or any legitimate medical reason.
4.  As a user i want to gain access to elevate privliges and collect data.

**Diagram** :

**Description** :

The core component of OpenMRS plays a pivotal role in the system, enabling superadmins to manage system configurations, merge patient reports, and oversee patient data. However, this critical functionality also presents a potential vulnerability, as rogue administrators could exploit it for data tampering or unauthorized data collection.

To counter these threats, the current system incorporates role-based access control to restrict unauthorized actions. Additionally, it employs AES data encryption to secure data during transmission across networks. The system also features logging and auditing mechanisms to detect unusual activities and alert relevant users.

The system permits superadmins to upload or download data, which could pose a security risk. To address this, implementing a global data privacy policy that requires user consent before any data upload or download activity can mitigate data collection attacks.

Furthermore, an extra layer of security can be introduced to the system, requiring additional approval for critical actions, such as managing system configurations or attempting to gain superadmin access. Implementing a multifactor access code can help prevent data tampering attacks in such scenarios.
