### Part-1: Top Level Claims:

1. Login - Authentication
2. Login - Session hijack
3. Data Management
4. Patient Sharing Records
5. API handling
### Part-1: Breadth and Depth of Assurance Arguments:

### 1.1 Login - Authentication Assurance Case #1:

**Diagram**:

![Assurance_Case_3](/AssuranceCases_Diagrams/Authentication_AssuranceCase.png)

**Assessment**:

Authentication is the most important assurance case to be addressed. The top-level claim for this case is that the openMRS platform implements an acceptable authentication mechanism for user login functionality. The major concerns for an authentication mechanism are to ensure the credentials not being strongly encrypted before storage, weak password, unsecured data transmission, lack of password policies, and session hijacking. While the first 4 doubts are expressed in the diagram, session hijacking is addressed as a separate assurance case.

### 1.2 Login Session Hijack Assurance Case #1:

**Diagram**:

![Assurance_Case_3](/AssuranceCases_Diagrams/Claim1.PNG)

**Assessment**:

This top-level claim starts by OpenMRS minimizing the theft of session cookies which is solved by OWASP CSRFGuard minimizes the theft of session cookies. OWASP suggests to have CSRF protection in applications and also suggests a way to do it along with a version vulnerablitiy of CSRFGuard as justification. An Open Source contributor to OpenMRS delved into the code to find CSRFGuard version 4.1.1 is being used and the httpOnly attribute is set to true which provides evidence for the top-level claim and C2. An Open Source contributor also found OWASP states that any XSS can be used to defeat all CSRF mitigations which is justification for a rebuttal to make Sub Claim C3. C3 is an unfinished and open claim left to be investigated.

### 1.3 Data Management Assurance Case #2:

**Diagram**:

![Assurance_Case_3](/AssuranceCases_Diagrams/DataManagement_AssurnaceCase.png)

**Assessment**:

The OpenMRS system allows administrators to merge patient records and manage patient data.
The top level claim of data management usecase is ensure keep patient information safe and private, ensuring patient data privacy, confidentiality, and integrity.

The major concern is that data tampering might occur.The subclaims focus on ensuring that only authorized and privileged users can access critical features. Based on our code analysis, all the features are secured with role based access control.This is implemented with "restrictbyrole" module and integrated with core system. The code shows that the API layers are secured with role-based access control using attribute "AuthorisedAnnotation". Additionally, the system prevents excessive redirects when multiple login attempts are made, enhancing security.

The confidentiality of patient data and the possibility of data breaches are two additional concerns.
The current system encrypts all patient data and sensitive information during network communications. There's substantial evidence in the code demonstrating the use of AES encryption and encoding of URI components to protect patient data.

However , there is still a risk that a staff user can gain access to the admin role , so implementing multi-factor authentication for critical features is suggested to safeguard patient data.Patient data breach prevention is essential, especially in different regions with varying standards. The HIPAA standard is not yet fully implemented but is in progress to ensure patient data privacy.

### 1.4 Patient Sharing Records Assurance Case:

**Diagram**:

![Assurance_Case_4](/AssuranceCases_Diagrams/SharingRecords_AssuranceCase.png)

**Assessment**:

In OpenMRS, patients can grant access to trusted individuals to view and access their records. The process begins with patients identifying whom they want to share their details with. Patients are required to provide the name, relationship type, and email address of the trusted person. Once this information is collected, an invitation link embedded with a shared token is sent to the trusted person. The top-level claim asserts that OpenMRS minimizes the risk of this invitation link being tampered with.
However, potential issues include the link being manipulated by a hacker or an unauthorized person gaining access to the email, or the shared token being guessed through a brute force attack. These concerns are addressed through counterarguments supported by evidence:

- The shared tokens have a validity period of only 10 minutes. This information was verified from the code repository.
- Authentication and Validation: OpenMRS delegates authentication to OAuth 2.0. This fact was noticed in the code, and supporting documents were found to substantiate this claim.
- Additionally, the shared tokens are securely stored in the phr_sharing_token table and transmitted in encrypted forms

### Part-2 Evidence Alignment Observations:

### 2.1. Authentication Usecase

### 2.1.1 Available Evidences

OpenMRS has availoable evidences for password encryption and password change policy that includes a security question. TLS/HTTPS is the standard for application that is written on the official documentation.

### 2.1.2 Unavailable Evidences

Multi-factor authentication is not present and is assessed as the most commonly missing feauture. An additional security question can also be added to further strengthen the password change policy.

### 2.2. Assurance Case 2

### 2.2.1 Available Evidence

**E1 - 1: Role Based Access Control:**

The system services are managed according to user privileges and roles. In essence, the privileges associated with each role are all contained in the "role_privilege" table. The 'user_role' table contains a mapping between users and roles.
Additionally, an authorize attribute is added to each api. The generic library 'org.openmrs.annotation' can be used to verify all of the available attributes in the OpenMRS system.AuthorizedAnnotationAttributes and the related PrivilegeConstants for each privilege are linked to the user role.
Depending on the requirement, the attribute is applied at the method level as well as the class level.

**E2 : Prevent Illegal Redirection**

When repeated login attempts are made, this mechanism prevents unnecessary redirects and the maximum number of attempts is 5.All HTTP and HTTPS calls are managed by the procedure "openConnectionCheckRedirects," which also looks for any unnecessary redirections.

**E3 : AES Encryption**

By utilizing AES encryption, the Open mrs system illustrates the security procedures employed in "Security.java file".

