# Software Requirements Specification (SRS)

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website |
| **Document ID** | SRS-001 |
| **Version** | 1.2 |
| **Status** | Draft baseline: pending stakeholder validation |
| **Last updated** | 18 October 2026 |
| **Target launch** | 30 November 2026 |
| **Owner** | Project Developer |
| **Related** | [CHR-001](project-charter.md), [STK-001](stakeholder-register.md), [RTM-001](requirements-traceability-matrix.md), [ADR-LOG-001](architecture-decisions.md), [SAD-001](system-architecture-design.md) |

---

## 1. Introduction

### 1.1 Purpose

This document defines the scope, functional requirements, quality requirements, constraints, and acceptance conditions for the first release of the Rotaract Club of UNZA website. It guides implementation, testing, stakeholder review, and release acceptance.

### 1.2 Product Vision

Provide a professional, responsive, maintainable online presence for the Rotaract Club of UNZA. The website presents club information, events, news, service projects, and impact reports. Authorized users maintain content through an administrative dashboard without editing source code.

The code should be reusable for other club websites in the future. Release 1 serves one club and is not multi-tenant.

### 1.3 Intended Users

* Public visitors and community partners.
* Prospective Rotaractors.
* Club members using public pages (member accounts are out of scope).
* Authorized board members and Public Image Committee members.
* The project developer and future maintainers.

### 1.4 Conventions

