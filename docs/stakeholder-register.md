# Stakeholder Register

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website |
| **Document ID** | STK-001 |
| **Version** | 1.2 |
| **Status** | Draft: for stakeholder confirmation |
| **Last updated** | 18 October 2026 |
| **Target launch** | 30 November 2026 |
| **Owner** | Project Developer |
| **Related** | [CHR-001](project-charter.md), [SRS-001](software-requirements-specification.md), [ADR-LOG-001](architecture-decisions.md) |

---

## 1. Purpose

This register identifies the people and groups who influence, use, maintain, or are affected by the website. It records their interests, how they will be engaged, and the decisions needed from each.

> **Working model:** The developer builds Release 1 alone and stakeholder availability is limited. Reviews are therefore focused, time-boxed, and based on consolidated documents.

## 2. Stakeholder Register

| ID | Stakeholder | Role and interest | Influence | Engagement approach | Decisions needed |
| :---: | :--- | :--- | :---: | :--- | :--- |
| **ST-01** | Club President | Accountable club leader; wants a professional, accurate online presence | High | One focused review of scope and public content; written decisions where possible | Scope, public information, administrator delegation (OI-01, OI-09) |
| **ST-02** | Club Board Members | Operational leadership; oversee or update official club information | High | Short targeted review of role permissions and content responsibilities | Who manages events, news, projects, club details, accounts (OI-01, OI-02) |
| **ST-03** | Public Image Committee | Primary content managers for news, photos, announcements, branding | High (content) | Request content types, field lists, branding assets, publishing expectations | Required fields, image rights, publishing authority (OI-04, OI-05, OI-07) |
| **ST-04** | Club Secretary / Information Owner | Holds official records of events, leadership, and club history | Medium–High | Request verified event details, leadership roster, official contacts | Information accuracy, update frequency, retirement of old data (OI-05, OI-10) |
| **ST-05** | Club Members | Browse public content; future users of member features | Medium | Optional short questionnaire or informal review (no member portal in Release 1) | Usability feedback; log requests for future member accounts |
| **ST-06** | Prospective Members | Visitors learning about the club and submitting membership enquiries | Medium | Review public pages and the contact flow with representative test users | Clarity of the membership path and the contact form |
| **ST-07** | General Public and Partners | Seek reliable information on projects, events, and partnership impact | Medium | Validate clarity of project impact and contact information | Which outcomes and contact details may be published (OI-07) |
| **ST-08** | Website Administrator(s) | Maintain the platform, moderate content, manage dashboard settings | High (operations) | Walkthrough of administration tasks, dashboard, and permission boundaries | Account lifecycle, content workflow, backups (OI-01, OI-02, OI-08) |
| **ST-09** | Project Developer | Elicits requirements; designs, builds, tests, deploys, documents | High (technical) | Maintains requirements, decision log, traceability matrix, release checklist | Technical feasibility, security design, hosting setup, schedule risks |
| **ST-10** | Hosting and Domain Provider | Provides infrastructure, web server, database | Medium | Compare at least two providers on cost, PHP/MySQL support, HTTPS, backups | Capabilities, domain availability, renewal cost, deployment limits (OI-06) |
| **ST-11** | Club Treasurer *(added)* | Approves spending and owns payment of hosting and domain | Medium | Review provider quotes and renewal costs | Budget approval and account ownership (OI-06) |

## 3. Stakeholders and System Roles