Strong security measures against potential loss or theft are required since patient data confidentiality is of the utmost significance. By employing AES encryption, the system not only protects the data from eavesdropping but also verify its integrity. Any tampering or unauthorized modifications can be detected, ensuring the data's authenticity.Patient data is also safeguarded while stored in the database. Unauthorized access to the database or theft of stored data is rendered futile without the correct encryption keys.The use of an initialization vector (initVector) adds an extra layer of security by ensuring that identical plaintext inputs result in different ciphertext outputs. This prevents pattern recognition and enhances the overall security of the encryption

The system ensures the patient data integrity using the cipher configuration 'AES/CBC/PKCS5Padding' to encrypt the data into a string, employing specific initialization vectors (initVector) and secret keys (secretKey).

**E4 : Encoded URI Parameters**

The system demonstrates the utilization of content encoding within the 'WebUtil.java' file. This file primarily contain methods responsible for enforcing the encoding of CSS (encodeForCSS) , Javascript (encodeForJavaScriptSource), HTML (encodeForHtmlContent), and URI ('encodeForUri) data. These encoding measures serve as crucial safeguards, ensuring that the system remains shielded from various injection attacks. By implementing these encoding practices, the system guarantees that it does not inadvertently expose any sensitive patient data while facilitating communication between the front end and back end.

**E5: Secured Browser Session Data**

Session cookies are only stored for the duration of the user's session and are deleted when the user logs out or closes their browser.There is sufficient evidence in the file 'CookieClearingFilter' to support the above claim. Also 'HTTP-only' flag is set on to cookies which prevents client-side scripts from accessing sensitive data stored in cookies.The system uses encodeURIComponent , which is encoding the Cookie and storing the data and is cleared when user logsout of the application.

### 2.2.2 Unavilable / Insufficient Evidence

**E1 -2 Multi Factor Authentication**

The system doesnt support multi factor authentication, and there is no supporting evidence for this scenario.Hence there is still a risk that a staff user can gain access to the admin role , so implementing multi-factor authentication for critical features is suggested to safeguard patient data.

**E7: GDPR Compliance , HIPPA Standard**

Based on the available documentation https://wiki.openmrs.org/display/docs/Security+and+Access+Control, it appears that the system may not be fully equipped to adhere to data privacy standards, particularly with regard to the handling of data location. This raises concerns about the system's ability to meet the requirements of data privacy standards.
It is crucial to prioritize addressing these issues and enhancing the system's capabilities to ensure compliance with data privacy standards, especially HIPAA, GDPR which is of atmost importance in healthcare environments. Compliance with GDPR is essential to ensure patient data privacy and confidentiality.

### 2.3. Assurance Case 3

### 2.4. Assurance Case 4

**E1: Shared token validity**

The links are strong enough and are resistant to manipulation. But if by any chance, the links are manipulated, the system ensures that no harm arises by setting a token expiry. After the token expires, the request coming back to the system will be disregarded.

**E2: phr_sharing_token table**

When a patient enters details of another trusted person, and initiates the process for records sharing, all these details will be stored in the phr_sharing_token table. Hence even if an unauthorized person gains access to the invite email, the information entered will be validated against what is already stored in the phr_sharing_token table.

**E3: OAuth 2.0**

Documentations of OpenMRS states that the system delegates authentication to OAuth 2.0 framework. OAuth 2.0 is an industry standard protocol for authorization and authentication on the internet. It is mainly used to secure access to web services and APIs.

**E4: OAuth 2.0**

Documentations of OpenMRS states that the shared tokens are encrypted. Hence there is no possibility of brute force attack on guessing the shared token.

Source: [OpenMRS Patient Portal Module Documentation](https://wiki.openmrs.org/display/docs/Patient+Portal+Module+-+Personal+Cancer+Toolkit+Project+Revamp)
**2.5.Assurance Case 5**

**2.5.1 Evidence Available** :

**E1 - Results of Parameterized Queries**

Parameterized queries in OpenMRS, as in other systems, are essential for security, efficiency, and maintainability. They provide a robust mechanism to interact with the database safely, without exposing the system to potential threats.

**E2 - URI Parameter documentation review**

URI parameters in OpenMRS are a critical aspect of how the system navigates and fetches data. They offer flexibility and stateless operations but should be used carefully, especially considering patient data's sensitive nature.

**2.5.2 Evidence Unavailable** :

**E1 - Multi-factor Authentication**

Multi-factor authentication is a powerful tool to enhance the security of OpenMRS, implementing it requires careful planning to balance security needs with user convenience. It's an especially valuable consideration for healthcare settings where the stakes related to data breaches are particularly high

### Project Board Link

- [Project Board](https://github.com/users/sanne88/projects/1)

### Planning & Reflection

Team Bug Busters consists of Brian, Carl, Gopinath, Sahithi, Surya, and Vidya. The team is led by Sahithi.

Individual Contributions:

Sahithi handled the Super Admin Data management assurance case by analysing the open mrs code and updated the evidences for the data management use case. She also contributed to the evidence alignment observations and the Project Reflection and Planning sections. Sahithi took initiative of managing the project board, breaking down tasks into smaller components, and creating tickets for this exercise. She also organized internal team meetings.

Surya worked on the Patient handling records assurance case. She gathered information from OpenMRS code base and the OpenMRS documenations. She also contributed to the evidence alignment observations and the project planning and reflection sections.

Gopinath worked on the authentication usecase as well as reviewed other's work on github.

Vidya worked on API Assurance case .She fetched information from openMRS documentation about API and possible security layers. Also she proposed additional layer of security that can be possible for openMRS.