* **Priority scale:** **Must** = essential for release 1; **Should** = high value if the schedule allows; **Could** = optional or deferrable.
* **Identifiers:** `FR-nnn` functional requirement, `NFR-nnn` non-functional requirement, `CR-nn` candidate requirement, `OI-nn` open issue. See the [documentation index](README.md).
* **Module names** in requirement tables are the modules defined in [SAD-001 §6](system-architecture-design.md#6-application-modules).

### 1.5 Terminology

| Term | Definition |
| :--- | :--- |
| **Site Administrator** | Application role with full administrative access (see §3) |
| **Content Manager** | Application role that manages public content |
| **Enquiry Manager** | Application role that handles contact enquiries |
| **Technical Maintainer** | Operational role for deployment and server work; not an application role |
| **Administrator** | Generic term for any signed-in user of the protected dashboard |
| **Draft / Published / Archived** | The three content states (see §5) |
| **Published content** | Content in the Published state, visible to the public |
| **Unpublish** | Return a Published item to Draft |
| **PDO** | PHP Data Objects, used for database access with prepared statements |
| **SRS / MVP** | Software Requirements Specification / Minimum Viable Product |
| **Traceability** | Linking needs to requirements, components, and tests (see RTM-001) |

---

## 2. Project Scope

### 2.1 Release 1 Scope

| In scope | Out of scope |
| :--- | :--- |
| Homepage and About page | Member registration and private accounts |
| Events and news (list and detail pages) | Multi-tenant hosting or isolated databases |
| Service projects and impact reports | Online payments, ticket sales, donations |
| Public contact form with safe processing | Real-time chat or messaging |
| Secure administrator login, logout, dashboard | Native mobile applications |
| Content management without code changes | Automated content-approval workflow |
| Server-side validation and baseline security | Full email marketing system |
| Responsive design and accessibility checks | Complex third-party integrations |
| Deployment, documentation, database backup procedure | |

### 2.2 Future Reuse

Club-specific values (name, logo, colours, contact details, social links) are kept separate from core logic where practical. Reuse for another club needs configuration and a separate deployment. A shared multi-club platform is not required ([ADR-003](architecture-decisions.md#adr-003-build-for-one-club-first-with-reusable-core-code)).

### 2.3 Technology

HTML, CSS, JavaScript, PHP 8.x, MySQL/MariaDB, PDO, Git and GitHub ([ADR-001](architecture-decisions.md#adr-001-use-a-phpmysql-server-rendered-application)).

---

## 3. Stakeholders and Access Model

### 3.1 Roles

| Role | Typical holders | Purpose |
| :--- | :--- | :--- |
| **Site Administrator** | President plus one designated successor or delegate (keep at least two active) | Full application access: accounts and roles, club settings, all content, enquiries, application configuration |
| **Content Manager** | Authorized Board and Public Image Committee members | Create, edit, publish, unpublish, and archive events, news, projects, and impact reports |
| **Enquiry Manager** | Secretary or designated contact person | View, process, and close public contact enquiries |
| **Technical Maintainer** | Developer or technical delegate | Code deployment, server setup, and technical maintenance only; access time-limited and minimized |

A person may hold more than one application role. Technical Maintainer is an operational role and carries no application permissions by default.

### 3.2 Permission Matrix

✔ = allowed, ✘ = not allowed.

| Action | Site Administrator | Content Manager | Enquiry Manager | Technical Maintainer |
| :--- | :---: | :---: | :---: | :---: |
| Sign in to the dashboard | ✔ | ✔ | ✔ | ✘ |
| Create and edit events, news, projects | ✔ | ✔ | ✘ | ✘ |
| Publish, unpublish, archive content | ✔ | ✔ | ✘ | ✘ |
| Permanently delete content | ✔ | ✘ | ✘ | ✘ |
| View and process enquiries (status changes) | ✔ | ✘ | ✔ | ✘ |
| Soft-delete enquiries per retention policy | ✔ | ✘ | ✔ | ✘ |
| Permanently purge enquiries | ✔ | ✘ | ✘ | ✘ |
| Edit club settings (FR-015) | ✔ | ✘ | ✘ | ✘ |
| Manage administrator accounts (FR-016) | ✔ | ✘ | ✘ | ✘ |
| View audit logs (if implemented, CR-03) | ✔ | ✘ | ✘ | ✘ |
| Deploy code and configure the server | ✘ | ✘ | ✘ | ✔ |

> Whether Content Managers may edit About-page and club-detail text is to be confirmed under OI-01.

### 3.3 Security Rules

1. Server-side permission checks are mandatory on every protected action.
2. Individual accounts only; shared accounts and public administrator registration are prohibited.
3. At least two Site Administrator accounts stay active to prevent lockout.

---

## 4. Functional Requirements

### 4.1 Public Website

| ID | Module | Priority | Requirement | Acceptance criteria |
| :--- | :--- | :---: | :--- | :--- |
| **FR-001** | Public Website | Must | Homepage presents club identity, introduction, navigation, and highlights | Accessible without login; shows verified logo and name; main links work; missing optional content causes no errors |
| **FR-002** | Public Website | Must | About page presents approved purpose, activities, board listing, and social links | Reachable from navigation; text and images render cleanly at all display sizes |
| **FR-003** | Events | Must | Display published upcoming and past events | Shows title, date and time, location, description; hides Draft and Archived events; upcoming events listed before past; clear empty state |
| **FR-005** | News | Must | Display news articles and announcements | Listing and detail views; publication date shown; hides Draft and Archived items |
| **FR-007** | Projects | Must | Display service projects and impact reports | Shows title, date or period, location, images, and impact indicators where supplied; hides Draft and Archived items |
| **FR-009** | Contact | Must | Public contact form for visitor enquiries | Required fields validated server-side; protected against spam and XSS; clear success confirmation; helpful error messages; enquiry stored (FR-010) |

### 4.2 Content and Enquiry Management

| ID | Module | Priority | Requirement | Acceptance criteria |
| :--- | :--- | :---: | :--- | :--- |
| **FR-004** | Events | Must | Manage events from the dashboard | Authorized users create, edit, publish, unpublish, and archive events; server-side validation of required fields; only Published events appear publicly, including via direct URL; permanent delete is Site Administrator only |
| **FR-006** | News | Must | Manage news articles from the dashboard | Same lifecycle as FR-004; publish and unpublish act directly with no approval step |
| **FR-008** | Projects | Must | Manage service projects and impact metrics | Same lifecycle as FR-004; impact fields editable without code changes |
| **FR-010** | Contact | Must | Store and protect submitted enquiries | Enquiry saved to the database before any notification; visible only to Enquiry Managers and Site Administrators; status New, In Progress, or Handled; retention per OI-10 |

### 4.3 Authentication and Administration

| ID | Module | Priority | Requirement | Acceptance criteria |
| :--- | :--- | :---: | :--- | :--- |
| **FR-011** | Authentication | Must | Administrator login and logout | Individual accounts; passwords stored with `password_hash()`; session ID regenerated at login; logout invalidates the session; secure session handling |
| **FR-012** | Authorization | Must | Server-side role checks | Every protected route and action checks the user's role; direct URL or request bypass is rejected |
| **FR-013** | Admin | Must | Administrative dashboard | Protected; shows only actions permitted for the signed-in role; gives feedback alerts |
| **FR-014** | Admin | Must | Code-free content management | All routine content updates are made through forms, with no edits to PHP, HTML, or SQL files |
| **FR-016** | Users | Should | Administrator account management | Site Administrator can create, disable, and reset passwords for accounts. A documented manual database procedure is an accepted fallback if the schedule is tight (OI-02) |

### 4.4 Configuration and Scope Control

| ID | Module | Priority | Requirement | Acceptance criteria |
| :--- | :--- | :---: | :--- | :--- |
| **FR-015** | Settings | Must | Update core club information | Contact details, social links, and description can be changed through the dashboard or a configuration file (OI-09), and the public site reflects the change |
| **FR-017** | Settings | Should | Configurable club identity | Name, logo, and colours live in configuration or settings, not in core code; another club can be configured without changing core code |
| **FR-018** | Public Website | Must | Public scope guard | No public account registration or login prompts appear on public pages |

---

## 5. Data Model

This section is the **master definition** of content fields and states. Other documents refer to it.

### 5.1 Content States

All content items (events, news, projects) use three states:

| State | Meaning | Public visibility |
| :--- | :--- | :---: |
| **Draft** | Being prepared, or unpublished | Hidden |
| **Published** | Approved for display | Visible |
| **Archived** | Retired but retained (soft delete) | Hidden |

Unpublishing moves an item from Published to Draft. Permanent deletion is a separate Site Administrator action.

### 5.2 Entities and Fields

| Entity | Required fields | Optional or system-managed fields |
| :--- | :--- | :--- |
| **Events** | Title, start date and time, location, short description, state | End date and time, cover image with alt text, extended description, registration link, organizer info, attendance notes |
| **News articles** | Title, body or summary, state | *System:* created, updated, and published timestamps (the public publication date). *Optional:* cover image with alt text, category, author byline |
| **Service projects** | Title, description, date or period, state | Category, location, partners, photos with alt text, goals, outcomes, beneficiary count, impact summary |
| **Club settings** | Club name, primary contact email | Description, logo path, social links, brand colours, leadership board listing |
| **Enquiries** | Sender name, email, subject, message | Phone number. *System:* submission timestamp. *Status:* New, In Progress, Handled |
| **Users and roles** | Name, email, password hash, role(s), account status | Last sign-in time |
| **Media** | File reference, alt text, uploader | Timestamps, linked content item |
| **Audit log** *(if implemented, CR-03)* | Actor, action, target, timestamp | Details |

---

## 6. Non-Functional Requirements

| ID | Domain | Priority | Requirement | Verification | Test case |
| :--- | :--- | :---: | :--- | :--- | :--- |
| **NFR-001** | Responsive design | Must | Fully usable from 360 px viewport width upward without horizontal overflow | Browser testing at 360, 768, 1280 px | TC-UI-001 |
| **NFR-002** | Usability | Must | Intuitive navigation, readable typography, clear validation feedback | Representative user task review | TC-UX-001 |
| **NFR-003** | Database safety | Must | PDO prepared statements for all dynamic input; no SQL string concatenation | Code review, SQL injection tests | TC-SEC-002 |
| **NFR-004** | Passwords | Must | Hash with `password_hash()`; verify with `password_verify()` | Code inspection, database check | TC-SEC-003 |
| **NFR-005** | Input validation | Must | Server-side validation regardless of client-side JavaScript | Boundary and malformed-input tests | TC-VAL-001 |
| **NFR-006** | CSRF | Must | CSRF token on every state-changing request (POST, PUT, DELETE) | Security review, missing-token tests | TC-SEC-004 |
| **NFR-007** | XSS | Must | Context-aware output escaping; sanitize rich-text input | Security review, payload tests | TC-SEC-005 |
| **NFR-008** | Session security | Must | Regenerate session ID after login; secure cookies over HTTPS; expire on inactivity | Security inspection | TC-SEC-006 |
| **NFR-009** | Secrets | Must | Credentials and keys kept outside the web root (for example `.env`) and out of Git | Repository audit | TC-SEC-007 |
| **NFR-010** | Architecture | Must | Separate views, logic, and data access; no monolithic single-file design | Architecture review | TC-ARCH-002 |
| **NFR-011** | Compatibility | Must | Works on current Chrome, Safari, Firefox, and Edge | Browser test matrix | TC-COMP-001 |
| **NFR-012** | Accessibility | Should | Semantic HTML, keyboard navigation, sufficient contrast, alt text (WCAG 2.2 AA target) | Audit tools (WAVE, Lighthouse) | TC-A11Y-001 |
| **NFR-013** | Performance | Should | Target Largest Contentful Paint of 2.5 s or less on representative connections; results measured and documented | Lighthouse lab audits | TC-PERF-001 |
| **NFR-014** | Backup and recovery | Must | Documented, repeatable backup with a verified restoration | Trial restore in a test environment | TC-OPS-001 |
| **NFR-015** | Error handling | Must | Generic user-facing errors; detailed errors written to protected server logs | Triggered-exception tests | TC-SEC-008 |

---

## 7. Hosting and Budget

### 7.1 Infrastructure

| Component | Target specification |
| :--- | :--- |
| Hosting model | Managed shared PHP/MySQL hosting |
| Runtime | PHP 8.x with the PDO MySQL extension |
| Database | MySQL or MariaDB |
| Security | Free automated SSL (Let's Encrypt), SFTP, protected directories outside the web root |
| Tooling | Scheduled or manual database export; custom domain management |

Provider selection and checklist: [ADR-011](architecture-decisions.md#adr-011-hosting-and-domain-provider). Deployment layout: [SAD-001 §10](system-architecture-design.md#10-deployment-architecture).

### 7.2 Year-One Cost Estimate

| Item | Estimated annual cost | Notes |
| :--- | :---: | :--- |
| Domain registration | US$10–20 | .org or local extension |
| Shared hosting | US$40–80 | Introductory rate |
| SSL certificate | US$0 | Free issuer |
| **Subtotal (budget envelope)** | **US$50–100** | Domain plus hosting; excludes developer labour and paid plugins |
| Contingency (about 20%) | US$10–20 | For renewal increases or fees; tracked separately |
| **Worst case with contingency** | **about US$60–120** | |

> The earlier draft showed US$50–100 as the total *including* contingency, which does not add up from the line items. All documents now use US$50–100 as the envelope for domain plus hosting. The Treasurer should confirm whether contingency sits inside or outside it (OI-06).

---

## 8. Open Issues and Decisions

This is the **single register** of open issues. Status values: **Open** (no proposal yet), **Proposed** (recommendation awaiting confirmation), **Accepted** (decided).

| ID | Issue or decision | Owner | Status | Related |
| :--- | :--- | :--- | :---: | :--- |
| **OI-01** | Confirm named Site Administrators, Content Managers, and Enquiry Managers; confirm who may edit club details | Board, President | Proposed | §3, ADR-004, STK-001 |
| **OI-02** | Decide between an account-management UI and manual database provisioning for launch | Developer, Board | Proposed | FR-016, ADR-004 |
| **OI-03** | Confirm whether SMTP email notifications are required for contact submissions | Board, contact person | Open | FR-009, ADR-014 |
| **OI-04** | Finalize soft-delete and archive policy versus permanent deletion | Board, Public Image Committee | Proposed | FR-004, FR-006, FR-008, ADR-012 |
| **OI-05** | Confirm required and optional fields for events, news, and projects | Public Image Committee, Secretary | Proposed | §5 |
| **OI-06** | Select hosting and domain after reviewing two quotes; confirm budget and contingency treatment | Treasurer, with President and Developer | Open | §7, ADR-011 |
| **OI-07** | Secure rights and consent for published photos, stories, and impact statistics | Public Image Committee | Open | FR-007 |
| **OI-08** | Appoint the post-launch system maintainer and operational owner | Board | Open | ST-08, ADR-004 |
| **OI-09** | Decide whether branding and club details need a dashboard UI or a configuration file for release 1 | Developer, President | Open | FR-015, FR-017 |
| **OI-10** | Set enquiry retention (proposed: 12 months after closure) | Board | Proposed | FR-010, ADR-012 |

---

## 9. Candidate Requirements

The architecture ([SAD-001 §11](system-architecture-design.md#11-security-architecture)) describes controls that have no matching baselined requirement. These are **not** part of the baseline until validated.

| ID | Candidate requirement | Source | Suggested priority | Depends on |
| :--- | :--- | :--- | :---: | :--- |
| **CR-01** | Image upload controls: file-type allowlist, size limits, generated filenames, storage outside executable paths | SAD-001 §11 | Must | None |
| **CR-02** | Login rate limiting or throttling | SAD-001 §11 | Should | Hosting capabilities |
| **CR-03** | Audit log of administrative actions (role changes, account creation, permanent deletion) | SAD-001 §7 | Could | None |
| **CR-04** | Enforcement of enquiry retention (anonymize or purge after retention period) | ADR-012 | Should | OI-10 |

---

## 10. Stakeholder Validation and Sign-Off

*This document is a baseline proposal pending formal approval.*

| Role | Assigned name | Status | Date |
| :--- | :--- | :--- | :--- |
| Club Representative | *To be confirmed* | Pending | n/a |
| Public Image Chair | *To be confirmed* | Pending | n/a |
| Lead Developer | *To be completed* | Pending | n/a |

## 11. Revision Log

| Version | Date | Description | Author |
| :--- | :--- | :--- | :--- |
| 1.0 | 18 October 2026 | Initial draft | Project Developer |
| 1.1 | 18 October 2026 | Recommendations incorporated | Project Developer |
| 1.2 | 18 October 2026 | Consolidation: reorganized sections; added permission matrix, content states, canonical data model, NFR priorities and test links, acceptance criteria for all FRs, candidate requirements, related-document links; clarified budget; unified terminology | Project Developer |
