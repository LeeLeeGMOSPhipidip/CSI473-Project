# Technical Debt and Risk Register
## RREMMOGO

### 1. Introduction

This document identifies potential technical debt and project risks associated with developing and maintaining RREMMOGO.

Technical debt refers to shortcuts or design decisions that may make development easier initially but could create additional maintenance work in the future.

Project risks are possible events that could negatively affect system functionality, security, performance, usability, or project completion.

### 2. Technical Debt Register

| ID | Technical debt | Possible impact | Recommended solution | Priority |
|---|---|---|---|---|
| TD01 | Poorly organised source code. | Difficult to maintain and modify the application. | Use modular code and separate application responsibilities. | High |
| TD02 | Repeated code across modules. | Increases maintenance effort and inconsistency. | Introduce reusable functions and components. | Medium |
| TD03 | Insufficient input validation. | Invalid or incomplete information may enter the system. | Implement client-side and server-side validation. | High |
| TD04 | Limited automated testing. | Defects may remain undetected. | Introduce automated unit and integration tests. | High |
| TD05 | Incomplete documentation. | Future developers may struggle to understand the system. | Maintain technical documentation and update the README. | Medium |
| TD06 | Inefficient database queries. | Search and transaction operations may become slow. | Optimise queries and introduce appropriate indexes. | Medium |
| TD07 | Limited error handling. | Users may receive unclear error messages or experience failures. | Implement consistent error handling and logging. | High |
| TD08 | Accessibility not fully verified. | Some users may experience difficulty accessing the system. | Conduct accessibility reviews and testing. | Medium |

### 3. Project Risk Register

| ID | Risk | Likelihood | Impact | Mitigation strategy |
|---|---|---|---|---|
| R01 | Unauthorised users gain access to accounts. | Medium | High | Enforce authentication, secure passwords, and role-based permissions. |
| R02 | Double booking of rental items. | Medium | High | Validate item availability before confirming a rental and use database transactions. |
| R03 | Incorrect payment confirmations. | Medium | High | Require proof-of-payment submission and seller confirmation. |
| R04 | Users fail to return rented items on time. | High | High | Record due dates and provide overdue reminders. |
| R05 | Loss of uploaded proof of payment or item images. | Medium | High | Use secure file storage and regular backups. |
| R06 | Database failure or data loss. | Medium | High | Implement backup and recovery procedures. |
| R07 | Users provide false item information. | Medium | Medium | Allow reporting, administrative review, and clear listing requirements. |
| R08 | Communication disputes between renters and sellers. | Medium | Medium | Maintain transaction records and relevant communication history. |
| R09 | System performance decreases as users increase. | Medium | Medium | Monitor performance and optimise database queries. |
| R10 | Project implementation takes longer than planned. | Medium | High | Prioritise essential features and follow a development schedule. |
| R11 | External email or notification service becomes unavailable. | Medium | Medium | Handle service failures and provide alternative in-app status messages. |
| R12 | Users upload malicious or inappropriate files. | Medium | High | Validate file types and sizes, restrict access, and scan uploaded files where possible. |

### 4. Risk Management Strategy

The project will manage risks through the following activities:

1. Identify possible risks during development.
2. Assess their likelihood and potential impact.
3. Prioritise high-impact risks.
4. Implement preventive measures.
5. Monitor risks throughout the project.
6. Update the risk register when new risks are identified.

### 5. Highest-Priority Risks

The following risks require particular attention:

**Security risks:** User accounts and personal information must be protected through authentication and access control.

**Double booking:** The system must prevent two users from successfully renting the same unavailable quantity during overlapping rental periods.

**Payment disputes:** Payment confirmation must be recorded clearly. The platform should distinguish between uploading proof of payment and the seller confirming receipt.

**Data loss:** Rental records, user information, and uploaded documents should be protected through appropriate backup and recovery procedures.

**Late returns:** The system should record due dates and notify users when rental items become overdue.

### 6. Conclusion

Managing technical debt and project risks helps ensure that the Student Rental Marketplace remains secure, reliable, maintainable, and usable. The register should be reviewed regularly as the project develops.
