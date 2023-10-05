### 1.1 Assurance Case #1:

**Diagram**:

![Assurance_Case_1](/AssuranceCases_Diagrams/Claim1.PNG)

**Assessment**:

This top-level claim starts by OpenMRS minimizing the theft of session cookies which is solved by OWASP CSRFGuard minimizes the theft of session cookies. OWASP suggests to have CSRF protection in applications and also suggests a way to do it along with a version vulnerablitiy of CSRFGuard as justification. An Open Source contributor to OpenMRS delved into the code to find CSRFGuard version 4.1.1 is being used and the httpOnly attribute is set to true which provides evidence for the top-level claim and C2. An Open Source contributor also found OWASP states that any XSS can be used to defeat all CSRF mitigations which is justification for a rebuttal to make Sub Claim C3. C3 is an unfinished and open claim left to be investigated. 
