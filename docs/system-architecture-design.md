# System Architecture Design (SAD)

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website |
| **Document ID** | SAD-001 |
| **Version** | 1.1 |
| **Status** | Draft for review |
| **Last updated** | 18 October 2026 |
| **Target launch** | 30 November 2026 |
| **Requirements baseline** | [SRS-001](software-requirements-specification.md) v1.2 |
| **Prepared for** | Rotaract Club of UNZA |
| **Owner** | Project Developer |
| **Architecture style** | Modular monolith, organized in layers using MVC principles |
| **Related** | [ADR-LOG-001](architecture-decisions.md), [RTM-001](requirements-traceability-matrix.md), [STK-001](stakeholder-register.md) |

---

## 1. Purpose

This document describes the high-level architecture of the Rotaract Club of UNZA website. It turns the requirements into a technical design that guides database design, UI/UX design, implementation, testing, and deployment.

Release 1 serves one club. The code is organized so useful modules can be reused for other clubs later, but Release 1 is **not** a multi-tenant platform ([ADR-003](architecture-decisions.md#adr-003-build-for-one-club-first-with-reusable-core-code)).

## 2. Architectural Goals

The architecture should:

1. Support the public website and a protected administrator dashboard.
2. Support events, news, service projects and impact reports, contact enquiries, and club settings.
3. Use HTML, CSS, JavaScript, PHP, and MySQL.
4. Be feasible for one developer who is learning while building.
5. Deploy to managed PHP/MySQL hosting without complex infrastructure.
6. Encourage separation of concerns, testability, and reuse.
7. Protect administrator accounts, contact enquiries, and uploaded media.
8. Meet the 30 November 2026 target by avoiding unnecessary complexity.

## 3. Selected Architecture

### 3.1 Decision

Use a **modular monolith**, organized into **layers**, applying **MVC principles** where useful ([ADR-001](architecture-decisions.md#adr-001-use-a-phpmysql-server-rendered-application), [ADR-002](architecture-decisions.md#adr-002-use-a-modular-monolith)). These choices complement each other:

| Concept | What it means here |
| :--- | :--- |
| **Modular monolith** | One deployable application divided into clearly bounded business modules |
| **Layered architecture** | Separates presentation, application logic, and data access |
| **MVC** | Separates request handling (controllers), data and rules (models), and templates (views) |

### 3.2 Why This Fits

* Several related features, but no need for independently deployed services.
* One PHP application and one MySQL database keep hosting and deployment simple.
* Module boundaries prevent one large, tangled codebase.
* Practical for a solo developer with a near-term launch.
* Modules can be reused or extracted later if a real need appears.

Microservices, Kubernetes, message brokers, and an API gateway are out of scope for Release 1.

## 4. System Context

```text
                    +--------------------------+
                    | Public visitor / member  |
                    | Browser on phone or PC   |
                    +------------+-------------+
                                 |
                                 | HTTPS
                                 v
                    +--------------------------+
                    | Rotaract Club of UNZA    |
                    | Website                  |
                    | Public pages + Admin UI  |
                    +------------+-------------+
                                 |
                                 | Server-side PHP
                                 v
                    +--------------------------+
                    | MySQL database           |
                    | Content, users, enquiries|
                    +--------------------------+

    Authorized administrators use the protected dashboard.
    Visitors view published content and submit contact enquiries.
```

| External actor | Interaction |
| :--- | :--- |
| **Public visitor** | Reads published content; submits the contact form |
| **Authorized administrator** | Signs in and performs actions allowed by assigned roles |
| **Hosting provider** | Runs PHP and MySQL, provides HTTPS, supports backups and restoration |
| **Club content owners** | Supply approved text, event details, project results, and images to authorized publishers |

## 5. Logical Architecture

```text
+------------------------------------------------------------------+
|                         PRESENTATION                             |
| Public pages | Admin dashboard | Forms | Shared layouts          |
| HTML templates | CSS | JavaScript                                |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                     HTTP / CONTROLLER LAYER                      |
| Routes | Controllers | Request parsing | Response handling        |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                    APPLICATION / SERVICE LAYER                   |
| Validation | Use cases | Authorization checks | Content workflows |
| Authentication service | Enquiry handling | Club settings        |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                       DOMAIN / MODEL LAYER                       |
| Users and roles | Events | News | Projects | Enquiries | Settings |
| Domain rules and state transitions                               |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                        DATA ACCESS LAYER                         |
| Repositories | PDO | Transactions                                |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                              MYSQL                               |
| Persistent application data                                      |
+------------------------------------------------------------------+
```

The layers describe logical responsibilities. They all run inside the same PHP application.

## 6. Application Modules

Module names here are used in the requirement tables of the SRS and RTM.

| Module | Responsibilities | Main data | Requirements |
| :--- | :--- | :--- | :--- |
| **Public Website** | Home, About, navigation, public layouts | Club settings, published content | FR-001, FR-002, FR-018 |
| **Authentication** | Login, logout, password verification, session lifecycle | Users, password hashes, account status | FR-011 |
| **Authorization** | Role and permission checks on every protected action | Roles, user-role assignments | FR-012 |
| **Users** | Administrator account management | Users, roles | FR-016 |
| **Events** | Create, edit, publish, unpublish, archive, display events | Events | FR-003, FR-004 |
| **News** | Manage and display articles and announcements | News articles | FR-005, FR-006 |
| **Projects** | Manage and display service projects, reports, approved impact figures | Projects | FR-007, FR-008 |
| **Contact** | Validate and store enquiries, track status, support retention | Enquiries | FR-009, FR-010 |
| **Admin** | Dashboard summaries and links to management tasks | Aggregated module data | FR-013, FR-014 |
| **Settings** | Public club details and branding settings | Club settings | FR-015, FR-017 |
| **Media** | Validate, store, and serve uploaded images | Media metadata and files | FR-004, FR-006, FR-008, CR-01 |
| **Logging** | Record administrative actions and operational errors where practical | Audit and error logs | NFR-015, CR-03 |

Keep modules focused. Do not build a generic content-management framework before the core use cases work.

## 7. Role and Permission Boundaries

Roles and the permission matrix are defined once, in [SRS-001 §3](software-requirements-specification.md#3-stakeholders-and-access-model). In summary:

* **Site Administrator:** accounts, settings, all content, and enquiries.
* **Content Manager:** create, edit, publish, unpublish, and archive events, news, projects, and impact reports.
* **Enquiry Manager:** view and process contact enquiries.
* **Technical Maintainer:** operational role for deployment only; no application permissions.

A user may hold more than one application role. Exact assignments are confirmed by the club (OI-01).

**Security rules for the design**

1. The server checks permissions for every protected action; hiding a button is not security.
2. Public users must not reach the dashboard or private enquiries.
3. No public administrator registration exists.
4. Each administrator has an individual account.
5. Keep Site Administrator accounts to a small designated group.
6. If a full account-management interface is not ready by launch, create accounts through a documented secure procedure (OI-02).
7. Log important actions such as role changes, account creation, and permanent deletion where practical (CR-03).

## 8. Key Request Flows

### 8.1 Public Visitor Views an Event

1. The browser requests the Events page over HTTPS.
2. The route sends the request to the Events controller.
3. The controller asks the Events service or repository for Published events.
4. The repository queries MySQL through PDO with prepared statements.
5. The controller passes the results to the view.
6. The view renders HTML; CSS and JavaScript provide presentation and optional interaction.
7. The browser shows Published events only.

### 8.2 Administrator Publishes News

1. The administrator submits the login form over HTTPS.
2. Authentication verifies the password hash and starts a secure session.
3. The administrator opens the news form.
4. The server checks permission to create or publish news.
5. The application validates and normalizes the submitted fields.
6. The repository writes the article to MySQL with prepared statements.
7. The application records publication metadata and, where implemented, an audit event.
8. The public News page shows the article because its state is Published.

There is no separate approval step in Release 1 ([ADR-006](architecture-decisions.md#adr-006-authorized-content-managers-can-publish-directly)).

### 8.3 Visitor Submits a Contact Enquiry

1. The visitor submits the contact form over HTTPS.
2. The server validates required fields and applies basic anti-abuse controls.
3. The application stores the enquiry in MySQL.
4. The visitor sees a generic success message that exposes no internal data.
5. An authorized Enquiry Manager reviews the enquiry and updates its status.
6. Email notification is optional and enabled only after configuration and testing ([ADR-014](architecture-decisions.md#adr-014-store-enquiries-first-notify-optionally)).

## 9. Data and Persistence Principles

The detailed schema comes from the database design phase. Entities and fields are defined in [SRS-001 §5](software-requirements-specification.md#5-data-model). The core entities are users, roles and assignments, events, news, projects, enquiries, club settings, media metadata, and an optional audit log.

* Use primary keys and foreign keys where appropriate.
* Store creation and update timestamps; record publication time for published content.
* Represent content state explicitly: Draft, Published, or Archived.
* Use transactions for operations that must succeed or fail together.
* Use PDO prepared statements for all database access.
* Never store plaintext passwords.
* Store uploaded files apart from their metadata; never keep executable uploads in a public directory.
* Prefer archive (soft delete) for content; restrict permanent deletion to Site Administrators.
* Define enquiry retention and deletion rules with the club before launch ([ADR-012](architecture-decisions.md#adr-012-content-management-deletion-and-enquiry-retention), OI-10).

## 10. Deployment Architecture

```text
                  Visitor / Administrator Browser
                               |
                               | HTTPS
                               v
                    +----------------------+
                    | DNS + Domain         |
                    | Managed PHP Hosting  |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Web server           |
                    | Apache or equivalent |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | PHP application      |
                    | Public + Admin       |
                    +----------+-----------+
                               |
                         Private DB access
                               |
                               v
                    +----------------------+
                    | MySQL / MariaDB      |
                    +----------------------+

     Backups: database export + media/application backup
     Secrets: environment/config outside the public web root
```

**Hosting recommendation.** Use managed shared PHP/MySQL hosting, subject to verifying support for a current PHP 8.x, MySQL/MariaDB, HTTPS, backup and restoration, SFTP, and configuration outside the public web root ([ADR-011](architecture-decisions.md#adr-011-hosting-and-domain-provider)).

Provider, domain, final price, and backup schedule remain open (OI-06). Domain and hosting accounts belong to the club or an officially designated club representative, not solely to the developer.

## 11. Security Architecture

Security controls tie directly to non-functional requirements and must be tested; listing them here does not make the implementation secure.

| Control | Requirement |
| :--- | :--- |
| HTTPS for all public and administrative traffic | NFR-008 |
| `password_hash()` and `password_verify()` | NFR-004 |
| Server-side authentication and authorization checks | FR-011, FR-012 |
| PDO prepared statements | NFR-003 |
| Server-side validation of all input | NFR-005 |
| Context-appropriate output escaping | NFR-007 |
| CSRF tokens on state-changing forms | NFR-006 |
| Secure cookies, session ID regeneration after login, logout and expiry | NFR-008 |
| Secrets outside version control and outside the public web root | NFR-009 |
| Generic user-facing errors; detailed diagnostics in protected logs | NFR-015 |
| Backups with a tested restoration procedure | NFR-014 |
| Restricted access to enquiries and administrative logs | FR-010, FR-012 |
| Login rate limiting or throttling suited to the hosting | CR-02 *(candidate)* |
| Upload allowlists, size limits, MIME and type checks, generated filenames, storage outside executable and public paths where possible | CR-01 *(candidate)* |

## 12. Quality Attributes and Design Responses

| Quality attribute | Architectural response | Requirements |
| :--- | :--- | :--- |
| Maintainability | Separate modules and layers; shared layouts and utilities | NFR-010 |
| Security | Central authorization checks, PDO, validation, escaping, CSRF, session controls | NFR-003 to NFR-009, NFR-015 |
| Usability | Responsive public pages, clear navigation, consistent administration forms | NFR-001, NFR-002, NFR-011, NFR-012 |
| Performance | Efficient queries, pagination where needed, optimized images, appropriate indexes | NFR-013 |
| Availability | Managed hosting, backups, documented recovery | NFR-014 |
| Portability | Avoid provider-specific services; document configuration | ADR-011 |
| Reusability | Keep club-specific values in configuration; modules stay club-neutral | FR-017 |
| Testability | Keep business rules out of templates; test validation, permissions, and core use cases | RTM-001 |

## 13. Suggested Source-Code Structure

This is the single reference layout for the project. Adjust it during implementation if needed.

```text
rotaract-unza-website/
├── public/                    # Only publicly accessible web files
│   ├── index.php              # Front controller / entry point
│   └── assets/
│       ├── css/
│       ├── js/
│       └── images/
├── app/
│   ├── Core/                  # Router, database connection, error handler, helpers
│   ├── Middleware/            # Authentication, authorization, CSRF checks
│   ├── Modules/
│   │   ├── Auth/
│   │   ├── Users/
│   │   ├── Events/
│   │   ├── News/
│   │   ├── Projects/
│   │   ├── Contact/
│   │   ├── Admin/
│   │   ├── Settings/
│   │   └── Media/
│   └── Shared/                # Shared layouts, validation, view helpers, utilities
├── config/                    # Non-secret defaults; secrets injected securely
├── database/
│   ├── migrations/
│   └── seeds/
├── storage/                   # Logs and non-public uploads where supported
├── tests/
├── docs/                      # Project documentation and ADRs
├── .env.example               # Placeholder configuration (committed)
├── .gitignore                 # Excludes .env and other secrets
└── README.md
```

Within each module, keep controller, service, repository, model, and view files together so the module's responsibilities stay visible.

**Deployment rule.** Point the hosting document root at `public/` if the provider allows it ([ADR-008](architecture-decisions.md#adr-008-keep-the-web-document-root-in-public)). If not, adapt deployment so application source, credentials, logs, and private uploads cannot be requested through the browser.

## 14. Out of Scope for Release 1

* Microservices or distributed services.
* Public member registration portal.
* Multi-club tenancy or a shared platform for several clubs.
* Complex content-approval workflow.
* Online payments and membership-fee processing.
* Event ticketing or registration, unless separately prioritized.
* Native mobile applications.
* Complex API for third-party integrations.
* Advanced analytics beyond practical dashboard summaries.

These may be reconsidered if stakeholders identify a clear need.

## 15. Risks and Mitigations

| ID | Risk | Mitigation |
| :--- | :--- | :--- |
| R-01 | Scope grows before launch | Prioritize public pages, admin login, publishing, and enquiries |
| R-02 | Permissions are too broad | Confirm the role matrix; test every protected action (TC-SEC-001) |
| R-03 | Hosting prevents safe file separation | Verify document-root and configuration options before purchase |
| R-04 | Content or images arrive late | Agree content owners; use approved placeholders during development |
| R-05 | Data loss or accidental deletion | Backups, archive-first behaviour, and a restoration test |
| R-06 | Security work is postponed | Include security checks in each module's implementation and test plan |
| R-07 | Future reuse becomes difficult | Keep club identity and configuration separate from reusable modules |

## 16. Open Decisions

These are tracked in [SRS-001 §8](software-requirements-specification.md#8-open-issues-and-decisions).

| Decision needed | Tracked as |
| :--- | :--- |
| Final role assignments | OI-01 |
| Whether account management has a UI at launch | OI-02 |
| Whether Site Administrators alone may permanently delete content | OI-04 |
| Hosting provider, domain, renewal budget, backup schedule | OI-06 |
| Whether email notifications are needed for enquiries | OI-03 |
| Content fields and states for events, news, projects, and reports | OI-05 |
| Enquiry retention period | OI-10 |
| Whether club settings are edited in the dashboard or by configuration only | OI-09 |
| Who owns content updates and technical maintenance after launch | OI-08 |

## 17. Architecture Acceptance Checklist

- [ ] The public website and protected admin area are both represented.
- [ ] Each module has a clear responsibility.
- [ ] All protected actions require server-side permission checks.
- [ ] Database access uses PDO prepared statements.
- [ ] The design supports secure authentication and sessions.
- [ ] Private application files and secrets are not publicly accessible.
- [ ] Release 1 is a single-club application.
- [ ] Hosting supports the selected PHP/MySQL design.
- [ ] Backup and restoration arrangements are documented.
- [ ] The architecture has been checked against the SRS and the RTM.

## 18. Next Steps and Approval

**Next design deliverable:** database entity-relationship diagram (ERD), followed by page wireframes.

**Approval note:** this is a proposed design. Review it against SRS-001 and confirm it with the club before treating it as the final baseline.

## 19. Revision Log

| Version | Date | Description | Author |
| :--- | :--- | :--- | :--- |
| 1.0 | 18 October 2026 | Initial architecture draft | Project Developer |
| 1.1 | 18 October 2026 | Consolidation: standardized header; converted tables to standard Markdown; added Users module, requirement mapping, and `.env.example`; linked security controls to NFR and CR IDs; replaced duplicated role and field text with SRS references; added risk IDs; mapped open decisions to `OI` IDs; added revision log | Project Developer |
