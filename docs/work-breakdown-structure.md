# Work Breakdown Structure (WBS)

**Project:** Rotaract Club of UNZA Website  |  **Document ID:** WBS-001  |  **Version:** 1.0 (draft baseline)  |  **Date:** 18 October 2026  |  **Target launch:** 30 November 2026

**Derived from:** SRS-001 v1.1, RTM-001 v1.1, Project Charter

## 1. Purpose and Basis

This WBS decomposes the first release of the Rotaract Club of UNZA website into manageable work packages. Every package has a number, a deliverable, the requirements it traces to (FR/NFR IDs from the SRS and RTM), and an effort estimate. It is the basis for scheduling, progress tracking, and change control.

The WBS follows the 100% rule: it covers all work needed to deliver the SRS scope (including project management, decisions, hosting, documentation, and handover) and nothing outside it. Items listed as out of scope in SRS section 2.1 (member accounts, multi-tenant hosting, payments, chat, native apps, approval workflows, email marketing) are deliberately excluded.

## 2. Assumptions and Constraints

1. One software engineering student performs all development, testing, and deployment (Charter, Key Constraint). Stakeholder time is limited, so stakeholder tasks are short, scheduled, and batched.
2. Release 1 serves a single club on one deployment. The code separates club-specific configuration from core logic so another club can reuse it by configuration and a separate deployment (SRS 1.2, 2.2). This resolves, for Release 1 only, the Charter's open question on shared versus separate installations.
3. Stack: HTML, CSS, JavaScript, PHP 8.x, MySQL/MariaDB, PDO, Git/GitHub, on managed shared hosting (US$50–100 year-one budget).
4. Roughly 6 weeks remain (19 Oct – 29 Nov), with launch on Monday 30 November 2026.
5. Effort figures are planning estimates in hours for the single developer, excluding stakeholder time and excluding contingency (see section 8).
6. Priorities follow the SRS: all Must items are in the baseline; FR-016, FR-017, NFR-012 (as audited) and NFR-013 are Should and are the first descoping levers.

## 3. WBS Overview (Level 1 and 2)

| WBS | Work Package Group | Est. Hours |
| :--- | :--- | :--- |
| 1 | Project Management | 24 |
| 2 | Requirements Validation and Decisions | 14 |
| 3 | Architecture and Design | 22 |
| 4 | Environment and Project Foundation | 14 |
| 5 | Core Security and Authentication | 24 |
| 6 | Administrative Dashboard and Content Management | 46 |
| 7 | Public Website | 30 |
| 8 | Content, Branding and Consent | 12 |
| 9 | Testing and Quality Assurance | 30 |
| 10 | Hosting, Deployment and Operations | 18 |
| 11 | Documentation and Training | 14 |
| 12 | Launch, Handover and Closure | 8 |
| | **Total** | **256** |

```
0 Rotaract Club of UNZA Website (Release 1)
├── 1 Project Management
├── 2 Requirements Validation and Decisions
├── 3 Architecture and Design
├── 4 Environment and Project Foundation
├── 5 Core Security and Authentication
├── 6 Administrative Dashboard and Content Management
├── 7 Public Website
├── 8 Content, Branding and Consent
├── 9 Testing and Quality Assurance
├── 10 Hosting, Deployment and Operations
├── 11 Documentation and Training
└── 12 Launch, Handover and Closure
```

## 4. Detailed WBS

### 1 Project Management (24 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 1.1 | Charter and baseline review | Confirmed scope baseline; reconciled Charter versus SRS wording | Charter, SRS 1–2 | 3 |
| 1.2 | Project plan and schedule | Schedule with milestones, task board (e.g. GitHub Projects) | This WBS | 4 |
| 1.3 | Stakeholder communication and review sessions | Weekly status note; scheduled review slots with Board and Public Image Committee | SRS 8 | 8 |
| 1.4 | Progress tracking and change control | Updated RTM statuses; change log with date, rationale, and impact | RTM 4 | 6 |
| 1.5 | Risk and issue management | Maintained risk register and open-issues tracker | SRS 7 | 3 |

### 2 Requirements Validation and Decisions (14 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 2.1 | Close open issues OI-01 to OI-10 | Decision record: role holders (OI-01), admin account approach (OI-02), SMTP need (OI-03), retention/soft-delete (OI-04, OI-10), hosting (OI-06), maintainer (OI-08), branding config approach (OI-09) | SRS 7 | 6 |
| 2.2 | Confirm deployment model | Written decision: single-club deployment, config-driven reuse | Charter, FR-017 | 2 |
| 2.3 | Validate SRS and RTM with stakeholders | Signed-off SRS v1.x baseline (Club Representative, Public Image Chair, Lead Developer) | SRS 8 | 4 |
| 2.4 | Confirm content field sets | Approved field list for events, news, projects, settings, enquiries | OI-05, SRS 4.3 | 2 |

