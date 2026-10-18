# Project Charter

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website (Release 1 of the Rotaract Club Website Platform) |
| **Document ID** | CHR-001 |
| **Version** | 1.1 |
| **Status** | Draft baseline: pending stakeholder validation |
| **Last updated** | 18 October 2026 |
| **Target launch** | 30 November 2026 |
| **Owner** | Project Developer |
| **Related** | [STK-001](stakeholder-register.md), [SRS-001](software-requirements-specification.md), [ADR-LOG-001](architecture-decisions.md), [SAD-001](system-architecture-design.md) |

---

## 1. Vision

Develop a professional, responsive, and maintainable website that lets a Rotaract club establish an online presence, showcase its community impact, communicate with the public, and manage its content efficiently.

Release 1 serves the **Rotaract Club of UNZA**. The code is built so that other clubs can reuse it later through configuration and a separate deployment.

## 2. Objectives

| # | Objective | How Release 1 delivers it | Requirements |
| :-: | :--- | :--- | :--- |
| 1 | Establish a professional online presence | Homepage, About page, responsive and accessible design | FR-001, FR-002, NFR-001, NFR-012 |
| 2 | Showcase service projects, achievements, and impact | Events, news, and projects with impact reports | FR-003, FR-005, FR-007 |
| 3 | Provide a central system for managing content | Protected administrative dashboard | FR-004, FR-006, FR-008, FR-013 |
| 4 | Support other clubs through a reusable architecture | Club-specific settings separated from core logic; separate deployment per club (see §7) | FR-017, ADR-003 |
| 5 | Let authorized board and Public Image Committee members manage content without editing source code | Form-based content management with role-based access | FR-014, FR-012 |

## 3. Scope

The full scope is defined in [SRS-001 §2](software-requirements-specification.md#2-project-scope). In summary:

| In scope (Release 1) | Out of scope (Release 1) |
| :--- | :--- |
| Public pages: home, about, events, news, service projects, impact reports | Member registration and private accounts |
| Public contact form and enquiry management | Multi-tenant hosting |
| Administrative dashboard with secure login | Online payments, ticketing, donations |
| Server-side validation and baseline security | Real-time chat, native mobile apps |
| Deployment, documentation, backup procedure | Approval workflow, email marketing |

## 4. Target Users

| User group | Release 1 access |
| :--- | :--- |
| Members of the public and community partners | Public pages and contact form |
| Prospective Rotaractors | Public pages and contact form |
| Club members | Public pages only (member accounts are deferred; see [ADR-005](architecture-decisions.md#adr-005-defer-member-accounts)) |
| Authorized board members and Public Image Committee members | Administrative dashboard according to assigned role |

Detailed stakeholder analysis: [STK-001](stakeholder-register.md).

## 5. Technology Stack

| Layer | Choice |
| :--- | :--- |
| Frontend | HTML5, CSS3, JavaScript |
| Backend | PHP 8.x |
| Database | MySQL / MariaDB |
| Database access | PHP PDO with prepared statements |
| Version control | Git and GitHub |
| Hosting | Managed shared PHP/MySQL hosting (see [ADR-011](architecture-decisions.md#adr-011-hosting-and-domain-provider)) |

## 6. Approach and Timeline

**Approach:** iterative development, requirements validation, incremental testing, and continuous documentation ([ADR-010](architecture-decisions.md#adr-010-use-incremental-development-and-traceability)).

**Target launch:** 30 November 2026.

## 7. Resolved Assumption: Multi-Club Support

The original charter left open whether multiple clubs would share one application and database or receive separate installations. This is now decided by [ADR-003](architecture-decisions.md#adr-003-build-for-one-club-first-with-reusable-core-code):

- Release 1 is a **single-club** application with no tenant identifiers in the database.
- Reuse for another club means **configuration plus a separate deployment**.
- A shared multi-club platform is deferred and would trigger a review of the architecture decisions.

## 8. Constraints

| Constraint | Detail |
| :--- | :--- |
| Team | One software engineering student develops, tests, and deploys the system |
| Stakeholders | Limited availability; reviews are short, time-boxed, and use consolidated documents |
| Budget | Hosting and domain budget pending approval; planning envelope US$50–100 for year one (see [SRS-001 §7](software-requirements-specification.md#7-hosting-and-budget)) |
| Schedule | Fixed launch date of 30 November 2026 |

## 9. Success Criteria

1. The site is live on a club-owned domain by 30 November 2026.
2. Every **Must** requirement has at least one documented passing test ([RTM-001 §6](requirements-traceability-matrix.md#6-verification-and-validation-rules)).
3. Authorized users can publish and update events, news, and projects without editing source code.
4. A backup has been taken and successfully restored in a test environment.
5. A post-launch maintainer and operational owner are named (OI-08).
6. First-year domain and hosting costs stay within the approved budget.

## 10. High-Level Risks

| Risk | Summary mitigation |
| :--- | :--- |
| Scope grows before launch | Prioritize Must requirements; manage changes through the RTM |
| Late stakeholder decisions or content | Track decisions as open issues (`OI-nn`); use placeholder content in development |
| Security work postponed | Build and test security controls with each module |
| Hosting limits prevent safe file separation | Verify document-root options before purchase |

The full risk register is in [SAD-001 §15](system-architecture-design.md#15-risks-and-mitigations).

## 11. Approval

| Role | Name | Status | Date |
| :--- | :--- | :--- | :--- |
| Club Representative | *To be confirmed* | Pending | n/a |
| Public Image Chair | *To be confirmed* | Pending | n/a |
| Project Developer | *To be completed* | Pending | n/a |

## 12. Revision Log

| Version | Date | Description | Author |
| :--- | :--- | :--- | :--- |
| 1.0 | n/a | Original charter (unversioned) | Project Developer |
| 1.1 | 18 October 2026 | Standardized format; resolved the multi-club assumption (ADR-003); corrected "registered members" to reflect deferred member accounts (ADR-005); added scope summary, constraints, success criteria, and risk summary derived from SRS and SAD | Project Developer |
