# Architecture Decision Records (ADR)

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website |
| **Document ID** | `ADR-INDEX-001` |
| **Version** | 1.1 |
| **Status** | Initial decisions plus recommendations; selected items require validation |
| **Last Updated** | 18 October 2026 |

---

## 1. Purpose

This document records significant technical decisions, their context, options considered, and consequences to prevent repeated debate and guide future maintainers.

### Summary Index of Decisions

| ID | Title | Status | Primary Focus |
| :--- | :--- | :---: | :--- |
| **ADR-001** | [PHP/MySQL Server-Rendered Stack](#adr-001-use-a-phpmysql-server-rendered-application) | `ACCEPTED` | Technology Stack |
| **ADR-002** | [Modular Monolith Architecture](#adr-002-use-a-modular-monolith) | `ACCEPTED` | Application Architecture |
| **ADR-003** | [Single-Club First, Reusable Core](#adr-003-build-for-one-club-first-with-reusable-core-code) | `ACCEPTED` | Scope & Reusability |
| **ADR-004** | [Role-Based Authorization (Server-Side)](#adr-004-use-role-based-authorization-with-server-side-checks) | `PROPOSED` | Security & Access |
| **ADR-005** | [Defer Member Accounts](#adr-005-defer-member-accounts) | `ACCEPTED` | Scope & Features |
| **ADR-006** | [Direct Publishing by Authorized Managers](#adr-006-authorized-content-managers-can-publish-directly) | `ACCEPTED` | Content Management |
| **ADR-007** | [MySQL Persistence with PDO & Parameterized Queries](#adr-007-use-mysql-with-pdo-and-parameterized-queries) | `ACCEPTED` | Database & Security |
| **ADR-008** | [Web Document Root in `public/`](#adr-008-keep-the-web-document-root-in-public) | `PROPOSED` | Security & Hosting |
| **ADR-009** | [Secrets Outside Version Control](#adr-009-store-secrets-outside-version-control) | `ACCEPTED` | Security & Config |
| **ADR-010** | [Incremental Development & Traceability](#adr-010-use-incremental-development-and-traceability) | `ACCEPTED` | Process & Methodology |
| **ADR-011** | [Hosting and Domain Provider](#adr-011-hosting-and-domain-provider) | `PROPOSED` | Infrastructure & Cost |
| **ADR-012** | [Content Management, Deletion & Enquiry Retention](#adr-012-content-management-deletion-and-enquiry-retention) | `PENDING` | Governance & Privacy |
| **ADR-013** | [Role Model & Content Publishing Permissions](#adr-013-role-model-and-content-publishing) | `PROPOSED` | Governance & Security |
| **ADR-014** | [Content Schema & Contact Enquiries Workflow](#adr-014-content-fields-and-contact-enquiries) | `PROPOSED` | Data & Operations |

---

## Architecture Decisions

### ADR-001: Use a PHP/MySQL Server-Rendered Application

| Attribute | Details |
| :--- | :--- |
| **Status** | **`ACCEPTED`** |
| **Decision Date** | 18 October 2026 |

#### Context
The developer prefers HTML, CSS, JavaScript, PHP, and MySQL and aims to balance personal learning, code quality, and delivery speed.

#### Comparison of Options
| Architecture Option | Pros | Cons | Decision |
| :--- | :--- | :--- | :---: |
| **PHP/MySQL Server-Rendered (PDO)** | Simple deployment, matches developer skills, single codebase | Requires discipline to maintain layout separation | **Selected** |
| **React Frontend + REST API** | Modern client experience | High complexity, overhead of two separate applications | Rejected |
| **Node.js Backend** | Single language (JS) throughout | Requires learning new backend conventions | Rejected |
| **PHP Framework (e.g., Laravel)** | Built-in authentication & routing | Higher initial learning curve for custom setup | Deferred |
| **Hosted Website Builder** | Very fast setup | Limited customization, low learning value | Rejected |

#### Rationale & Consequences
* **Rationale:** A server-rendered PHP application fits the target stack, avoids API overhead, and provides practical full-stack experience.
* **Consequences:**
  * Keep the initial release as a single deployable unit.
  * Use PHP PDO with prepared statements for database operations.
  * Re-evaluate frameworks (e.g., Laravel) if auth, routing, and migrations become too complex to build safely.

---

### ADR-002: Use a Modular Monolith

| Attribute | Details |
| :--- | :--- |
| **Status** | **`ACCEPTED`** |
| **Decision Date** | 18 October 2026 |

#### Context
The application needs public pages, content administration, authentication, and database access. It is managed by a single developer.

#### Architectural Strategy
```
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

#### Consequences
* Avoid monolithic single-file scripts (e.g., `index.php` with embedded SQL and HTML).
* Keep SQL queries out of view templates.
* Use shared layouts and reusable partial components.

---

### ADR-003: Build for One Club First, with Reusable Core Code

| Attribute | Details |
| :--- | :--- |
| **Status** | **`ACCEPTED`** |
| **Decision Date** | 18 October 2026 |

#### Context
The initial release targets the **Rotaract Club of UNZA**. The core codebase should be reusable for other clubs in the future without building a full multi-tenant architecture immediately.

#### Deployment Strategy Matrix
| Strategy | Implementation Effort | Security Risk | Choice |
| :--- | :---: | :---: | :---: |
| **Single Tenant (UNZA) with Config Isolation** | Low | Low | **Selected** |
| **Multi-Tenant (Shared DB with `tenant_id`)** | High | High (Data leakage risk) | Deferred |
| **Hardcoded UNZA-only Script** | Very Low | Low | Rejected |

#### Consequences
* Do not add tenant identifier columns to database tables for the initial release.
* Separate configuration settings (branding, credentials, settings) from system logic.
* Document deployment procedures to streamline setup for future clubs.

---

### ADR-004: Use Role-Based Authorization with Server-Side Checks

| Attribute | Details |
| :--- | :--- |
| **Status** | **`PROPOSED`** *(Pending role assignment confirmation)* |
| **Decision Date** | 18 October 2026 |

#### Proposed Access Control Matrix

| Role | Target Users | Allowed Actions | Restricted Actions |
| :--- | :--- | :--- | :--- |
| **Site Administrator** | President & named technical successor | Manage users, configure system settings, delete content permanently, manage content | None |
| **Content Manager** | Board Members / Public Image Committee | Create, edit, publish, unpublish, and archive content | Account management, system configuration |
| **Enquiry Manager** | Club Secretary / Designated Officer | View and process public contact form enquiries | System settings, user account creation |

#### Key Rules
1. **Server-Side Verification:** Every protected endpoint must re-verify authorization. Never rely solely on hiding buttons/links in the client UI.
2. **Account Security:** Individual accounts only—shared credentials and public self-registration are strictly prohibited.
3. **Redundancy:** Maintain at least two active Site Administrator accounts to prevent lockouts.

---

### ADR-005: Defer Member Accounts

| Attribute | Details |
| :--- | :--- |
| **Status** | **`ACCEPTED`** |
| **Decision Date** | 18 October 2026 |

#### Decision
Public users do not require logins to access public content. Member registration, member profile management, and member-only portals are deferred to post-v1.0 releases.

---

### ADR-006: Authorized Content Managers Can Publish Directly

| Attribute | Details |
| :--- | :--- |
| **Status** | **`ACCEPTED`** |
| **Decision Date** | 18 October 2026 |

#### Workflow Comparison
```
[ Drafting ] ---> ( Direct Publish by Content Manager ) ---> [ Live Site ]
                                vs.
[ Drafting ] ---> [ Submit for Approval ] ---> [ Review ] ---> [ Live Site ] (Deferred)
```

#### Consequences
* No multi-step approval pipeline is needed for release 1.0.
* Content items must support explicit status flags (`draft`/`published`/`archived`).
* Direct public URL queries must never expose items in `draft` or `archived` states.

---

### ADR-007: Use MySQL with PDO and Parameterized Queries

| Attribute | Details |
| :--- | :--- |
| **Status** | **`ACCEPTED`** |
| **Decision Date** | 18 October 2026 |

#### Technical Safeguards
| Risk | Technical Control |
| :--- | :--- |
| **SQL Injection** | Mandatory use of PDO parameterized queries |
| **Cross-Site Scripting (XSS)** | Context-aware HTML escaping on view rendering |
| **Data Integrity Faults** | Strict foreign keys and database constraints |
| **Credential Exposure** | Credentials stored outside public tree in `.env` |

---

### ADR-008: Keep the Web Document Root in `public/`

| Attribute | Details |
| :--- | :--- |
| **Status** | **`PROPOSED`** |

#### Directory Security Structure
```
rotaract-website/             <-- RESTRICTED (Outside Web Root)
├── app/                      <-- Application Code & Logic
├── config/                   <-- DB Connections & Config
├── database/                 <-- SQL Migrations & Seeds
├── .env                      <-- Sensitive Credentials
└── public/                   <-- PUBLIC WEB ROOT (Exposed via HTTP)
    ├── index.php             <-- Front Controller Entry Point
    └── assets/               <-- Static CSS, JS, Images
```

---

### ADR-009: Store Secrets Outside Version Control

| Attribute | Details |
| :--- | :--- |
| **Status** | **`ACCEPTED`** |
| **Decision Date** | 18 October 2026 |

#### Rules
* Commit `.env.example` with placeholder values only.
* Never commit `.env` or production credentials to Git.
* Add sensitive configuration filenames to `.gitignore`.

---

### ADR-010: Use Incremental Development and Traceability

| Attribute | Details |
| :--- | :--- |
| **Status** | **`ACCEPTED`** |
| **Target Deadline** | 30 November 2026 |

#### Development Principles
* Build in small end-to-end features (Feature $\rightarrow$ Route $\rightarrow$ Logic $\rightarrow$ DB $\rightarrow$ Test).
* Maintain a Requirements Traceability Matrix linking requirements to test cases.
* Prioritize Must-have items first; defer optional scope additions.

---

### ADR-011: Hosting and Domain Provider

| Attribute | Details |
| :--- | :--- |
| **Status** | **`PROPOSED`** *(Provider selection pending)* |
| **Target Budget** | **US$50–100** (First-year allocation for domain + hosting) |

#### Provider Checklist & Requirements

| Metric / Feature | Mandatory Requirement | Candidate (e.g., Hostinger) |
| :--- | :---: | :---: |
| **PHP Runtime** | PHP 8.x support | Pending Check |
| **Database Support** | MySQL / MariaDB with PDO | Pending Check |
| **Security** | HTTPS / Free SSL Certificate | Pending Check |
| **Access Control** | Custom Web Root (`public/`) & SFTP | Pending Check |
| **Backups** | Automated periodic database/file backups | Pending Check |
| **Ownership** | Registered directly under Club Name | Pending Check |

---

### ADR-012: Content Management, Deletion and Enquiry Retention

| Attribute | Details |
| :--- | :--- |
| **Status** | **`PENDING`** *(Awaiting club sign-off)* |

#### Proposed Retention & Deletion Rules

| Data Type | Standard Deletion Method | Access Restrictions | Proposed Retention Policy |
| :--- | :--- | :--- | :--- |
| **Public Events / News** | Soft Delete / Archive | Content Managers | Indefinite until manually archived |
| **System Audit Logs** | System Purge | Site Administrators | 12 Months |
| **Contact Enquiries** | Soft Delete / Anonymize | Enquiry Managers | Auto-purge/Anonymize 12 months after closure |

---

### ADR-013: Role Model and Content Publishing

| Attribute | Details |
| :--- | :--- |
| **Status** | **`PROPOSED`** |

#### Summary
Implements least-privilege role boundaries (Administrator, Content Manager, Enquiry Manager). Direct publishing is enabled for Content Managers to streamline operations. Permanent deletion and administrative functions remain strictly reserved for Site Administrators.

---

### ADR-014: Content Fields and Contact Enquiries

| Attribute | Details |
| :--- | :--- |
| **Status** | **`PROPOSED`** |

#### Minimum Required Fields by Module

| Module | Mandatory Fields | Optional Fields |
| :--- | :--- | :--- |
| **Events** | Title, Date/Time, Location, Short Summary | Featured Image, Extended Description, Registration Link |
| **News** | Title, Publication Date, Body Content | Cover Image, Author Bio |
| **Service Projects** | Title, Category/Period, Description | Impact Metrics, Photo Gallery |
| **Contact Enquiries** | Sender Name, Email, Subject, Message | Phone Number |

> **Enquiry Fallback Policy:** Enquiries are saved to the administration database first. If SMTP notifications fail or are unconfigured, the enquiry is safely logged in the system dashboard without displaying an error to the sender.

---

## 2. Target Directory Structure

```text
rotaract-website/
├── public/                  # Publicly accessible web root
│   ├── index.php            # Front Controller
│   └── assets/              # Static assets
│       ├── css/
│       ├── js/
│       └── images/
├── app/                     # Core application logic
│   ├── Controllers/
│   ├── Models/
│   ├── Repositories/
│   ├── Services/
│   ├── Views/
│   └── Helpers/
├── config/                  # Configuration files
├── database/                # Schema migrations & seeds
│   ├── migrations/
│   └── seeds/
├── tests/                   # Test suite
├── docs/                    # Documentation & ADRs
├── .env.example             # Template for environment configuration
├── .gitignore               # Git exclusion list
└── README.md                # Project overview
```

---

## 3. Decision Review Triggers

Re-evaluate these architectural decisions if any of the following conditions occur:

* 🚩 **Multi-Tenancy Expansion:** The platform is expanded to host multiple independent clubs natively.
* 🚩 **Authentication Scope:** Member account registration becomes a mandatory requirement.
* 🚩 **Team Scaling:** Development expands to a multi-developer team requiring continuous integration pipelines.
* 🚩 **API Requirements:** External integrations require exposing a public REST or GraphQL API.
* 🚩 **Hosting Constraints:** Deployment provider rules prevent configuring `public/` as the web root.
```

The document has been formatted into clear Markdown with tables, code structures, and status badges to make it easy to read and navigate.
