# Requirements Traceability Matrix (RTM)

## Document Control & Metadata

| Attribute | Details |
| :--- | :--- |
| **Project Name** | Rotaract Club of UNZA Website |
| **Document ID** | RTM-001 |
| **Version** | 1.1 |
| **Status** | Initial baseline v1.1 --- recommendations added; validate before implementation |
| **Last Updated** | 18 October 2026 |

---

## 1. Purpose

This matrix links functional and non-functional requirements to their underlying stakeholder needs, planned implementation components, and corresponding validation/testing evidence. It ensures end-to-end traceability across the project lifecycle so that every requirement is implemented, verified, and justified.

---

## 2. Requirements Traceability Matrix

### 2.1 Functional Requirements (FR)

| Stakeholder Need / Source | Req ID | Requirement Summary | Priority | Planned Component | Planned Validation Evidence | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Professional public presence** | `FR-001` | Homepage with club identity and featured content | **Must** | Public homepage / views | `TC-PUB-001`: Homepage content and navigation check | Not started |
| **Visitors need club information** | `FR-002` | About the Club page | **Must** | Public pages / views | `TC-PUB-002`: Page availability and content check | Not started |
| **Visitors need event information** | `FR-003` | List published events | **Must** | Events module | `TC-EVT-001`: Listing, ordering, empty state, and unpublished check | Not started |
| **Club needs to maintain events** | `FR-004` | Create / edit / publish / unpublish / delete events | **Must** | Event controller, service, repository, and admin views | `TC-EVT-002`: CRUD & validation;<br>`TC-SEC-001`: Authorization | Not started |
| **Public needs announcements** | `FR-005` | Display published news articles | **Must** | News module | `TC-NEWS-001`: Published vs. unpublished visibility check | Not started |
| **Club needs to publish news** | `FR-006` | Manage news articles | **Must** | News management controller & admin views | `TC-NEWS-002`: Create, edit, publish, delete, and validation | Not started |
| **Public needs service impact** | `FR-007` | Display service projects and impact reports | **Must** | Projects module | `TC-PRJ-001`: Project listing and detail page check | Not started |
| **Club needs project records** | `FR-008` | Manage project content | **Must** | Project management module | `TC-PRJ-002`: Create, edit, publish, and authorization checks | Not started |
| **Visitors need to contact club** | `FR-009` | Public contact form | **Must** | Contact page & request handler | `TC-CON-001`: Valid/invalid input and basic spam protection | Not started |
| **Club needs to review enquiries** | `FR-010` | Store and protect submitted enquiries | **Must** | Enquiry persistence & admin module | `TC-CON-002`: Data persistence, access restriction, and error handling | Not started |
| **Admins need protected access** | `FR-011` | Admin login and logout | **Must** | Authentication service & login views | `TC-AUTH-001`: Valid/invalid login & session invalidation | Not started |
| **Restricted access required** | `FR-012` | Server-side authorization | **Must** | Authorization middleware / helpers | `TC-SEC-001`: Unauthenticated and unauthorized access tests | Not started |
| **Admins need management UI** | `FR-013` | Administrative dashboard | **Must** | Admin layout & dashboard view | `TC-ADM-001`: Dashboard access and role-based navigation | Not started |
| **No code edits for content** | `FR-014` | Dashboard content management | **Must** | Admin forms & content modules | `TC-ADM-002`: Update content via UI and verify public change | Not started |
| **Club details maintenance** | `FR-015` | Update agreed club details | **Must** | Settings / configuration module | `TC-SET-001`: Update approved fields and verify frontend display | Not started |
| **Admin user administration** | `FR-016` | Create / disable / reset admin accounts | **Should** | Account administration UI / service | `TC-ADM-003`: Role restriction and disabled-account tests | Pending decision |
| **Future reuse for other clubs** | `FR-017` | Keep club identity configurable | **Should** | Configuration / environment files | `TC-ARCH-001`: Re-skin/re-configure test without core code edits | Not started |
| **Member portal deferred** | `FR-018` | No member accounts in release one | **Must** | Scope / route design | `TC-SCOPE-001`: Confirm absence of public user registration | Confirmed scope |

---

### 2.2 Non-Functional Requirements (NFR)