### 3 Architecture and Design (22 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 3.1 | System architecture | Layered design (views, logic, data access), directory layout, routing approach, configuration strategy | NFR-010, FR-017 | 5 |
| 3.2 | Database design | ERD and schema for users, roles, events, news, projects, project media, settings, enquiries; soft-delete and status fields | SRS 4.3, OI-04 | 5 |
| 3.3 | Security design | Role-permission matrix, session policy, CSRF approach, validation and escaping rules, upload rules, error-logging policy | NFR-003 to 009, NFR-015, FR-012 | 3 |
| 3.4 | Information architecture and wireframes | Sitemap, public and admin wireframes, empty and error states | FR-001 to 013 | 5 |
| 3.5 | Visual design and UI kit | Responsive layout rules (360/768/1280 px), typography, colour tokens, accessible form and component styles | NFR-001, 002, 012 | 4 |

### 4 Environment and Project Foundation (14 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 4.1 | Repository and branching | GitHub repo, branching rules, `.gitignore` excluding secrets | NFR-009 | 2 |
| 4.2 | Local development environment | Working PHP/MySQL stack matching production versions | Tech preference | 2 |
| 4.3 | Project skeleton | Directory structure, front controller/router, autoloading, base layout inheritance | NFR-010 | 4 |
| 4.4 | Configuration and secrets loader | `.env`/config loader outside web root; club identity config file | NFR-009, FR-017 | 2 |
| 4.5 | Database scripts | Schema migration scripts, seed data (roles, first admin), PDO connection class | NFR-003 | 2 |
| 4.6 | Logging and error handling | Central exception handler; non-revealing user errors; secure server logs | NFR-015 | 2 |

### 5 Core Security and Authentication (24 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 5.1 | Session manager | Secure cookie flags, ID regeneration after login, inactivity expiry | NFR-008 | 4 |
| 5.2 | Authentication service | Login and logout, `password_hash()`/`password_verify()`, generic failure messages, basic login throttling | FR-011, NFR-004 | 6 |
| 5.3 | Authorization (RBAC) | Role checks middleware applied to every protected route; Site Administrator, Content Manager, Enquiry Manager, Technical Maintainer | FR-012, SRS 3.1 | 5 |
| 5.4 | CSRF protection | Token generation and validation for all state-changing requests | NFR-006 | 3 |
| 5.5 | Validation framework | Reusable server-side validators and error messaging | NFR-005 | 3 |
| 5.6 | Output escaping and sanitisation | Escaping helpers per context; rich-text sanitiser | NFR-007 | 3 |

### 6 Administrative Dashboard and Content Management (46 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 6.1 | Admin layout and dashboard | Protected layout, role-based navigation, feedback alerts | FR-013 | 6 |
| 6.2 | Event management | Create, edit, publish, unpublish, archive/delete (role-restricted purge); validation | FR-004, FR-014 | 7 |
| 6.3 | News management | Create, edit, publish/unpublish, archive/delete without approval workflow | FR-006, FR-014 | 6 |
| 6.4 | Project and impact management | Project CRUD with impact fields (beneficiaries, outcomes, partners) and publish status | FR-008, FR-014 | 7 |
| 6.5 | Image upload handling | Safe uploads (type, size, rename, storage path), alt text capture, image resizing as needed | FR-004/006/008, NFR-007 | 5 |
| 6.6 | Club settings | Edit club name, contact email, social links, description, logo path, board listing | FR-015, FR-017 | 4 |
| 6.7 | Enquiry management | Protected list and detail view; status New / In Progress / Handled; retention-based delete | FR-010 | 6 |
| 6.8 | Account management *(Should)* | Create, disable, reset passwords for admin users; manual DB fallback if time-boxed | FR-016, OI-02 | 5 |

### 7 Public Website (30 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 7.1 | Public layout, navigation, footer | Responsive shared templates, accessible navigation, social links | FR-001, NFR-001, 012 | 5 |
| 7.2 | Homepage | Identity, introduction, highlights; tolerates missing optional content | FR-001 | 3 |
| 7.3 | About page | Purpose, activities, board listing, social links | FR-002 | 2 |
| 7.4 | Events pages | Listing (upcoming before past), detail view, empty states, hides unpublished | FR-003 | 5 |
| 7.5 | News pages | Listing and detail with published dates | FR-005 | 4 |
| 7.6 | Service projects and impact reports pages | Listing and detail with images and impact indicators | FR-007 | 5 |
| 7.7 | Contact form and handler | Server-side validation, honeypot/rate limiting, confirmation and error messages, DB storage (SMTP if OI-03 requires) | FR-009, FR-010 | 5 |
| 7.8 | Error pages and basics | 404/500 pages, page titles and meta basics, no public login prompts | FR-018, NFR-015 | 1 |

