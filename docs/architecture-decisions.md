# Architecture Decision Records (ADR Log)

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website |
| **Document ID** | ADR-LOG-001 *(formerly ADR-INDEX-001)* |
| **Version** | 1.2 |
| **Status** | Accepted decisions plus proposals awaiting validation |
| **Last updated** | 18 October 2026 |
| **Owner** | Project Developer |
| **Related** | [SRS-001](software-requirements-specification.md), [SAD-001](system-architecture-design.md), [RTM-001](requirements-traceability-matrix.md) |

---

## 1. Purpose

This log records significant technical decisions, their context, the options considered, and their consequences. It prevents repeated debate and guides future maintainers.

**Status values:** **Accepted** (decided), **Proposed** (recommended, awaiting confirmation), **Merged** or **Superseded** (replaced by another record). Open issues are tracked in [SRS-001 §8](software-requirements-specification.md#8-open-issues-and-decisions).

## 2. Decision Index

| ID | Decision | Status | Focus | Related |
| :--- | :--- | :---: | :--- | :--- |
| [ADR-001](#adr-001-use-a-phpmysql-server-rendered-application) | PHP/MySQL server-rendered stack | Accepted | Technology | NFR-010 |
| [ADR-002](#adr-002-use-a-modular-monolith) | Modular monolith | Accepted | Architecture | NFR-010 |
| [ADR-003](#adr-003-build-for-one-club-first-with-reusable-core-code) | Single club first, reusable core | Accepted | Scope, reuse | FR-017 |
| [ADR-004](#adr-004-use-role-based-authorization-with-server-side-checks) | Role-based authorization and direct publishing permissions | Proposed | Security, access | FR-012, OI-01, OI-02 |
| [ADR-005](#adr-005-defer-member-accounts) | Defer member accounts | Accepted | Scope | FR-018 |
| [ADR-006](#adr-006-authorized-content-managers-can-publish-directly) | Direct publishing | Accepted | Content workflow | FR-004, FR-006, FR-008 |
| [ADR-007](#adr-007-use-mysql-with-pdo-and-parameterized-queries) | MySQL with PDO and parameterized queries | Accepted | Data, security | NFR-003, NFR-007 |
| [ADR-008](#adr-008-keep-the-web-document-root-in-public) | Web document root in `public/` | Proposed | Security, hosting | NFR-009 |
| [ADR-009](#adr-009-store-secrets-outside-version-control) | Secrets outside version control | Accepted | Security | NFR-009 |
| [ADR-010](#adr-010-use-incremental-development-and-traceability) | Incremental development and traceability | Accepted | Process | RTM-001 |
| [ADR-011](#adr-011-hosting-and-domain-provider) | Hosting and domain provider | Proposed | Infrastructure | OI-06 |
| [ADR-012](#adr-012-content-management-deletion-and-enquiry-retention) | Deletion and enquiry retention | Proposed | Governance | OI-04, OI-10 |
| [ADR-013](#adr-013-role-model-and-content-publishing) | Role model and publishing | Merged into ADR-004 | n/a | n/a |
| [ADR-014](#adr-014-store-enquiries-first-notify-optionally) | Store enquiries first, notify optionally | Proposed | Operations | FR-009, FR-010, OI-03 |

---

## 3. Decisions

### ADR-001: Use a PHP/MySQL Server-Rendered Application

| Status | Date | Related |
| :--- | :--- | :--- |
| **Accepted** | 18 October 2026 | NFR-010 |

**Context.** The developer prefers HTML, CSS, JavaScript, PHP, and MySQL and wants to balance learning, code quality, and delivery speed.

**Options considered**

| Option | Pros | Cons | Outcome |
| :--- | :--- | :--- | :---: |
| **PHP/MySQL server-rendered (PDO)** | Simple deployment, matches skills, single codebase | Needs discipline to keep layers separate | **Selected** |
| React frontend plus REST API | Modern client experience | Two applications to build and host | Rejected |
| Node.js backend | One language throughout | New backend conventions to learn | Rejected |
| PHP framework (for example Laravel) | Built-in authentication and routing | Steeper initial learning curve | Deferred |
| Hosted website builder | Fastest setup | Limited customization, low learning value | Rejected |

**Decision.** Use a server-rendered PHP application with MySQL, accessed through PDO.

**Consequences**

* Release 1 is a single deployable unit.
* All database access uses PDO prepared statements.
* Re-evaluate a framework if authentication, routing, or migrations become too complex to build safely.

---

### ADR-002: Use a Modular Monolith

| Status | Date | Related |
| :--- | :--- | :--- |
| **Accepted** | 18 October 2026 | NFR-010, SAD-001 §3 |

**Context.** The application needs public pages, content administration, authentication, and database access, and one developer maintains it.

**Decision.** Build one deployable application divided into clearly separated modules and layers.

```text
+------------------------------------------------------------------+
|                          MODULAR MONOLITH                        |
|                                                                  |
|  [ Public Views ]   [ Admin Views ]   [ Controllers / Routing ]  |
|         \                 |                 /                    |
|          +----------------+----------------+                     |
|                           |                                      |
|                  [ Application Logic ]                           |
|                           |                                      |
|                  [ Database / Repositories ]                     |
+------------------------------------------------------------------+
```

**Consequences**

* No single-file scripts that mix HTML, logic, and SQL.
* SQL stays out of view templates.
* Shared layouts and reusable partial templates are used.

Module layout: [SAD-001 §6 and §13](system-architecture-design.md#6-application-modules).

---

### ADR-003: Build for One Club First, with Reusable Core Code

| Status | Date | Related |
| :--- | :--- | :--- |
| **Accepted** | 18 October 2026 | FR-017, CHR-001 §7 |

**Context.** Release 1 serves the Rotaract Club of UNZA. The core should be reusable by other clubs later without building multi-tenancy now.

**Options considered**

| Strategy | Effort | Security risk | Outcome |
| :--- | :---: | :---: | :---: |
| **Single tenant (UNZA) with configuration separated from logic** | Low | Low | **Selected** |
| Multi-tenant (shared database with `tenant_id`) | High | High (data-leakage risk) | Deferred |
| Hard-coded UNZA-only script | Very low | Low | Rejected |

**Decision.** Single-club deployment. Another club reuses the code through configuration and its own deployment.

**Consequences**

* No tenant identifier columns in the Release 1 schema.
* Club-specific settings (branding, credentials) live in configuration, not in logic.
* Deployment steps are documented to ease setup for future clubs.

---

### ADR-004: Use Role-Based Authorization with Server-Side Checks

| Status | Date | Related |
| :--- | :--- | :--- |
| **Proposed** *(pending role-assignment confirmation, OI-01)* | 18 October 2026 | FR-011, FR-012, FR-016, NFR-008; absorbs ADR-013 |

**Context.** Different club officers need different powers, and the club has little tolerance for access mistakes or lockouts.

**Decision.** Use three application roles (Site Administrator, Content Manager, Enquiry Manager) with server-side checks on every protected action. Technical Maintainer is an operational role with no application permissions. Content Managers publish directly (ADR-006), while permanent deletion and administration stay with Site Administrators. The authoritative permission matrix is in [SRS-001 §3.2](software-requirements-specification.md#32-permission-matrix).

**Rules**

1. **Server-side verification.** Every protected endpoint re-checks authorization. Hiding buttons is not security.
2. **Individual accounts only.** No shared credentials and no public self-registration.
3. **Redundancy.** Keep at least two active Site Administrator accounts.
4. **Least privilege.** A user holds only the roles they need; Technical Maintainer access is time-limited.
5. **Account provisioning.** If the account-management UI (FR-016) is not ready for launch, create accounts through a documented secure procedure (OI-02).

---

### ADR-005: Defer Member Accounts

| Status | Date | Related |
| :--- | :--- | :--- |
| **Accepted** | 18 October 2026 | FR-018 |

**Decision.** Public users need no login. Member registration, profiles, and member-only areas are deferred beyond Release 1.

---

### ADR-006: Authorized Content Managers Can Publish Directly

| Status | Date | Related |
| :--- | :--- | :--- |
| **Accepted** | 18 October 2026 | FR-004, FR-006, FR-008 |

**Decision.** Authorized users publish content directly; there is no approval pipeline in Release 1.

```text
Release 1 :  Draft ---> (Content Manager publishes) ---> Published (live)

Deferred  :  Draft ---> Submit for approval ---> Review ---> Published (live)
```

**Consequences**

* Content carries an explicit state: **Draft**, **Published**, or **Archived** ([SRS-001 §5.1](software-requirements-specification.md#51-content-states)).
* Public pages and direct URLs never expose Draft or Archived items.

---

### ADR-007: Use MySQL with PDO and Parameterized Queries

| Status | Date | Related |
| :--- | :--- | :--- |
| **Accepted** | 18 October 2026 | NFR-003, NFR-007, NFR-009 |

| Risk | Control |
| :--- | :--- |
| SQL injection | Mandatory PDO parameterized queries |
| Cross-site scripting (XSS) | Context-aware HTML escaping when rendering views |
| Data integrity faults | Foreign keys and database constraints |
| Credential exposure | Credentials kept in `.env` outside the public tree |

---

### ADR-008: Keep the Web Document Root in `public/`

| Status | Date | Related |
| :--- | :--- | :--- |
| **Proposed** *(depends on hosting, ADR-011)* | 18 October 2026 | NFR-009, SAD-001 §13 |

**Decision.** Only the `public/` directory is web-accessible. Application code, configuration, migrations, and secrets sit outside it.

```text
rotaract-unza-website/      <-- not web-accessible
├── app/                    <-- application code
├── config/                 <-- configuration
├── database/               <-- migrations and seeds
├── storage/                <-- logs, private uploads
├── .env                    <-- secrets (never committed)
└── public/                 <-- WEB ROOT
    ├── index.php           <-- front controller
    └── assets/             <-- static CSS, JS, images
```

**Fallback.** If the provider cannot set the document root to `public/`, adapt deployment so that source, credentials, logs, and private uploads cannot be requested through the browser ([SAD-001 §13](system-architecture-design.md#13-suggested-source-code-structure)). Full directory structure: SAD-001 §13.

---

### ADR-009: Store Secrets Outside Version Control

| Status | Date | Related |
| :--- | :--- | :--- |
| **Accepted** | 18 October 2026 | NFR-009 |

**Rules**

* Commit `.env.example` with placeholder values only.
* Never commit `.env` or production credentials.
* List sensitive configuration files in `.gitignore`.

---

### ADR-010: Use Incremental Development and Traceability

| Status | Date | Related |
| :--- | :--- | :--- |
| **Accepted** | 18 October 2026 | RTM-001, CHR-001 §6 |

**Decision.** Deliver by 30 November 2026 through small end-to-end increments.

* Build each feature end to end: route, logic, database, test.
* Keep the RTM current so every requirement links to tests.
* Do Must requirements first; defer optional scope.

---

### ADR-011: Hosting and Domain Provider

| Status | Date | Related |
| :--- | :--- | :--- |
| **Proposed** *(provider not yet selected, OI-06)* | 18 October 2026 | SRS-001 §7, SAD-001 §10 |

**Decision (proposed).** Use managed shared PHP/MySQL hosting, with a year-one budget of **US$50–100** for domain plus hosting. Compare at least two providers; domain and hosting accounts are registered to the club or an officially designated club representative, not only to the developer.

**Provider checklist**

| Requirement | Need | Provider A | Provider B |
| :--- | :---: | :---: | :---: |
| PHP 8.x support | Mandatory | To check | To check |
| MySQL/MariaDB with PDO | Mandatory | To check | To check |
| HTTPS with free SSL | Mandatory | To check | To check |
| SFTP | Mandatory | To check | To check |
| Custom web root (`public/`) | Strongly preferred; otherwise use the ADR-008 fallback | To check | To check |
| Automated database and file backups | Mandatory | To check | To check |
| Club-owned registration | Mandatory | To check | To check |
| Renewal price | Record for comparison | To check | To check |

---

### ADR-012: Content Management, Deletion and Enquiry Retention

| Status | Date | Related |
| :--- | :--- | :--- |
| **Proposed** *(awaiting club sign-off, OI-04, OI-10)* | 18 October 2026 | FR-004, FR-006, FR-008, FR-010, CR-04 |

**Decision (proposed).** Archive first; restrict permanent deletion to Site Administrators.

| Data | Standard removal | Who may act | Proposed retention |
| :--- | :--- | :--- | :--- |
| Events, news, projects | Archive (soft delete); permanent delete only by Site Administrator | Content Managers archive; Site Administrators delete | Until archived or deleted by choice |
| Contact enquiries | Soft-delete or anonymize after closure | Enquiry Managers and Site Administrators; purge by Site Administrator or automatic | Delete or anonymize 12 months after closure |
| Audit logs *(if implemented, CR-03)* | System purge | Site Administrators | 12 months |

---

### ADR-013: Role Model and Content Publishing

| Status | Date | Related |
| :--- | :--- | :--- |
| **Merged into [ADR-004](#adr-004-use-role-based-authorization-with-server-side-checks)** | 18 October 2026 | n/a |

This record duplicated ADR-004 and ADR-006. Its content (least-privilege roles, direct publishing, deletion and administration reserved for Site Administrators) is now held in ADR-004 and ADR-006. The ID is kept so existing references remain valid.

---

### ADR-014: Store Enquiries First, Notify Optionally

| Status | Date | Related |
| :--- | :--- | :--- |
| **Proposed** *(OI-03 open)* | 18 October 2026 | FR-009, FR-010, OI-03 |

**Decision (proposed).** Every contact enquiry is saved to the database first. Email notification is optional and only enabled after SMTP is configured and tested. If email fails or is not configured, the enquiry remains safely in the dashboard and the visitor sees a normal success message with no technical error.

Required and optional fields for enquiries and all content types are defined in [SRS-001 §5](software-requirements-specification.md#5-data-model).

---

## 4. Decision Review Triggers

Revisit these decisions if any of the following occurs:

* **Multi-tenancy:** the platform must natively host several independent clubs (ADR-003).
* **Authentication scope:** member accounts become mandatory (ADR-005).
* **Team scaling:** more than one developer or a need for continuous integration (ADR-001, ADR-002).
* **API needs:** an external integration requires a public REST or GraphQL API.
* **Hosting limits:** the provider cannot serve from `public/` (ADR-008, ADR-011).

## 5. Revision Log

| Version | Date | Description | Author |
| :--- | :--- | :--- | :--- |
| 1.0 | 18 October 2026 | Initial decisions | Project Developer |
| 1.1 | 18 October 2026 | Added ADR-011 to ADR-014 and recommendations | Project Developer |
| 1.2 | 18 October 2026 | Consolidation: renamed document ID to ADR-LOG-001; standardized every record (status, date, related items); merged ADR-013 into ADR-004; set ADR-012 to Proposed; field lists moved to SRS §5; directory structure moved to SAD §13; added related requirements; removed stray text | Project Developer |