| Category / Focus | Req ID | Requirement Summary | Priority | Planned Component | Planned Validation Evidence | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Responsive UI** | `NFR-001` | Usable at representative viewport widths | **Must** | CSS / Layout components | `TC-UI-001`: Viewport checks at 360px, 768px, and 1280px | Not started |
| **Usability** | `NFR-002` | Consistent navigation and validation feedback | **Must** | Shared views & form components | `TC-UX-001`: Navigation and form usability review | Not started |
| **SQL Safety** | `NFR-003` | PDO prepared statements for untrusted values | **Must** | Data access layer (Repository/PDO) | `TC-SEC-002`: Source code review & SQL injection penetration tests | Not started |
| **Password Storage** | `NFR-004` | Password hashing and verification (`password_hash`) | **Must** | Authentication service | `TC-SEC-003`: Database password hash inspection & verification test | Not started |
| **Input Handling** | `NFR-005` | Strict server-side validation | **Must** | Request handlers / services | `TC-VAL-001`: Boundary value, missing input, and malformed input tests | Not started |
| **CSRF Defense** | `NFR-006` | CSRF protection on state-changing requests | **Must** | Middleware / Request handlers | `TC-SEC-004`: Form submission with missing/invalid CSRF token | Not started |
| **XSS Defense** | `NFR-007` | Contextual output escaping & sanitization | **Must** | View rendering engine / templates | `TC-SEC-005`: Stored and reflected XSS payload testing | Not started |
| **Session Security** | `NFR-008` | Secure session handling and protected routes | **Must** | Session manager & server configuration | `TC-SEC-006`: Session fixation, cookie flags, and direct URL tests | Not started |
| **Secret Protection** | `NFR-009` | Secrets excluded from public files & repository | **Must** | Environment setup (`.env` strategy) | `TC-SEC-007`: Repository scan and deployment config audit | Not started |
| **Maintainability** | `NFR-010` | Separate presentation, logic, and data layers | **Must** | Overall project structure | `TC-ARCH-002`: Code structure review and setup documentation check | Not started |
| **Compatibility** | `NFR-011` | Cross-browser & device testing | **Must** | Complete web application | `TC-COMP-001`: Test matrix against major mobile/desktop browsers | Not started |
| **Accessibility** | `NFR-012` | WCAG 2.2 AA alignment where applicable | **Should** | Front-end templates | `TC-A11Y-001`: Keyboard navigation, form labels, contrast & alt-text | Not started |
| **Performance** | `NFR-013` | Measure public pages and document results | **Should** | Assets & Core Web Vitals | `TC-PERF-001`: Lab measurement (Lighthouse / PageSpeed Insights) | Not started |
| **Data Recovery** | `NFR-014` | Documented backup and restore procedure | **Must** | Operational scripts / DB procedures | `TC-OPS-001`: Database backup generation and test environment restore | Not started |
| **Error Handling** | `NFR-015` | No sensitive technical details leaked in production | **Must** | Exception handlers & loggers | `TC-SEC-008`: Trigger controlled errors and inspect responses/logs | Not started |

---

## 3. Recommended Baseline Decisions to Validate

| Area | Recommended Baseline | Validation Evidence Required |
| :--- | :--- | :--- |
| **Administrator Permissions** | Principle of least privilege; individual accounts; **Site Administrator** manages accounts/settings; **Content Managers** publish updates; **Enquiry Manager** processes contact submissions. | Written confirmation of role assignments; permission authorization test cases (`TC-SEC-001`). |
| **Hosting & Budget** | Managed shared PHP/MySQL hosting; evaluate at least two options; **US$50–100** year-one budget envelope for domain + hosting (excluding optional paid email). | Provider quote comparison spreadsheet evaluating upfront vs. renewal costs, SSL availability, and backup tooling. |
| **Content Fields** | Minimal mandatory input fields for events, news, and projects; optional image and impact metrics; direct publishing without multi-step workflow. | Review of form fields and layout mocks by Secretary and Public Image Committee representatives. |
| **Deletion & Retention** | Soft-delete/archiving preferred over immediate destruction; permanent delete restricted to Site Admin; enquiries stored in DB; 12-month retention for closed enquiries. | Verification of soft-delete functionality in database queries and role-restricted execution of purge actions. |

---

## 4. Verification and Validation Rules

* **Verification:** Confirms that the built implementation matches the exact technical specifications.
* **Validation:** Confirms that the delivered functionality fulfills actual stakeholder operational needs.
* **Coverage Standard:** Every `Must` priority requirement requires at least one documented passing test result prior to release.
* **Defect Handling:** Any failed test execution must be mapped to a recorded defect or a formally signed-off exception.
* **Change Management:** Any requirement revisions following baseline approval must be logged with date, rationale, and impact assessment on schedule, budget, and tests.

---

## 5. Status Definitions

| Status Code | Meaning |
| :--- | :--- |
| **Not started** | Requirement defined; development work has not commenced. |
| **In progress** | Active development or implementation is underway. |
| **Implemented** | Code written and deployed to testing, awaiting formal verification. |
| **Tested** | Required test cases executed and passed with documented evidence. |
| **Accepted** | Formal stakeholder sign-off obtained against acceptance criteria. |
| **Deferred** | Postponed to a future release by explicit decision. |
| **Pending decision** | Implementation blocked awaiting stakeholder policy or architectural choice. |

---

## 6. Document Revision Log

| Version | Date | Description of Changes | Author / Contributor |
| :--- | :--- | :--- | :--- |
| **1.0** | 18 October 2026 | Initial requirements traceability baseline derived from SRS draft. | Development Team |
| **1.1** | 18 October 2026 | Added baseline recommendations for permissions, hosting budget, content schemas, and operational retention. | Senior Editor / Lead Developer |
