# Software Requirements Specification (SRS)

## Metadata & Control

| Attribute | Details |
| :--- | :--- |
| **Project Name** | Rotaract Club of UNZA Website |
| **Document ID** | SRS-001 |
| **Version** | 1.1 |
| **Status** | Draft baseline v1.1 --- recommendations incorporated; pending stakeholder validation |
| **Last Updated** | 18 October 2026 |
| **Target Launch** | 30 November 2026 |
| **Tech Preference** | HTML, CSS, JavaScript, PHP, MySQL, PDO |

---

## 1. Introduction

### 1.1 Purpose
This document defines the scope, functional requirements, quality requirements, constraints, and acceptance conditions for the first release of the Rotaract Club of UNZA website. It is intended to guide implementation, testing, stakeholder review, and release acceptance.

### 1.2 Product Vision
Provide a professional, responsive, maintainable online presence for the Rotaract Club of UNZA. The website will present club information, events, news, service projects, and impact reports, and will allow authorized users to maintain content through an administrative dashboard without editing source code.

The application code should be reusable for separate club websites in the future. The first release will serve one club and will not implement multi-tenant hosting.

### 1.3 Intended Users
* Public visitors and community partners.
* Prospective Rotaractors.
* Club members using public pages.
* Authorized board members and Public Image Committee members.
* The project developer and future maintainers.

> **Note:** Member registration and private member accounts are outside the first-release scope.

### 1.4 Terminology

| Term | Definition |
| :--- | :--- |
| **Administrator** | A person with an authorized account to access protected management functions. |
| **Content Manager** | An authorized user permitted to manage specified public content. |
| **Published Content** | Content approved for public display through its publication status. |
| **SRS** | Software Requirements Specification. |
| **MVP** | Minimum Viable Product. |
| **PDO** | PHP Data Objects, used for secure database access. |
| **Traceability** | Linking stakeholder needs to requirements, implementation components, and tests. |

---

## 2. Project Scope

### 2.1 Release Scope Overview

| In Scope (First Release) | Out of Scope (First Release) |
| :--- | :--- |
| Homepage & About Club page | Member registration & private accounts |
| Events & News listing + detail pages | Multi-tenant hosting / isolated databases |
| Service projects & Impact reports | Online payments, ticket sales, or donations |
| Public contact form & safe processing | Real-time chat or messaging |
| Secure Admin login, logout & dashboard | Native mobile applications |
| Non-code Content Management (CRUD) | Automated content-approval workflow |
| Server-side validation & basic security | Full email marketing system |
| Responsive design & accessibility checks | Complex non-approved third-party integrations |
| Deployment, docs & DB backup procedure | |

### 2.2 Future Reuse Strategy
The codebase should separate club-specific configuration (such as club name, logo, colours, contact details, and social links) from core application logic where practical. Reuse for another club may require configuration and a separate deployment. A shared multi-club platform is not required.

---

## 3. Stakeholders and Access Model

### 3.1 Proposed Access Roles & Permissions

| Role | Target Holder(s) | Recommended Permissions |
| :--- | :--- | :--- |
| **Site Administrator** | President + Secretary / Delegate *(Keep at least 2 active)* | Full administrative access: account/role management, club-wide settings, all content, contact enquiries review, and operational configurations. |
| **Content Manager** | Authorized Board & Public Image Committee members | Create, edit, publish, unpublish, and archive events, news, projects, and impact reports. Cannot manage admin accounts or server configuration. |
| **Enquiry Manager** | Secretary or designated contact person | View, process, mark as handled, and delete public contact enquiries based on retention policy. |
| **Technical Maintainer** | Developer or technical support delegate | Code deployment, technical maintenance, and server environment setup only. Production access should be time-limited/minimized. |

> **Security Rule:** Server-side permission checks are mandatory. Shared accounts and public administrative registration are strictly prohibited.

---

## 4. Functional Requirements

*Priority Scale: **Must** = Essential for initial release; **Should** = High value if schedule allows; **Could** = Optional/Deferred.*

### 4.1 Public Website Features

