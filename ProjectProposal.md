# Project Proposal
# Systems Engineering View
# Security Needs, Threats, and Features
# Motivation
# OSS project description
# License Summary
The OpenMRS core application is licensed under the Mozilla Public License (MPL) version 2, along with a healthcare disclaimer.

# Contributing Guidelines
GuideLines
 1. Join the OpenMRS community. Get an OpenMRS ID
 2. Setup Git and Fork the repository and pull chanegs from "main" branch
 3. Name of the repository should include "OpenMRS" like "openmrs-category/module-name"
 4. Select or Create issue on the JIRA board https://issues.openmrs.org/ and change the status to "Claim issue"
 5. If its a new issue , then wait for your issue to be reviewed by a core OpenMRS developer.
 6. Begin work on the issue after it has been assessed and discussed, and a core developer changes its status to "Ready for Work".
If you are selecting an existing ticket to work on, make sure that:
	The issue is marked as "Ready for Work".
	The issue is not "In Progress" and claimed by someone else.
	The issue is not "blocked" waiting for the completion of another issue.

Coding Guidelines

 1. Use both the @Deprecated annotation and the @deprecated javadoc comment. The @deprecated javadoc annotation should point to the new method that is replacing the current one. DAO methods do not have to go through a deprecation cycle. They can be changed/deleted outright.
 2. To enforce security, avoiding XSS scripting by using StringEscapeUtils.escapeJavaScript() and StringEscapeUtils.escapeHtml() to escape any user-generated data in pages.
 3. Use Eclipse auto-formatting features for managing the style of your code.
 4. Provide enough comments for the code changed, add Javadoc comments for any new code introduced.
 5. Unit Test methods to cover all the scenarios.
 6. Run YourKit profiler for any performance issues.
 7. Add a comment to the ticket with a link to your pull request. This will automatically schedule the ticket to be reviewed by a core developer.

 Environment Setup

1. They recommed to use OpenMRS SDK which has all the required setup environment.Apart from this Java > 1.7 or higher needs to be installed
2. OpenMRS Standalone Application -This is a easy to run and  is the recommended option to update an existing module.Installation of "Maven" is still required as this is not bundled in the standalone application.
3. OpenMRS organised the privileges of its contributors in developer stages as below
  1. "Noob"
  2. "Begineer"
  3. "Coder"
  4. "Skilled"
  5. "Expert"
  6. "Guru"

The developers can go from being a newcomer to the community (/dev/null) to a development expert (/dev/5) as their technical proficiency increases. The goal of the developer phases is to assist people understand where they are in their journey, inspire them to become more proficient in OpenMRS development.

Questions
Developers with any questions can post the questions to the 
https://talk.openmrs.org/ and also can email to the community community@openmrs.org and they will help answer any questions.

# Security History
# Project Planning and Reflection
# Overall team planning and Individual Contribution


