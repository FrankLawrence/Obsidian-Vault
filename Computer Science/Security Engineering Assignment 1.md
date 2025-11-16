---
Tags: 
Created: 2025-09-18 10:48:49
---
(Links:: [[Security Engineering]])
# Development Processes
- Waterfall model corresponds with red line
	- Assumption: Requirements *rarely* change
- Actual development corresponds with blue line
	- Growing in *quantity* but also complexity 
- Agile development adds requirements in each iteration, which are based on the iteration prior

> [!question]- Which diagram is assumed by the classical waterfall model?
> Phase model

> [!question]- Which diagram is more likely to correspond to the actual development?
> V-Model

# Security Environments

You are an employee at a bank and work on an internal application for client advisors to view and edit client data. Through code scanning, vulnerabilities on the application’s input fields (to be filled by the client advisor) have been discovered. If exploited, they may lead to leaking confidential client data or violate the integrity of that data.

During a meeting, it should be discussed whether fixing these vulnerabilities is necessary for an internal application. The application is used only by client advisors and is not publicly accessible.

> [!question]- What are the assumptions on the environment that must be made if the vulnerabilities are not fixed? May there be reasons why these assumptions might not hold?
> - No unauthorised group gains access to the internal application (those inside and outside the organization).
> - Client advisor does not have malicious intent

> [!question]- You want to decide whether to fix the vulnerabilities. What are some points that should be considered?
> - What is the expected cost of execution
> - What is the expected cost when exploited
> - Are the assumptions realistic


# User Authentication Risk
Suppose you are designing a web application that requires users to have accounts. You consider two options:

1. Users sign up with username and password. User credentials are stored in a database.
2. Users use single sign-on (based on the OpenID Connect protocol), such as Sign in with Google.

> [!question] Compare these two options with respect to incurred security risk. What are important security considerations for each option? Which option would you prefer, and why?
> - **Option 1**: Build your own authentication
> 	- Secure storage of credentials
> 	- Implementation of authentication and access control
> - **Option 2**: SSO
> 	- Giving control over data and access to 3rd party
> 	- If google goes down, all the services are unaccessible
> 	- Data can be logged
> 	- They can impersonate any user

# Privacy Principles in Software Development

Privacy concerns in software applications have gained significant attention in recent times, particularly in the light of the recent enactment of the data protection laws, like the Gen- eral Data Protection Regulation (or GDPR). These regulations requires companies to be transparent about their methods of collecting, storing and handling personal data. Since the GDPR came into effect on the 25th of May, 2018; privacy engineering are now playing an important role in software development process.

Not all aspects of GDPR compliance can be integrated into the development process. In this section, we consider several enforcable principles of the GDPR.

(a) Consider the Article 5, Section 1(c) of the GDPR, which addresses the “Principles relating to processing of personal data” that related to the minimization of (personal) data. Personal data shall be: adequate, relevant and limited to what is necessary in relation to the purposes for which they are processed (‘data minimisation’)”. 
> [!question] How can this principle be applied to protect user privacy in software applications?

(b) Consider the Article 13, Section 1(c); Article 7, Section 1 and 2; and Recital 32 of the GDPR on “Consents”. Let’s discuss the role of consent in user data collection and processing. 
> [!question] What are the best practices for obtaining and managing user consent in software applications?

As you will see in this course, implementing security and privacy requirements is not a trivial task. Moreover, these requirements evolve over time, either by the software owner or because the regulation has changed. In the latter part of the course, we will discuss possible techniques to tackle this problem.

---
References: