# Requirements Traceability Matrix (RTM)

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website |
| **Document ID** | RTM-001 |
| **Version** | 1.2 |
| **Status** | Initial baseline: validate before implementation |
| **Last updated** | 18 October 2026 |
| **Owner** | Project Developer |
| **Related** | [SRS-001](software-requirements-specification.md), [SAD-001](system-architecture-design.md), [ADR-LOG-001](architecture-decisions.md), [STK-001](stakeholder-register.md) |

---

## 1. Purpose

This matrix links each requirement to its stakeholder need, the architecture module that implements it, and the test that proves it. It shows at a glance whether every requirement is built, verified, and justified.

**Component names** are the modules, layers, or shared areas defined in [SAD-001 §6](system-architecture-design.md#6-application-modules): *Public Website, Authentication, Authorization, Events, News, Projects, Contact, Admin, Users, Settings, Media, Logging*, plus the layers *Core*, *Middleware*, and *Shared*.

---

## 2. Traceability Matrix

### 2.1 Functional Requirements

| Need or source | ID | Summary | Priority | Component | Validation | Status |
| :--- | :--- | :--- | :---: | :--- | :--- | :--- |
| Professional public presence | FR-001 | Homepage with identity and highlights | Must | Public Website | TC-PUB-001 | Not started |
| Visitors need club information | FR-002 | About page | Must | Public Website | TC-PUB-002 | Not started |
| Visitors need event information | FR-003 | Display published events | Must | Events | TC-EVT-001 | Not started |
| Club maintains events | FR-004 | Create, edit, publish, unpublish, archive events; permanent delete by Site Administrator | Must | Events, Authorization, Admin | TC-EVT-002, TC-SEC-001 | Not started |
| Public needs announcements | FR-005 | Display published news | Must | News | TC-NEWS-001 | Not started |
| Club publishes news | FR-006 | Manage news articles | Must | News, Admin | TC-NEWS-002 | Not started |
| Public needs service impact | FR-007 | Display projects and impact reports | Must | Projects | TC-PRJ-001 | Not started |
| Club keeps project records | FR-008 | Manage project content and impact | Must | Projects, Admin, Media | TC-PRJ-002 | Not started |
| Visitors contact the club | FR-009 | Public contact form | Must | Contact | TC-CON-001 | Not started |
| Club reviews enquiries | FR-010 | Store and protect enquiries | Must | Contact, Admin | TC-CON-002 | Not started |
| Admins need protected access | FR-011 | Login and logout | Must | Authentication | TC-AUTH-001 | Not started |
| Access must be restricted | FR-012 | Server-side authorization | Must | Authorization, Middleware | TC-SEC-001 | Not started |
| Admins need a management UI | FR-013 | Administrative dashboard | Must | Admin | TC-ADM-001 | Not started |
| No code edits for content | FR-014 | Dashboard content management | Must | Admin (all content modules) | TC-ADM-002 | Not started |
| Club details maintenance | FR-015 | Update club details | Must | Settings | TC-SET-001 | Not started |
| Admin user administration | FR-016 | Create, disable, reset administrator accounts | Should | Users | TC-ADM-003 | Pending decision (OI-02) |
| Future reuse by other clubs | FR-017 | Configurable club identity | Should | Settings, Core (configuration) | TC-ARCH-001 | Not started |
| Member portal deferred | FR-018 | No public accounts in release 1 (scope confirmed, ADR-005) | Must | Public Website, Core (routing) | TC-SCOPE-001 | Not started |

### 2.2 Non-Functional Requirements

| Focus | ID | Summary | Priority | Component | Validation | Status |
| :--- | :--- | :--- | :---: | :--- | :--- | :--- |
| Responsive UI | NFR-001 | Usable from 360 px width upward | Must | Shared (layouts, CSS) | TC-UI-001 | Not started |
| Usability | NFR-002 | Consistent navigation and validation feedback | Must | Shared (layouts, forms) | TC-UX-001 | Not started |
| SQL safety | NFR-003 | PDO prepared statements | Must | Core (database), repositories | TC-SEC-002 | Not started |
| Password storage | NFR-004 | `password_hash()` and `password_verify()` | Must | Authentication | TC-SEC-003 | Not started |
| Input handling | NFR-005 | Server-side validation | Must | Shared (validation), services | TC-VAL-001 | Not started |
| CSRF defence | NFR-006 | CSRF tokens on state-changing requests | Must | Middleware | TC-SEC-004 | Not started |
| XSS defence | NFR-007 | Output escaping and sanitization | Must | Shared (view helpers) | TC-SEC-005 | Not started |
| Session security | NFR-008 | Secure session handling | Must | Authentication, Middleware | TC-SEC-006 | Not started |
| Secret protection | NFR-009 | Secrets outside web root and Git | Must | Core (configuration) | TC-SEC-007 | Not started |
| Maintainability | NFR-010 | Separate views, logic, and data layers | Must | Whole application | TC-ARCH-002 | Not started |
| Compatibility | NFR-011 | Cross-browser support | Must | Whole application | TC-COMP-001 | Not started |
| Accessibility | NFR-012 | WCAG 2.2 AA target | Should | Shared (layouts) | TC-A11Y-001 | Not started |
| Performance | NFR-013 | LCP target of 2.5 s or less, measured | Should | Shared (assets) | TC-PERF-001 | Not started |
| Data recovery | NFR-014 | Documented backup and tested restore | Must | Operations procedures | TC-OPS-001 | Not started |
| Error handling | NFR-015 | No sensitive details leaked | Must | Core (error handler), Logging | TC-SEC-008 | Not started |

---

## 3. Coverage Summary and Gaps

### 3.1 Coverage

| Type | Total | Must | Should | With a test case |
| :--- | :---: | :---: | :---: | :---: |
| Functional | 18 | 16 | 2 | 18 |
| Non-functional | 15 | 13 | 2 | 15 |
| **Total** | **33** | **29** | **4** | **33** |

### 3.2 Gaps Found

Candidate requirements `CR-01` to `CR-04` ([SRS-001 §9](software-requirements-specification.md#9-candidate-requirements)) describe controls in the architecture that have no baselined requirement or test yet.

| ID | Candidate | Proposed test | Status |
| :--- | :--- | :--- | :--- |
| CR-01 | Image upload controls | TC-SEC-009 *(proposed)*: upload forbidden types, oversize files, executable content | Awaiting decision |
| CR-02 | Login throttling | TC-SEC-010 *(proposed)*: repeated failed logins | Awaiting decision |
| CR-03 | Audit log | TC-ADM-004 *(proposed)*: log entries for role change, account creation, permanent delete | Awaiting decision |
| CR-04 | Enquiry retention enforcement | TC-CON-003 *(proposed)*: purge or anonymize after retention period | Depends on OI-10 |

---

## 4. Test Case Index

| Test case | Title | Covers |
| :--- | :--- | :--- |
| TC-PUB-001 | Homepage content and navigation | FR-001 |
| TC-PUB-002 | About page availability and content | FR-002 |
| TC-EVT-001 | Event listing, ordering, empty state, unpublished check | FR-003 |
| TC-EVT-002 | Event CRUD and validation | FR-004 |
| TC-NEWS-001 | Published versus unpublished news visibility | FR-005 |
| TC-NEWS-002 | News create, edit, publish, archive, validation | FR-006 |
| TC-PRJ-001 | Project listing and detail pages | FR-007 |
| TC-PRJ-002 | Project create, edit, publish, authorization | FR-008 |
| TC-CON-001 | Contact form valid and invalid input; spam protection | FR-009 |
| TC-CON-002 | Enquiry persistence, access restriction, error handling | FR-010 |
| TC-AUTH-001 | Valid and invalid login; session invalidation | FR-011 |
| TC-SEC-001 | Unauthenticated and unauthorized access (every role) | FR-004, FR-012 |
| TC-ADM-001 | Dashboard access and role-based navigation | FR-013 |
| TC-ADM-002 | Update content through the UI and verify the public change | FR-014 |
| TC-SET-001 | Update club details and verify public display | FR-015 |
| TC-ADM-003 | Account role restrictions and disabled-account behaviour | FR-016 |
| TC-ARCH-001 | Re-configure club identity without core code edits | FR-017 |
| TC-SCOPE-001 | Confirm no public registration or login prompts | FR-018 |
| TC-UI-001 | Viewport checks at 360, 768, 1280 px | NFR-001 |
| TC-UX-001 | Navigation and form usability review | NFR-002 |
| TC-SEC-002 | Code review and SQL injection tests | NFR-003 |
| TC-SEC-003 | Password hash inspection and verification | NFR-004 |
| TC-VAL-001 | Boundary, missing, and malformed input | NFR-005 |
| TC-SEC-004 | Missing or invalid CSRF token | NFR-006 |
| TC-SEC-005 | Stored and reflected XSS payloads | NFR-007 |
| TC-SEC-006 | Session fixation, cookie flags, direct URL access | NFR-008 |
| TC-SEC-007 | Repository scan and deployment configuration audit | NFR-009 |
| TC-ARCH-002 | Code structure review and setup documentation check | NFR-010 |
| TC-COMP-001 | Browser and device matrix | NFR-011 |
| TC-A11Y-001 | Keyboard navigation, labels, contrast, alt text | NFR-012 |
| TC-PERF-001 | Lighthouse or PageSpeed measurement | NFR-013 |
| TC-OPS-001 | Backup generation and test restore | NFR-014 |
| TC-SEC-008 | Controlled errors; inspect responses and logs | NFR-015 |

---

## 5. Baseline Decisions to Validate

| Area | Recommended baseline | Evidence required | Tracked as |
| :--- | :--- | :--- | :--- |
| Administrator permissions | Least privilege; individual accounts; Site Administrator manages accounts and settings; Content Managers publish; Enquiry Manager handles enquiries (matrix in SRS-001 §3.2) | Written role assignments; TC-SEC-001 | OI-01, OI-02, ADR-004 |
| Hosting and budget | Managed shared PHP/MySQL hosting; compare at least two providers; US$50–100 year-one envelope | Quote comparison covering upfront and renewal cost, SSL, and backup tooling | OI-06, ADR-011 |
| Content fields | Minimal mandatory fields; optional images and impact metrics; direct publishing | Field and layout review by Secretary and Public Image Committee | OI-05, SRS-001 §5 |
| Deletion and retention | Archive by default; permanent delete by Site Administrator only; 12-month retention after enquiry closure | Soft-delete behaviour verified in queries; purge restricted by role | OI-04, OI-10, ADR-012 |

---

## 6. Verification and Validation Rules

* **Verification:** confirms the build matches the technical specification.
* **Validation:** confirms the delivered functionality meets real stakeholder needs.
* **Coverage standard:** every **Must** requirement needs at least one documented passing test before release.
* **Defect handling:** every failed test maps to a recorded defect or a signed-off exception.
* **Change management:** any change after baseline approval is logged with date, rationale, and impact on schedule, budget, and tests.

## 7. Status Definitions

| Status | Meaning |
| :--- | :--- |
| **Not started** | Defined; development has not begun |
| **In progress** | Development is under way |
| **Implemented** | Built and deployed to a test environment; awaiting verification |
| **Tested** | Required tests executed and passed with documented evidence |
| **Accepted** | Stakeholder sign-off against acceptance criteria obtained |
| **Deferred** | Postponed to a later release by explicit decision |
| **Pending decision** | Blocked until a stakeholder or architecture decision is made |

## 8. Revision Log

| Version | Date | Description | Author |
| :--- | :--- | :--- | :--- |
| 1.0 | 18 October 2026 | Initial traceability baseline derived from the SRS draft | Development Team |
| 1.1 | 18 October 2026 | Added baseline recommendations for permissions, hosting, content fields, and retention | Senior Editor, Lead Developer |
| 1.2 | 18 October 2026 | Consolidation: components renamed to SAD modules; added coverage summary, gap analysis, and test case index; replaced undefined status "Confirmed scope"; linked decisions to `OI` and `ADR` IDs | Project Developer |