| Requirement ID | Module | Priority | Description & Acceptance Criteria |
| :--- | :--- | :--- | :--- |
| **FR-001** | Homepage | **Must** | **Desc:** Displays club identity, introduction, navigation, and highlights.<br>**Acceptance:** Accessible without login; presents verified logo and name; main links function; missing optional content does not cause errors. |
| **FR-002** | About Page | **Must** | **Desc:** Displays approved purpose, activities, board listing, and social links.<br>**Acceptance:** Link accessible via navigation; text/images render cleanly across display sizes. |
| **FR-003** | Public Events | **Must** | **Desc:** Displays published upcoming and past events.<br>**Acceptance:** Shows title, date/time, location, description; hides unpublished events; orders upcoming before past; presents clean empty states. |
| **FR-005** | Public News | **Must** | **Desc:** Displays news articles and announcements.<br>**Acceptance:** Articles listing + detail views; shows published dates; hides unpublished content. |
| **FR-007** | Service Projects | **Must** | **Desc:** Displays service project information and impact reports.<br>**Acceptance:** Displays titles, dates, locations, images, and impact indicators where supplied; hides unpublished items. |
| **FR-009** | Contact Form | **Must** | **Desc:** Public form for visitor inquiries with validation.<br>**Acceptance:** Validates required inputs server-side; safe handling against spam/XSS; clear confirmation message upon success; helpful error messages on failure. |

### 4.2 Administrative & Security Requirements

| Requirement ID | Module | Priority | Description & Acceptance Criteria |
| :--- | :--- | :--- | :--- |
| **FR-004** | Event Management | **Must** | Manage events via dashboard. Server-side validation required. Saved published events appear publicly. Hides unpublished items. |
| **FR-006** | News Management | **Must** | Manage news articles via dashboard. Direct publish/unpublish capability without extra approval workflow. |
| **FR-008** | Project Management | **Must** | Manage service projects and impact metrics without source code changes. |
| **FR-010** | Enquiry Management | **Must** | Store submissions securely in database; display only to authorized managers in protected dashboard. |
| **FR-011** | Authentication | **Must** | Password hashing via PHP `password_hash()`; session invalidation on logout; secure session handling. |
| **FR-012** | Authorization | **Must** | Server-side role checks on every protected endpoint/URL. Direct request bypass must be prevented. |
| **FR-013** | Admin Dashboard | **Must** | Protected control panel rendering actions permitted for the logged-in role; provides feedback alerts. |
| **FR-014** | Code-free Management | **Must** | All standard content updates occur via form inputs without editing PHP/HTML/SQL source files. |
| **FR-015** | Club Info Config | **Must** | Ability to update core contact details, social links, and descriptions via dashboard or config file. |
| **FR-016** | Account Management | **Should** | Creation, disabling, and password resets for admin users by Site Administrator. Manual DB fallback allowed if timeline is tight. |
| **FR-017** | Configurable Identity | **Should** | Decouple branding parameters (logo, colors, name) into dedicated config files or database settings. |
| **FR-018** | Public Scope Guard | **Must** | Strictly prohibit public user account creation or login prompts on public views. |

---

### 4.3 Data Model Specification

| Entity | Required Fields | Optional / System-Managed Fields |
| :--- | :--- | :--- |
| **Events** | Title, Start Date/Time, Location, Short Description, Publication Status | End Date/Time, Cover Image, Registration Link, Organizer Info, Attendance Notes. |
| **News Articles** | Title, Body/Summary, Publication Status | *System:* Timestamps. *Optional:* Cover Image, Category, Author Byline, Image Alt Text. |
| **Service Projects** | Project Title, Description, Date/Period, Publication Status | Location, Partners, Photos + Alt Text, Goals, Outcomes, Beneficiary Count, Impact Summary. |
| **Club Settings** | Club Name, Primary Contact Email | Logo Path, Social Links, Brand Colors, Leadership Board Listing. |
| **Enquiries** | Sender Name, Contact Details, Message | *System:* Submission Timestamp. *Status:* New, In Progress, Handled. |

---

## 5. Non-Functional Requirements (NFRs)

