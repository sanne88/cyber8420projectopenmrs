### Part-1: Top Level Claims:

1. Login - Session hijack
2. Data Management
3. Patient Sharing Records

### Part-1: Breadth and Depth of Assurance Arguments:

### 1.1 Login Session Hijack Assurance Case #1:

**Diagram**:

![Assurance_Case_1](/AssuranceCases_Diagrams/Claim1.PNG)

**Assessment**:

This top-level claim starts by OpenMRS minimizing the theft of session cookies which is solved by OWASP CSRFGuard minimizes the theft of session cookies. OWASP suggests to have CSRF protection in applications and also suggests a way to do it along with a version vulnerablitiy of CSRFGuard as justification. An Open Source contributor to OpenMRS delved into the code to find CSRFGuard version 4.1.1 is being used and the httpOnly attribute is set to true which provides evidence for the top-level claim and C2. An Open Source contributor also found OWASP states that any XSS can be used to defeat all CSRF mitigations which is justification for a rebuttal to make Sub Claim C3. C3 is an unfinished and open claim left to be investigated. 

### 1.2 Data Management Assurance Case #2:

**Diagram**:

![Assurance_Case_2](/AssuranceCases_Diagrams/DataManagement_AssurnaceCase.png)

**Assessment**:

The OpenMRS system allows administrators to merge patient records and manage patient data.
The top level claim of data management usecase is ensure keep patient information safe and private, ensuring patient data privacy, confidentiality, and integrity.

The major concern is that data tampering might occur.The subclaims focus on ensuring that only authorized and privileged users can access critical features. Based on our code analysis, all the features are secured with role based access control.This is implemented with "restrictbyrole" module and integrated with core system. The code shows that the API layers are secured with role-based access control using attribute "AuthorisedAnnotation". Additionally, the system prevents excessive redirects when multiple login attempts are made, enhancing security.

The confidentiality of patient data and the possibility of data breaches are two additional concerns.
The current system encrypts all patient data and sensitive information during network communications. There's substantial evidence in the code demonstrating the use of AES encryption and encoding of URI components to protect patient data.

However , there is still a risk that a staff user can gain access to the admin role , so implementing multi-factor authentication for critical features is suggested to safeguard patient data.Patient data breach prevention is essential, especially in different regions with varying standards. The HIPAA standard is not yet fully implemented but is in progress to ensure patient data privacy.

### 1.3 Patient Sharing Records Assurance Case:
**Diagram**:

![Assurance_Case_3](/AssuranceCases_Diagrams/SharingRecords_AssuranceCase.png)

**Assessment**:

In OpenMRS, patients can grant access to trusted individuals to view and access their records. The process begins with patients identifying whom they want to share their details with. Patients are required to provide the name, relationship type, and email address of the trusted person. Once this information is collected, an invitation link embedded with a shared token is sent to the trusted person. The top-level claim asserts that OpenMRS minimizes the risk of this invitation link being tampered with. 
	
However, potential issues include the link being manipulated by a hacker or an unauthorized person gaining access to the email, or the shared token being guessed through a brute force attack. These concerns are addressed through counterarguments supported by evidence:
- The shared tokens have a validity period of only 10 minutes. This information was verified from the code repository.
- Authentication and Validation: OpenMRS delegates authentication to OAuth 2.0. This fact was noticed in the code, and supporting documents were found to substantiate this claim. 
- Additionally, the shared tokens are securely stored in the phr_sharing_token table and transmitted in encrypted forms

###  Part-2 Evidence Alignment Observations:
### Planning & Reflection
