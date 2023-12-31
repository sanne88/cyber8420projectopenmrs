### Part-1: Code Review
### 1.1 Code Review Strategy
The OpenMRS system has different parts, and we're specifically looking at the OpenMRS Core, which is a medium-sized code component. To make sure our code is secure, we followed a strategy for reviewing it. This strategy involves looking at different situations and potential dangers.

Our code review strategy includes both automated and manual methods. For automation, we used tools like GitHub CodeQL and Sonar Cloud to find possible issues in the code. For CodeQL, we had to fork the OpenMRS-core repository to run the code analysis. For SonarCloud, we signed up for the trial version of the tool.
After that, for each threat we identified, we manually went through the code. We started with each misuse case, digging into the specific file responsible for that part of the code.

This approach helped us narrow down our focus to just the OpenMRS Core, making sure it's as secure as possible.

## 1.2 CWE Checklist for the Manual Code Review Process

Before initiating the manual code review process, we have come up with this checklist derived from our previous assignments:

1. ☑️ **CWE-798: Use of Hard-coded Credentials**
2. ☑️ **CWE-352: Cross-Site Request Forgery (CSRF)**
3. ☑️ **CWE-311: Missing Encryption of Sensitive Data**
4. ☑️ **CWE-307: Improper Restriction of Excessive Authentication Attempts**
5. ☑️ **CWE-321: Use of Hard-coded Cryptographic Key**
6. ☑️ **CWE-327: Use of a Broken or Risky Cryptographic Algorithm**

### 1.3 Manual Code Review
		
### CWE-476: NULL Pointer Dereference
FileName: CachePropertiesUTil.java 
Line no 74,69, 83 Null pointerexception

If there are missing or null values for certain cache configurations in cacheProperties or any unexpected cache configurations, attempting to access these properties without proper null checks could result in a NullPointerException and can lead to application crash.

Mitigation: Implement appropriate null checks.

### CWE-352: Cross-Site Request Forgery (CSRF)

This issue is reported in the threat model generated for the Data flow use case and we did a manual code review and examined the code manually. 
In the system's code, specifically in the 'OpenmrsFilter.java' file, filters are in place for handling requests. These filters are designed to provide CSRF protection. Importantly, the 'OpenmrsFilter' is set up to execute before the 'CSRFGuard' filter. This order is specified in the 'Web.xml' file on lines 168 and 178.

Additionally, the CSRF file, which contains tokens to prevent forgery and ensure system safety, is served dynamically. Additionally, this file isn't cached to avoid using outdated tokens and to maintain the effectiveness of the security measures.

### CWE-311:Missing Encryption of Sensitive Data
FileName:UserLogin.java
From line 148,149,150
OpenMRS authentication system transmits credentials, such as usernames and passwords, over an unencrypted channel, it risks exposing these details to unauthorized parties who might intercept the data. This is particularly crucial for applications that transmit data over the internet. If implementing a custom authentication scheme (as in the **TestAuthenticationCredentials** class ), it's crucial to ensure that any sensitive data handled by the module is encrypted. This includes not just the credentials themselves, but also any related data that might be used in the authentication process.

### CWE-307: Improper Restriction of Excessive Authentication Attempts is not explicitly present in the code snippet itself.
FileName:AuthenticationUserSessionListener.java
From lines 22-51
It refers to the lack of mechanisms to prevent or mitigate brute force attacks, where an attacker tries multiple passwords or passphrases with the hope of eventually guessing correctly. The code provided focuses on logging authentication events (like login success or failure). Still, it does not show any functionality related to limiting the number of failed login attempts or implementing account lockout policies after a certain number of failed attempts. These are typical strategies used to combat brute force attacks.
In the context of this code:
•There is no mechanism shown for tracking the number of consecutive failed login attempts for a user.
•There is no implementation of timeouts or account lockouts after multiple failed attempts.
The absence of these mechanisms means that if this code is part of the larger authentication system, and if there are no other controls in place elsewhere to handle excessive authentication attempts, it could be vulnerable to brute force attacks as indicated by CWE-307. However, it's important to note that such controls might be implemented in other parts of the OpenMRS system or in its configuration settings, which are not visible in this specific code snippet.