| ID | Domain | Standard | Verification Method |
| :--- | :--- | :--- | :--- |
| **NFR-001** | Responsive Design | Fully responsive across screen sizes down to 360px viewport width without horizontal overflow. | Multi-device browser testing (360px, 768px, 1280px). |
| **NFR-002** | Usability | Intuitive navigation and readable typography; clear validation indicators. | Representative user review task completion. |
| **NFR-003** | Database Safety | 100% prepared statements via PHP PDO for dynamic input. Zero SQL string concatenation. | Code review / static analysis. |
| **NFR-004** | Passwords | Strong password hashing using `password_hash()` and verified via `password_verify()`. | Code inspection. |
| **NFR-005** | Input Validation | Mandatory server-side input validation regardless of client-side JavaScript. | Automated & manual boundary testing. |
| **NFR-006** | CSRF | CSRF token validation on all state-changing HTTP requests (POST/PUT/DELETE). | Security review. |
| **NFR-007** | XSS Protection | Context-aware output escaping across template files; sanitization of rich text inputs. | Security review & payload insertion tests. |
| **NFR-008** | Session Security | Session ID regeneration post-login, HTTPS cookies, and automatic inactivity expiry. | Security inspection. |
| **NFR-009** | Secrets | API keys and credentials stored outside web root (e.g., `.env`) and excluded from Git. | Repository audit. |
| **NFR-010** | Architecture | Modular separation of concerns (Views, Logic, Database Access) avoiding monolithic single-file design. | Code architecture review. |
| **NFR-011** | Compatibility | Full compatibility across modern release versions of Chrome, Safari, Firefox, and Edge. | Browser testing matrix. |
| **NFR-012** | Accessibility | Semantic HTML structure, keyboard navigability, clear contrast, and alt text (WCAG 2.2 AA target). | Accessibility audit tools (e.g., Wave/Lighthouse). |
| **NFR-013** | Performance | Target LCP $\le$ 2.5 seconds on representative connections. | Lighthouse Lab Audits. |
| **NFR-014** | Backup & Recovery | Documented, repeatable backup procedure with verified restoration capability. | Trial restoration run in test environment. |
| **NFR-015** | Error Handling | Non-revealing user-facing errors; detailed internal errors saved to secure server log files. | Triggered exception testing. |

---

## 6. Hosting & Budget Plan

### 6.1 Infrastructure Specifications

| Component | Target Specification |
| :--- | :--- |
| **Hosting Model** | Managed Shared PHP/MySQL Hosting |
| **Runtime Environment** | PHP 8.x + PDO MySQL Extension |
| **Database Server** | MySQL / MariaDB |
| **Security Controls** | Free Automated SSL (Let's Encrypt), SFTP, Protected root directories |
| **Tooling** | Scheduled/manual database export tools, custom domain management |

### 6.2 Financial Estimate

| Item | Estimated Annual Cost | Notes |
| :--- | :--- | :--- |
| **Domain Registration** | $10 – $20 | .org or local extension |
| **Shared Hosting** | $40 – $80 | Introductory rate baseline |
| **SSL Certificate** | $0 | Included via free issuer |
| **Contingency Buffer** | ~20% | Account for renewal jumps or transaction fees |
| **Total Estimated Allocation** | **US$50 – $100 / Year 1** | *Excludes developer labor & paid plugins* |

---

## 7. Open Issues & Decisions Tracker

| ID | Issue / Decision Description | Responsible Owner | Target Status |
| :--- | :--- | :--- | :--- |
| **OI-01** | Confirm named Site Administrators, Content Managers, and Enquiry Managers | Board / President | Proposed |
| **OI-02** | Decide on Admin Account UI vs. Manual DB provisioning for initial launch | Developer / Board | Proposed |
| **OI-03** | Confirm if automated SMTP email notifications are required for form submissions | Board / Contact Person | Open |
| **OI-04** | Finalize soft-delete/archive retention policy vs. permanent deletion | Board / Public Image | Proposed |
| **OI-05** | Confirm required and optional metadata fields across events, news, and projects | Public Image / Secretary | Proposed |
| **OI-06** | Finalize hosting and domain provider purchase after reviewing two quotes | Treasurer / Developer | Open |
| **OI-07** | Secure rights/consent for published photos, stories, and impact statistics | Public Image Committee | Open |
| **OI-08** | Appoint post-launch system maintainer and operational owner | Board | Open |
| **OI-09** | Determine whether branding config needs full UI or simple config file for v1 | Developer / President | Open |
| **OI-10** | Establish inquiry retention window (Default proposed: 12 months post-closure) | Board | Proposed |

---

## 8. Stakeholder Validation & Sign-off

*This document represents a baseline proposal pending formal approval.*

| Role | Assigned Name | Status | Date |
| :--- | :--- | :--- | :--- |
| **Club Representative** | *To be confirmed* | Pending | — |
| **Public Image Chair** | *To be confirmed* | Pending | — |
| **Lead Developer** | *To be completed* | Pending | — |