### 8 Content, Branding and Consent (12 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 8.1 | Verify club identity assets | Approved logo, name, colours | FR-001 | 1 |
| 8.2 | Collect launch content | About text, board listing, social links, initial events, news, and project entries | FR-002, 003, 005, 007 | 4 |
| 8.3 | Rights and consent | Permissions for photos, stories, and impact statistics | OI-07 | 3 |
| 8.4 | Enter launch content | Production-ready content loaded via the dashboard (doubles as FR-014 check) | FR-014 | 4 |

### 9 Testing and Quality Assurance (30 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 9.1 | Test plan and cases | Test plan and executable cases for all TC IDs in the RTM | RTM 2, 4 | 4 |
| 9.2 | Functional testing | Results for TC-PUB, EVT, NEWS, PRJ, CON, AUTH, ADM, SET, SCOPE | All FR | 6 |
| 9.3 | Security testing | TC-SEC-001 to 008: authorization bypass, SQLi, hash inspection, CSRF, XSS, session, secrets scan, error leakage | NFR-003 to 009, 015, FR-012 | 7 |
| 9.4 | Responsive and cross-browser testing | TC-UI-001, TC-COMP-001 at 360/768/1280 px on Chrome, Safari, Firefox, Edge | NFR-001, 011 | 4 |
| 9.5 | Accessibility and performance audit | TC-A11Y-001, TC-PERF-001 (Lighthouse/WAVE), documented results and fixes | NFR-012, 013 | 4 |
| 9.6 | User acceptance testing | Stakeholders complete representative tasks; feedback log | NFR-002, SRS 8 | 3 |
| 9.7 | Defect fixing and regression | Defects closed or exceptions signed off; re-test evidence | RTM 4 | 2 |