### 1.4 Automate Code Review
Following are the results from running automated code analysis.
### GitHub CodeQL
![GitHub CodeQL](./CodeScanResults/CodeQL.png)

### SonarCloud
![SonarCloud](./CodeScanResults/SonarCloud.png)

### CWE-470: Use of Externally-Controlled Input to Select Classes or Code ('Unsafe Reflection')
Issue:
Filename: DatabaseUtil.java
Line No: 57, 58
The code in this file is prone to this vulnerability. This type of vulnerability occurs when an application allows an attacker to specify the classes to be loaded dynamically. In code, the class to be loaded (connectionDriver) is obtained from an external source which is passed as a method parameter. If the value of the connection driver is controlled by an external, untrusted source (such as user input or data from an untrusted network), it can lead to security issues. An attacker might manipulate this input to load a malicious class, which could potentially lead to various security risks, including code execution vulnerabilities. Line 58 also poses a potential threat for log injection attacks.
Mitigation: To mitigate this risk, input validation and sanitization need to be performed on the user or any externally provided input. Consider implementing security policies to restrict the classes that can be loaded dynamically based on certain criteria.

		
### CWE-321: Use of Hard-coded Cryptographic Key 
FileName:Security.Java 196, OpenMRSConstants.java 556,560

The default encryption key and initialization vector are hard-coded in the code (ENCRYPTION_VECTOR_DEFAULT and ENCRYPTION_KEY_DEFAULT). Hard-coded keys can pose a security risk, especially if they are not changed regularly.

On reviewing the code, the system demonstrates the dynamic generation of these keys in the Security.Java file in the method generateNewInitVector() 294.  Using SecureRandom instead of Random is designed to provide a higher level of security by using a cryptographically secure pseudo-random number generator (CSPRNG).

		
### CWE-611: Improper Restriction of XML External Entity Reference
Filename: UpdateFileParser.java, 77

Parsing untrusted XML files with a weakly configured XML parser may lead to an XML External Entity (XXE) attack. This type of attack uses external entity references to access arbitrary files on a system, carry out denial of service, or server-side request forgery. Even when the result of parsing is not returned to the user, out-of-band data retrieval techniques may allow attackers to steal sensitive data. Denial of services can also be carried out in this situation.

Mitigation:  Using an entity resolver that always returns an empty string can prevent the resolution of external entities, which is a good practice to mitigate XML External Entity (XXE) attacks.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
We noticed that CWE-22 was reported by both CodeQL and SonarCloud. This vulnerability is because the application uses external input to construct file paths which would be later accessed.
	
One of the instances where this vulnerability was reported was web/src/main/java/org/openers/module/web/WebModuleUtil.java:194. Zip slip is a type of vulnerability where an application extracts a file from an archive entry and uses it to construct file name paths without proper validation. The archive entry can include any string that includes ".." that can let attackers the restricted files too. This can lead to a directory traversal attack. To avoid this a good practice is to sanitize the input from the archive before using it further.
	
One more instance where this vulnerability is reported is on file 
web/src/main/java/org/openmrs/web/filter/StartupFilter.java:139. Here a file is accessed based on a path that is controlled by the user. This can lead to accessing, revealing or deleting critical information by attackers. Hence user input should be validated and sanitized before such usage.
	
This vulnerability is part of the top 25 report software vulnerabilities as reported by the CWE for the year 2023.

### CWE-327: Use of a Broken or Risky Cryptographic Algorithm
This vulnerability is introduced when the product uses a weak cryptographic algorithm. It was found on file api/src/main/java/org/openmrs/util/Security.java:242. Here the AES/CBC/PKCS5 padding algorithm is used. This a weak algorithm and should not be used since it can lead to decryption of sensitive data. This is a parent of CWE-328 which is also reported on the same instance of the same file. CWE-328 is focused on the hash used and it is generated when a weak hash is used. Hence it is suggested to use an up to date Cryptographic algorithm.

