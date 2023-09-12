# Project Proposal
## Team Bug Busters
OSS Repository link: [openmrs-core](https://github.com/openmrs/openmrs-core)
## Hypothetical Operation Environment

This hypothetical operational environment for OpenMRS depicts a typical healthcare environment where the OpenMRS platform is used to handle electronic health records (EHR). It highlights the value of system integration, data security, and compliance while giving medical practitioners a user-friendly and effective tool for patient care. The application is deployed using containerization and will be deployed on a Tomcat Apache Server. OpenMRS adopted the CI tool Bamboo following the shift into the agile development process. The continuous integration tools can be accessed at http://ci.openmrs.org/.

## Systems Engineering View

The following diagram offers a high-level overview of the openMRS core module. Since this package can be easily deployed as an application with minimal configuration changes, our perspective revolves around considering the application in terms of software and data as the core elements of our system of interest. We have identified enabling systems as those used in the development, deployment, and maintenance of the application. Other systems encompass third-party providers, such as insurance provider information and the payment services module.


![System Security Engineering View](https://github.com/sanne88/cyber8420projectopenmrs/assets/99826496/774bfce7-1aee-4bff-ac6d-257d01c57071)

## Perceived Security Threats

1. **Data Breaches**:
   - Unauthorized access to patient records can lead to privacy violations.
   - Data leakage due to inadequate data protection can expose sensitive patient information.

2. **Injection Attacks**:
   - SQL Injection: Attackers can manipulate the database by injecting malicious SQL queries.
   - Cross-Site Scripting (XSS): Vulnerabilities in web pages can allow attackers to inject malicious scripts into users' browsers.

3. **Authentication and Authorization**:
   - Weak passwords can be easily guessed or cracked.
   - Insufficient access controls can lead to unauthorized access to patient data.

4. **Session Management**:
   - Session fixation: Attackers may set a user's session ID, allowing them to hijack the session.
   - Session timeout issues can expose active sessions to unauthorized access.

5. **Cross-Site Request Forgery (CSRF)**:
   - CSRF attacks trick authenticated users into performing actions without their consent.

6. **Misconfigurations**:
   - Insecure defaults and misconfigurations can create vulnerabilities that attackers may exploit.

7. **Software Vulnerabilities**:
   - Vulnerabilities in dependencies and software components can be entry points for attacks.

8. **Insider Threats**:
   - Malicious or negligent actions by authorized users can jeopardize patient data.

9. **Denial of Service (DoS) Attacks**:
   - Attackers may overload the system with excessive traffic, causing it to become unavailable.

10. **Phishing and Social Engineering**:
    - Phishing attempts and social engineering tactics may lead to unauthorized access or data disclosure.

## Security Features

1. **Data Protection**:
   -  Secure patient records and sensitive data with strong encryption.
   -  Implement data encryption in transit (HTTPS) and at rest (data storage).

2. **Authentication and Authorization**:
   -  Enforce strong user authentication mechanisms (e.g., multi-factor authentication) and encrypt user credentials.
   -  Implement robust access control to prevent unauthorized access.
   -  Enforce password policies to ensure strong and unique passwords.

3. **Injection Prevention**:
   -  Implement input validation and output encoding to prevent SQL Injection.
   -  Mitigate Cross-Site Scripting (XSS) attacks through input sanitization.

4. **Session Management**:
   -  Prevent session fixation by generating new session IDs upon login.
   -  Configure proper session timeout settings for security.

5. **Cross-Site Request Forgery (CSRF) Protection**:
   - Implement CSRF protection mechanisms like anti-CSRF tokens.

6. **Configuration Security**:
   -  Ensure secure default configurations and provide secure configuration guidelines.
   -  Mitigate misconfigurations by conducting regular security reviews.

7. **Software Vulnerability Management**:
   - Regularly update and patch all software components.
   - Continuously monitor for vulnerabilities in dependencies and apply patches promptly.

8. **Insider Threat Mitigation**:
   - Implement access controls and audit trails to monitor user actions.
   - Provide training to employees to raise awareness about security threats.

9. **Denial of Service (DoS) Resilience**:
   - Implement measures such as rate limiting and traffic filtering to withstand DoS attacks.

10. **Phishing and Social Engineering Awareness**:
    - Conduct regular security awareness training for personnel to recognize and prevent social engineering attacks.

Ensure that these security features are addressed to maintain the security and integrity of the OpenMRS platform and safeguard patient data.

## Motivation
## OSS project description
OpenMRS is a collaborative open-source project to develop software to support the delivery of health care in developing countries. The system is designed to be usable in very resource poor environments and can be modified with the addition of new data items, forms and reports without programming. As a platform, it is intended that many organizations can adopt and modify avoiding the need to develop a system from scratch. Currently, it has around 40 packages and 500 contributors, is mostly written in Java(65%) with supporting languages of Javascript(18%), XML, HTML, CSS, SQL, Ruby, and NSIS. Openhub says the last commit was 8 months ago but at least 40 repositories show updates from the last few hours to 3 months ago. 

The core package (system of interest) is written in 
- Java(98.3%)
- JavaScript(0.5%)
- XSLT(0.4%)
- CSS (0.3%)
- Shell(0.3%)
- HTML(0.1%)
- Dockerfile(0.1%).

It has about 388 active contributors and the latest commit made about a week ago.
Documentation: [OpenMRS documentation](https://openmrs.org/documentation/) and [OpenMRS Devmanual](https://devmanual.openmrs.org/en/Technology/architecture.html)

## License Summary

The OpenMRS core application is licensed under the Mozilla Public License (MPL) version 2, along with a healthcare disclaimer http://openmrs.org/license.The Health-Related Additional Disclaimer of Warranty and Limitation of Liability is a tool afforded by MPL 2.0 that allows to address the specific medico-legal needs without having to modify the license itself.

The website supports using opensource IDE tools and offers licenses if there is a strong enough business justification.

## Contributing Guidelines
**GuideLines**:
 1. Join the OpenMRS community. Get an OpenMRS ID by registering at  http://om.rs/id
 2. Setup Git and Fork the repository and pull chanegs from "main" branch.Make sure the repository is public.
 3. Name of the repository should include "OpenMRS" like "openmrs-category/module-name"
 4. Select or Create issue on the JIRA board https://issues.openmrs.org/ and change the status to "Claim issue"
 5. If its a new issue , then wait for your issue to be reviewed by a core OpenMRS developer.
 6. Begin work on the issue after it has been assessed and discussed, and a core developer changes its status to "Ready for Work".
If you are selecting an existing ticket to work on, make sure that:
	The issue is marked as "Ready for Work".
	The issue is not "In Progress" and claimed by someone else.
	The issue is not "blocked" waiting for the completion of another issue.

**Coding Guidelines**:

 1. Use both the @Deprecated annotation and the @deprecated javadoc comment. The @deprecated javadoc annotation should point to the new method that is replacing the current one. DAO methods do not have to go through a deprecation cycle. They can be changed/deleted outright.
 2. To enforce security, avoiding XSS scripting by using StringEscapeUtils.escapeJavaScript() and StringEscapeUtils.escapeHtml() to escape any user-generated data in pages.
 3. Use Eclipse auto-formatting features for managing the style of your code.
 4. Provide enough comments for the code changed, add Javadoc comments for any new code introduced.
 5. Unit Test methods to cover all the scenarios.
 6. Run YourKit profiler for any performance issues.
 7. Add a comment to the ticket with a link to your pull request. This will automatically schedule the ticket to be reviewed by a core developer.

**Environment Setup**:

1. They recommed to use OpenMRS SDK which has all the required setup environment.Apart from this Java > 1.7 or higher needs to be installed
2. OpenMRS Standalone Application -This is a easy to run and  is the recommended option to update an existing module.Installation of "Maven" is still required as this is not bundled in the standalone application.
3. There is also the option to manually install everything.
4. OpenMRS organised the privileges of its contributors in developer stages as below
   - "Noob"
   - "Begineer"
   - "Coder"
   - "Skilled"
   - "Expert"
   - "Guru"

The developers can go from being a newcomer to the community (/dev/null) to a development expert (/dev/5) as their technical proficiency increases. The goal of the developer phases is to assist people understand where they are in their journey, inspire them to become more proficient in OpenMRS development.

**Get Help**:

Developers with any questions can post the questions to the https://talk.openmrs.org/ and also can email to the community community@openmrs.org and they will help answer any questions.

Slack Channel: https://wiki.openmrs.org/display/IRC

## Security History
OpenMRS had a couple of security vulnerabilities in the past as seen here: [Closed Security vulnerabilities in the past](https://issues.openmrs.org/browse/RA-1992?jql=status%20%3D%20Closed%20AND%20labels%20%3D%20security%20ORDER%20BY%20created%20DESC).
They have categorized security-related tickets as Story, Bug, Task, and New Features.
Some significant changes include:

|Ticket|Type|Description|Created|Resolved|
|------|------|------|------|------|
|[TRUNK-6185](https://issues.openmrs.org/browse/TRUNK-6185)|Story|Loading CSRF properties on initial startup of the OpenMRS application|2023-08-08|2023-08-26|
|[RESTWS-903](https://issues.openmrs.org/browse/RESTWS-903)|Bug|Session endpoint returns the session key id|2022-11-28|2022-11-28|
|[TRUNK-5829](https://issues.openmrs.org/browse/TRUNK-5829)|Bug|Dependency vulnerabilities amongst 10 repositories reported by GitHub|2020-07-03|2020-08-10|
|[TRUNK-4819](https://issues.openmrs.org/browse/TRUNK-4819)|Bug|OpenMRS is vulnerable to attackers unless proxies and XML entities are ignored. Spring EL support to be disabled|2016-01-28|2016-01-28|
|[TRUNK-4817](https://issues.openmrs.org/browse/TRUNK-4817)|Bug|User redirected to Login page after login.|2016-01-27|2016-03-09|
|[TRUNK-3936](https://issues.openmrs.org/browse/TRUNK-3936)|Bug|Openmrs-runtime.properties file contains database credentials|2013-03-21|2013-09-12|
|[TRUNK-3934](https://issues.openmrs.org/browse/TRUNK-3934)|Bug|openmrs-runtime.properties file should be modified by the .war file and this file is in the /user directory which should not be tampered. Ideally tomcat user should have write access to the /user directory|2013-03-21|2014-10-28|
|[TRUNK-3878](https://issues.openmrs.org/browse/TRUNK-3878)|Bug|Issue with Forgot password form|2013-01-17|2013-09-12|
|[TRUNK-3877](https://issues.openmrs.org/browse/TRUNK-3877)|Bug|Secret question deleted when password is changed|2013-01-17|2014-10-28|
|[TRUNK-3781](https://issues.openmrs.org/browse/TRUNK-3781)|New Feature|Daemon thread support for modules|2012-11-07|2012-12-04|
|[SEARCH-15](https://issues.openmrs.org/browse/SEARCH-15)|New Feature|Secure Lucene-generated index files|2011-06-23|2020-11-09|
|[TRUNK-2178](https://issues.openmrs.org/browse/TRUNK-2178)|Bug|Skip columns that contain user entered data|2011-04-04|2011-04-05|
|[TRUNK-1296](https://issues.openmrs.org/browse/TRUNK-1296)|Bug|Restrict forms available on the user dashboard based on user privileges and roles.|2009-10-09|2010-07-01|

A few critical unresolved security concerns need to be fixed are listed below.

|Ticket|Type|Description|Created|
|------|------|------|------|
|[TRUNK-6155](https://issues.openmrs.org/browse/TRUNK-6155)|Task|Upgrade Liquibase|2022-12-15|
|[TRUNK-6154](https://issues.openmrs.org/browse/TRUNK-6154)|Task|Few endpoints at the API are publicly available without any authorization|2022-12-14|
|[TRUNK-6043](https://issues.openmrs.org/browse/TRUNK-6043)|Bug|Blind time-based SQL injection vulnerability|2021-10-29|
|[TRUNK-6041](https://issues.openmrs.org/browse/TRUNK-6041)|Bug|Vulnerability in scripts to generate Liquibase file|2021-10-19|

There is also a highly critical security advisory issued by the OpenMRS team. Details of which can be found here: [Security Advisory](https://github.com/openmrs/openmrs-core/security/advisories/GHSA-8rgr-ww69-jv65). The vulnerability is about the possibility of attackers sending GET requests to the "/images" and "/initfilter/scripts" endpoints. Due to a lack of proper input validation and security checks, these requests can be manipulated and files on the system can be compromised. This vulnerability has been fixed and the OpenMRS community suggests upgrading to the latest patch version of OpenMRS and upgrading the tomcat instance as well.

Any security-related concerns can be emailed to [security@openmrs.org](mailto:security@openmrs.org)






## Project Planning and Reflection
Team Bug Busters consists of Brian, Carl, Gopinath, Sahithi, Surya, and Vidya. The team is led by Sahithi. The team members are skilled in a variety of fields, including AI, software development, and cyber security.

The team members participated in multiple brainstorming sessions and discussed various open-source repositories. In the end, all members of the team unanimously agreed to work on the OpenMRS repository for this course project.

All members of the team contributed to this document.
- Brian worked on the Motivation section
- Carl worked on the OSS Project Description section
- Gopinath worked on Security Needs, Threats, and Features and helped put the document together.
- Sahithi worked on the License Summary and Contributor Agreement and as a team lead organised the discussions amongst the team members.
- Surya worked on the Security History section.
- Vidya worked on the System Engineering View Diagram. All the team members discussed and identified the important components of the diagram.

Team Bug Busters meets internally every Tuesday. Some of the team members are full time employees and hence the team is collaborating in the evenings. The team is connected via teams. Other options like Zoom and Canvas are also being explored.
The Project Board can be found here: [cyber8420projectopenmrs](https://github.com/users/sanne88/projects/1)