### 10 Hosting, Deployment and Operations (18 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 10.1 | Hosting and domain procurement | Two-quote comparison; approved provider; purchase by Treasurer | OI-06, SRS 6 | 3 |
| 10.2 | Domain and DNS setup | Domain pointed to host | SRS 6.1 | 2 |
| 10.3 | Production environment configuration | PHP 8.x, PDO MySQL, SSL (Let's Encrypt), secrets outside web root, protected directories | NFR-008, 009 | 4 |
| 10.4 | Deployment procedure | Repeatable SFTP/Git deployment steps; staged test deploy | SRS 6.1 | 3 |
| 10.5 | Backup and restore | Documented backup routine; verified restoration in a test environment | NFR-014, TC-OPS-001 | 4 |
| 10.6 | Production smoke test | Post-deploy checklist (HTTPS, login, forms, uploads, error pages) | NFR-008, 015 | 2 |

### 11 Documentation and Training (14 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 11.1 | Administrator user guide | Step-by-step guide for content managers and enquiry managers | FR-014 | 4 |
| 11.2 | Technical documentation | Setup, architecture overview, configuration, database, code structure | NFR-010 | 4 |
| 11.3 | Operations runbook | Deployment, backup/restore, account recovery, log review | NFR-014 | 2 |
| 11.4 | Training session | Hands-on session for Board and Public Image Committee members | SRS 3 | 3 |
| 11.5 | Reuse guide | How to configure and deploy for another club | FR-017, SRS 2.2 | 1 |

### 12 Launch, Handover and Closure (8 h)

| WBS | Work Package | Deliverable | Trace | Hours |
| :--- | :--- | :--- | :--- | :--- |
| 12.1 | Go-live | Production release on 30 November 2026; launch announcement | Charter | 2 |
| 12.2 | Early-life support | First-week monitoring and quick fixes | NFR-015 | 3 |
| 12.3 | Handover | Named system maintainer and operational owner; credentials and access transferred; developer access minimised | OI-08, SRS 3.1 | 2 |
| 12.4 | Closure | Final acceptance sign-off; RTM updated to Tested/Accepted; lessons learned | RTM 5 | 1 |

## 5. Milestones and Schedule

| Milestone | Target Date | Exit Criteria | Main WBS |
| :--- | :--- | :--- | :--- |
| M1 Baseline agreed | Fri 23 Oct | Open issues decided; SRS validated; deployment model confirmed | 1, 2 |
| M2 Design approved | Fri 30 Oct | Architecture, schema, security design, wireframes reviewed | 3 |
| M3 Foundation and security core | Fri 6 Nov | Skeleton running; login, RBAC, CSRF, validation, escaping working | 4, 5 |
| M4 Admin CRUD complete | Fri 13 Nov | Events, news, projects, settings, enquiries manageable from dashboard | 6 |
| M5 Feature complete / code freeze | Fri 20 Nov | All public pages and contact form done; hosting purchased; launch content collected | 7, 8, 10.1–10.2 |
| M6 Testing complete / UAT passed | Wed 25 Nov | All Must test cases passed; defects closed or exceptions signed | 9 |
| M7 Production ready | Fri 27 Nov | Deployed, backup restore verified, smoke test passed, guides ready | 10, 11 |
| M8 Go-live | Mon 30 Nov | Site live; handover recorded | 12 |

**Indicative weekly flow:** W1 (19–25 Oct) packages 1, 2, 3 start; W2 (26 Oct–1 Nov) 3, 4, 5 start; W3 (2–8 Nov) 5, 6 start; W4 (9–15 Nov) 6, 7 start; W5 (16–22 Nov) 7, 8, 10.1–10.3, 9 start; W6 (23–29 Nov) 9, 10, 11, then 12.

## 6. Dependencies and Critical Path

| Dependency | Why it matters |
| :--- | :--- |
| 2.1 and 2.4 before 3.2 and 6.x | Field sets, retention, and role holders shape the schema and forms |
| 3 before 4 and 5 | Architecture and security design drive the skeleton and middleware |
| 4 and 5 before 6 and 7 | Every admin and form feature depends on routing, sessions, RBAC, CSRF, validation |
| 6.5 before 7.4, 7.6 | Public pages display uploaded images |
| 8.3 before 8.4 and publishing | No photos, stories, or statistics go live without consent |
| 10.1 before 10.3 to 10.6 | Production setup needs the purchased hosting and domain |
| 9.3 before 10.6 and 12.1 | Security fixes must land before production release |

**Critical path:** 2 → 3 → 4 → 5 → 6 → 7 → 9 → 10.4–10.6 → 12.1. Work packages 8, 10.1–10.2, and 11 run in parallel with development and are the main sources of schedule slack, but 8.3 and 10.1 depend on stakeholders and must be requested early.

## 7. Risks Linked to the WBS

| Risk | Affected WBS | Mitigation |
| :--- | :--- | :--- |
| Single developer with a fixed launch date and an estimate of about 256 h over roughly 6 weeks | All | Weekly tracking; apply the descoping levers below; no scope additions without change control |
| Slow stakeholder decisions or reviews | 2, 8, 9.6 | Fixed review slots in week 1; default decisions proposed in the SRS apply if no answer by the deadline |
| Hosting purchase delayed (budget undetermined) | 10 | Request quotes in week 1; develop locally until M5 |
| Missing consent or content | 8 | Launch with placeholder-free minimum content; publish only consented items |
| Security defects found late | 5, 9.3 | Build security first (package 5); test continuously, not only in package 9 |
| No named maintainer after launch | 12.3 | Resolve OI-08 before M7; keep documentation current |
| Shared hosting limits (PHP version, file permissions) | 10.3 | Confirm PHP 8.x, PDO MySQL, and SFTP before purchase |

## 8. Effort Summary and Descoping Levers

Planned effort is 256 hours with no contingency. Over six weeks this is about 43 hours per week, which is tight for a student. Recommended actions:

1. Add a contingency reserve of about 15% (roughly 38 hours) as a management reserve, held outside the work packages.
2. If effort runs over, descope in this order, always recording the change and updating the RTM:
   1. 6.8 Account management → provision admin accounts manually in the database (SRS allows this fallback for FR-016).
   2. 9.5 performance tuning (NFR-013) → measure and document only.
   3. 6.6 settings UI → config file for branding (OI-09), keeping the FR-015 contact and social fields editable.
   4. 11.5 reuse guide → defer to after launch.
3. Do not descope any Must item or any security test (9.3).

## 9. Definition of Done for a Work Package

- Deliverable exists, is committed to Git, and has no secrets in the repository.
- Linked requirement IDs have at least one documented passing test (Must items) before release.
- Server-side validation, CSRF, escaping, and authorization are applied where relevant.
- RTM status is updated (Not started → In progress → Implemented → Tested → Accepted).
- Any deviation is logged under change control.

## 10. Open Items Affecting This WBS

| Item | Impact if unresolved |
| :--- | :--- |
| OI-02 Admin account UI versus manual provisioning | Decides whether 6.8 is built or replaced with a manual procedure |
| OI-03 SMTP notifications | Adds email setup and testing to 7.7 and 10.3 if required |
| OI-04 / OI-10 Retention and soft-delete | Defines delete behaviour in 3.2, 6.2–6.4, 6.7 |
| OI-06 Hosting and domain purchase | Blocks 10.3 to 10.6 |
| OI-07 Consent for photos, stories, statistics | Blocks publishing content in 8.4 |
| OI-08 Maintainer appointment | Blocks 12.3 |
| OI-09 Branding UI versus config file | Sizes 6.6 and 4.4 |

## 11. Revision Log

| Version | Date | Change | Author |
| :--- | :--- | :--- | :--- |
| 1.0 | 18 October 2026 | Initial WBS derived from SRS v1.1, RTM v1.1, and the Project Charter | Development Team |