### Part-2: Key Findings and Contributions
### 2.1 Key Findings
We attempted both manual and automated code analysis on OpenMRS-core repository. Out of the suggested automated code analysis tools, we found GitHub CodeQL and SonarCloud suitable because of ease of integration with the OpenMRS-core repository. We forked the original repository and ran these tools on the forked repository. We scanned the results of our analysis and found these seven CWEs critical in our repository.
- [CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')](https://cwe.mitre.org/data/definitions/22.html)
- [CWE-307: Improper Restriction of Excessive Authentication Attempts](https://cwe.mitre.org/data/definitions/307.html)
- [CWE-311: Missing Encryption of Sensitive Data](https://cwe.mitre.org/data/definitions/311.html)
- [CWE-321: Use of Hard-coded Cryptographic Key](https://cwe.mitre.org/data/definitions/321.html)
- [CWE-327: Use of a Broken or Risky Cryptographic Algorithm](https://cwe.mitre.org/data/definitions/327.html)
- [CWE-470: Use of Externally-Controlled Input to Select Classes or Code ('Unsafe Reflection')](https://cwe.mitre.org/data/definitions/470.html)
- [CWE-611: Improper Restriction of XML External Entity Reference](https://cwe.mitre.org/data/definitions/611.html) 
 
In summary, the code review of the OpenMRS codebase reveled some of critical vulnerabilities across various files. One significant finding is related to the utilization of weaker or risky encryption methods, potentially compromising sensitive information and highlighting the need for a security update. Additionally, the authentication mechanism lacks measures to prevent brute force attacks, leaving the system vulnerable to multiple failed login attempts. At the database layer, there is an unsafe reflection due to dynamic class specification from external, untrusted sources. Instances were identified where weak XML parsing may lead to XML External Entity attacks. The web utility filters exhibit instances of CWE-22, indicating improper limitation of pathname and potential susceptibility to directory traversal attacks.

### 2.2 Contributions 

## Overview

This repository documents the findings and contributions of our team's security review of the OpenMRS system. Our analysis focused on both design and code aspects, aiming to enhance the security posture of the application.

## Design Contributions

In our examination of the design, we scrutinized Data Flow Diagrams, particularly for Sensitive Data Access and Authentication. The sensitive data access flow revealed 36 issues, of which 26 were mitigated, and 10 are under investigation. Notably, our findings emphasized the importance of implementing Multifactor Authentication (MFA) to counter potential spoofing attacks.

## Code Changes

Our manual code review resulted in targeted changes across specific files in the OpenMRS system. Notable modifications include the implementation of null checks in CachePropertiesUtil.java, enhancements to Cross-Site Request Forgery (CSRF) protection in OpenmrsFilter.java, and improvements in encryption practices in UserLogin.java. Additionally, DatabaseUtil.java underwent adjustments to mitigate externally controlled input risks. The Security.java file saw changes, favoring dynamic key generation over hard coding for improved security.

## Overall Contribution

Our combined efforts in design scrutiny and code-level enhancements contribute to a more robust and secure OpenMRS system. By addressing design flaws, emphasizing the need for specific security measures, and implementing safeguards against common weaknesses, we aim to fortify the overall security posture of the application.

---
### 2.3 Project Board & Repository Link
- [Project Board](https://github.com/users/sanne88/projects/1)
-  [Project Repository](https://github.com/sanne88/cyber8420projectopenmrs)

### 2.4 Planning & Reflection
Team Bug Busters consists of  Gopinath, Sahithi, Surya, and Vidya.

First, we implemented an automatic code-scanning code review process. When we first used CodeQL, it supplied several test files along with a list of potential threats.As recommended by the professor, we have also looked into SonarCloud code scanning.
In addition, because the code base is large, our team decided that automated code scanning should be performed before manual code reviews in order to scope our CWE list. With the size of the codebase, this method was more realistic than human code review. 
Based on the use cases, we divided the work between manual and automated code reviews after our initial discussion of the code review strategy. Each team member was assigned a specific task to do and contributed to the assignment.
Surya initiated the automated code review tool selection and setup GitHub CodeQL and SonarCloud for OpenMRS-core forked repository. Surya and Sahithi worked on the data-sensitive use case's manual code review in addition to the automatic tool scanning with Code QL, Sonar Cloud.
Vidya and Gopinath worked on Manual Code review for Authentication and API server functionalities.