Stakeholder groups map to the system roles defined in [SRS-001 §3](software-requirements-specification.md#3-stakeholders-and-access-model). Final assignments are confirmed under OI-01.

| Stakeholder | Proposed system role |
| :--- | :--- |
| President and one designated successor or delegate | Site Administrator |
| Board members and Public Image Committee members | Content Manager |
| Secretary or designated contact person | Enquiry Manager (may also hold another role) |
| Project Developer or technical delegate | Technical Maintainer (time-limited, deployment access only) |

## 4. Confirmed Project Context

| Aspect | Status or decision | Source |
| :--- | :--- | :--- |
| Target organization | Rotaract Club of UNZA; code kept modular for reuse by other clubs | ADR-003 |
| Multi-tenancy | Not in Release 1; single-club deployment | ADR-003 |
| Authentication | Public content needs no login; member accounts deferred | ADR-005 |
| Publishing | Board and Public Image Committee publish directly; no approval workflow | ADR-006 |
| Resources | Solo developer; limited stakeholder availability | CHR-001 |
| Target launch | 30 November 2026 | CHR-001 |
| Budget | Pending approval; planning envelope US$50–100 for year one | SRS-001 §7 |

## 5. Engagement Plan

| Step | Focus | Stakeholders | Method |
| :---: | :--- | :--- | :--- |
| 1 | Brand assets and copy | Public Image Committee, Secretary | Form request for logos, brand guidance, contact details, initial project stories |
| 2 | Scope alignment | President, Board representatives | One consolidated review of launch scope and public pages |
| 3 | Content fields | Public Image Committee | Short review of required and optional fields (SRS-001 §5) |
| 4 | Roles and security | Board | Approval of the role and permission model (SRS-001 §3) |
| 5 | Issue management | Developer and all | Log open questions in the open-issues register (SRS-001 §8) rather than making silent assumptions |
| 6 | SRS sign-off | President, Developer | Written baseline sign-off before full feature construction |

## 6. Decisions Proposed for Stakeholder Review

Full detail lives in the master documents; this table lists what stakeholders are asked to confirm.

| Area | Proposal in brief | Who decides | Master reference |
| :--- | :--- | :--- | :--- |
| Admin roles and access | Site Administrator (President plus one successor), Content Manager (Board and Public Image Committee), Enquiry Manager (Secretary or designate); individual accounts only | President, Board | SRS-001 §3, ADR-004, OI-01, OI-02 |
| Hosting and budget | Managed shared PHP/MySQL hosting; US$50–100 in year one; compare at least two providers; club-owned accounts | President, Treasurer | SRS-001 §7, ADR-011, OI-06 |
| Content fields | Minimal mandatory fields; images and impact metrics optional | Public Image Committee, Secretary | SRS-001 §5, OI-05 |
| Deletion and retention | Archive by default; permanent deletion by Site Administrator only | Board, Public Image Committee | ADR-012, OI-04 |
| Enquiry handling | Always store in the database; email notification optional; keep 12 months after closure | Board, contact person | ADR-014, ADR-012, OI-03, OI-10 |
| Photo and story consent | Confirm rights before publication | Public Image Committee | OI-07 |
| After-launch ownership | Name a maintainer and an operational owner | Board | OI-08 |

## 7. Open Questions

Open questions are tracked in one place: the **open-issues register** in [SRS-001 §8](software-requirements-specification.md#8-open-issues-and-decisions). The earlier `OQ-nn` identifiers map as follows.

| Former ID | Question (short) | Now tracked as |
| :---: | :--- | :---: |
| OQ-01 | Approve role boundaries | OI-01 |
| OQ-02 | Confirm named administrators and whether an account-management UI is needed at launch | OI-01, OI-02 |
| OQ-03 | Email notifications for contact submissions | OI-03 |
| OQ-04 | Archive-first versus permanent deletion | OI-04 |
| OQ-05 | Mandatory and optional fields | OI-05 |
| OQ-06 | Domain name and hosting provider | OI-06 |
| OQ-07 | Clearance of photos, stories, and metrics | OI-07 |
| OQ-08 | Maintenance after developer handover | OI-08 |

## 8. Approval

*Signing confirms review and acceptance of this register and the engagement approach.*

| Role | Nominated stakeholder | Status | Date |
| :--- | :--- | :--- | :--- |
| Club Representative | *To be confirmed* | Pending | n/a |
| Public Image Committee Representative | *To be confirmed* | Pending | n/a |
| Project Developer | *To be completed* | Pending | n/a |

## 9. Revision Log

| Version | Date | Description | Author |
| :--- | :--- | :--- | :--- |
| 1.1 | 18 October 2026 | Added recommended decisions and engagement plan | Project Developer |
| 1.2 | 18 October 2026 | Consolidation: replaced `OQ-nn` with the single `OI-nn` register; linked decisions to master documents; added ST-11 Treasurer; added stakeholder-to-role mapping; standardized header and approval table | Project Developer |
