### Part-1: Code Review
### 1.1 Code Review Strategy
The OpenMRS system has different parts, and we're specifically looking at the OpenMRS Core, which is a medium-sized code component. To make sure our code is secure, we followed a strategy for reviewing it. This strategy involves looking at different situations and potential dangers.

Our code review strategy includes both automated and manual methods. For automation, we used tools like GitHub CodeQL and Sonar Cloud to find possible issues in the code. After that, for each threat we identified, we manually went through the code. We started with each misuse case, digging into the specific file responsible for that part of the code.

This approach helped us narrow down our focus to just the OpenMRS Core, making sure it's as secure as possible.

### 1.2 Manual Code Review
		
### CWE-476: NULL Pointer Dereference
FileName: CachePropertiesUTil.java 
Line no 74,69, 83 Null pointerexception

If there are missing or null values for certain cache configurations in cacheProperties or any unexpected cache configurations, attempting to access these properties without proper null checks could result in a NullPointerException and can lead to applicatio crash.

Mitigation: Implement appropriate null checks.

### 1.3 Automate Code Review


### CWE-470: Use of Externally-Controlled Input to Select Classes or Code ('Unsafe Reflection')
Issue:
FileName: DatabaseUtil.java
Line No: 57 , 58
The code in this file is prone to a this vulnerability. This type of vulnerability occurs when an application allows an attacker to specify the classes to be loaded dynamically. In code, the class to be loaded (connectionDriver) is obtained from an external source which is passed as an method parameter.If the value of connectionDriver is controlled by an external, untrusted source (such as user input or data from an untrusted network), it can lead to security issues. An attacker might manipulate this input to load a malicious class, which could potentially lead to various security risks, including code execution vulnerabilities. The Lien no 58 also poses a potential threat for log injection attacks.
Mitigation: To mitigate this risk, input validation and sanitization needs to be performed on the user or any externally provided input.Consider implementing security policies to restrict the classes that can be loaded dynamically based on certain criteria.

		
### CWE-321: Use of Hard-coded Cryptographic Key 
FileName:Security.Java 196, OpenMRSConstants.java 556,560

The default encryption key and initialization vector are hard-coded in the code (ENCRYPTION_VECTOR_DEFAULT and ENCRYPTION_KEY_DEFAULT). Hard-coded keys can pose a security risk, especially if they are not changed regularly.

When reviewd the code ,the system demonstrates the dynamic generation of this keys in Security.Java file in the method generateNewInitVector() 294.  Using SecureRandom instead of Random is designed to provide a higher level of security by using a cryptographically secure pseudo-random number generator (CSPRNG).

		
### CWE-611: Improper Restriction of XML External Entity Reference
FileName: UpdateFileParser.java , 77

Parsing untrusted XML files with a weakly configured XML parser may lead to an XML External Entity (XXE) attack. This type of attack uses external entity references to access arbitrary files on a system, carry out denial of service, or server side request forgery. Even when the result of parsing is not returned to the user, out-of-band data retrieval techniques may allow attackers to steal sensitive data. Denial of services can also be carried out in this situation.

Mitigation:  Using an entity resolver that always returns an empty string can prevent the resolution of external entities, which is a good practice to mitigate XML External Entity (XXE) attacks.

### Part-2: Key Findings and Contributions
### 2.1 Key Findings
### 2.2 Contributions 
### Project Board & Repository Link
- [Project Board](https://github.com/users/sanne88/projects/1)
-  [Project Repository](https://github.com/sanne88/cyber8420projectopenmrs)
### Planning & Reflection

Team Bug Busters consists of  Gopinath, Sahithi, Surya, and Vidya.

First, we implemented an automatic code-scanning code review process. When we first used CodeQL, it supplied several test files along with a list of potential threats.As recommended by the professor, we have also looked into SonarQube code scanning.
In addition, because the code base is large, our team decided that automated code scanning should be performed before manual code reviews in order to scope our CWE list. With the size of the codebase, this method was more realistic than human code review. 
Based on the use cases, we divided the work between manual and automated code reviews after our initial discussion of the code review strategy. Each team member was assigned a specific task to do and contributed to the assignment.
