# Rotaract Club of UNZA Website: Documentation Set

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website |
| **Document ID** | IDX-001 |
| **Version** | 1.0 |
| **Status** | Draft: consolidation release |
| **Last updated** | 18 October 2026 |
| **Owner** | Project Developer |

This index explains how the project documents fit together, which document is the master for each topic, and the conventions every document follows.

---

## 1. Document Map

| ID | Document | File | Answers the question |
| :--- | :--- | :--- | :--- |
| **CHR-001** | Project Charter | [project-charter.md](project-charter.md) | Why are we doing this, and within what limits? |
| **STK-001** | Stakeholder Register | [stakeholder-register.md](stakeholder-register.md) | Who is involved, and what do we need from them? |
| **SRS-001** | Software Requirements Specification | [software-requirements-specification.md](software-requirements-specification.md) | What must the system do and how well? |
| **RTM-001** | Requirements Traceability Matrix | [requirements-traceability-matrix.md](requirements-traceability-matrix.md) | Is every requirement built and tested? |
| **ADR-LOG-001** | Architecture Decision Records | [architecture-decisions.md](architecture-decisions.md) | Which technical decisions were made, and why? |
| **SAD-001** | System Architecture Design | [system-architecture-design.md](system-architecture-design.md) | How is the system structured? |

## 2. Suggested Reading Order

1. **CHR-001**: purpose, scope, constraints.
2. **STK-001**: people and decisions needed from them.
3. **SRS-001**: requirements (master for scope, roles, data fields, open issues).
4. **ADR-LOG-001**, then **SAD-001**: how the requirements will be met.
5. **RTM-001**: how each requirement will be verified.

## 3. Single Sources of Truth

Each fact lives in one master location. Other documents summarize it briefly and link to it rather than restating it.

| Topic | Master location |
| :--- | :--- |
| Release scope (in / out) | SRS-001 §2 |
| Roles and permissions | SRS-001 §3 |
| Functional requirements | SRS-001 §4 |
| Data fields and content states | SRS-001 §5 |
| Non-functional requirements | SRS-001 §6 |
| Hosting and budget | SRS-001 §7 |
| Open issues (`OI-nn`) | SRS-001 §8 |
| Candidate requirements (`CR-nn`) | SRS-001 §9 |
| Technical decisions (`ADR-nnn`) | ADR-LOG-001 |
| Modules, source-code structure, security controls | SAD-001 §6, §13, §11 |
| Test cases (`TC-*`) and requirement status | RTM-001 |
| Stakeholders (`ST-nn`) | STK-001 |

## 4. Conventions

### 4.1 Identifiers

| Prefix | Meaning | Defined in |
| :--- | :--- | :--- |
| `FR-nnn` / `NFR-nnn` | Functional / non-functional requirement | SRS-001 |
| `CR-nn` | Candidate requirement (not yet baselined) | SRS-001 §9 |
| `OI-nn` | Open issue or pending decision | SRS-001 §8 |
| `ADR-nnn` | Architecture decision | ADR-LOG-001 |
| `ST-nn` | Stakeholder | STK-001 |
| `R-nn` | Architecture risk | SAD-001 §15 |
| `TC-AREA-nnn` | Test case | RTM-001 §4 |

### 4.2 Priority scale (MoSCoW)

**Must**: essential for release 1. **Should**: high value, included if the schedule allows. **Could**: optional, deferrable.

### 4.3 Status vocabularies

| Used for | Values |
| :--- | :--- |
| Document status | Draft → Draft baseline (pending validation) → Baselined |
| Open issues and ADRs | **Open** (no proposal yet) → **Proposed** (recommendation awaiting confirmation) → **Accepted** (confirmed). ADRs may also be **Superseded** or **Merged**. |
| Requirement progress (RTM) | Not started → In progress → Implemented → Tested → Accepted; also Deferred, Pending decision |
| Sign-off | Pending → Approved |
| Content items in the system | **Draft**, **Published**, **Archived** (see SRS-001 §5) |

### 4.4 Canonical role names

**Site Administrator**, **Content Manager**, **Enquiry Manager** (application roles), and **Technical Maintainer** (operational role with deployment and server access, not an application role). A person may hold more than one application role.

### 4.5 Writing style

Oxford English spelling (for example, *colour*, *authorize*), **enquiry** (not *inquiry*), dates as "18 October 2026", and a standard header table and revision log in every document.

---

## 5. What Changed in This Consolidation

| # | Inconsistency found | Resolution | Where |
| :-: | :--- | :--- | :--- |
| 1 | The ADR document ID (`ADR-INDEX-001`) looked like a decision ID | Renamed `ADR-LOG-001` | ADR-LOG-001 |
| 2 | Open questions tracked twice (`OI-nn` in SRS, `OQ-nn` in stakeholder register) | One register: `OI-nn` in SRS §8. Old `OQ` IDs mapped in STK-001 §7 | SRS §8, STK §7 |
| 3 | SRS listed 4 roles; ADR and SAD listed 3; Site Administrator holders and enquiry access differed | 3 application roles plus Technical Maintainer as an operational role; one permission matrix | SRS §3 |
| 4 | Content states differed (publication status / draft-published-archived / draft-published-unpublished-archived) | Three states; *unpublish* means returning a Published item to Draft | SRS §5 |
| 5 | Field lists differed between SRS, ADR-014 and the stakeholder register | SRS §5 is canonical. Enquiries need name, email, subject, message; news publication date is system-recorded; project category is optional | SRS §5 |
| 6 | FR-004 and RTM said "delete" while ADRs prefer soft-delete | Archive is the default; permanent delete is Site Administrator only | SRS §3, FR-004, ADR-012 |
| 7 | NFR table had no priorities; NFR-013 wording differed between SRS and RTM | Priority column added (NFR-012 and NFR-013 are Should); NFR-013 is a target LCP of 2.5 s or less, measured and documented | SRS §6 |
| 8 | Two different source-code structures (ADR vs SAD) | SAD §13 is canonical; ADR-008 shows only the security-relevant layout | SAD §13 |
| 9 | Budget lines (domain + hosting) sum to US$50–100, but the total row also claimed to include ~20% contingency | Shown separately; US$50–100 is the domain-plus-hosting envelope. Confirm under OI-06 | SRS §7 |
| 10 | Charter conflicted with later decisions ("registered club members", open multi-club assumption) | Updated per ADR-003 and ADR-005 | CHR-001 |
| 11 | RTM component names did not match SAD modules | RTM uses SAD module names | RTM §2 |
| 12 | Requirement IDs were interleaved between public and admin tables | IDs unchanged (traceability preserved); requirements grouped by area | SRS §4 |
| 13 | SAD controls with no matching requirement (uploads, login throttling, audit log, enquiry retention) | Recorded as candidate requirements `CR-01` to `CR-04` | SRS §9 |
| 14 | Treasurer owns hosting purchase (OI-06) but was not in the stakeholder register | Added as `ST-11` | STK §2 |
| 15 | RTM status "Confirmed scope" was not a defined status; ADR-013 duplicated ADR-004; ADR-012 used undefined "Pending" | Statuses normalized; ADR-013 merged into ADR-004; ADR-012 set to Proposed | RTM, ADR-LOG |
| 16 | Stray text and broken fences in the ADR file; grid-style tables in SAD | Cleaned and converted to standard Markdown tables | ADR-LOG, SAD |

## 6. Items Needing Confirmation After This Consolidation

These are normalizations I proposed. Please confirm or reverse them:

1. Technical Maintainer treated as an operational role rather than an application role (SRS §3).
2. Club settings editable by Site Administrators only; Content Managers cannot change them (SRS §3.2, OI-01).
3. Enquiry Managers may soft-delete enquiries; permanent purge is Site Administrator or automatic after retention (SRS §3.2, ADR-012).
4. Subject is a required field on the contact form (SRS §5).
5. Budget envelope interpretation (OI-06).

## 7. Related Document Not Refreshed

`work-breakdown-structure.md` (WBS-001) predates this consolidation. When you next update it, align task 5.3 (role list), retention wording in 6.7, and status names with SRS §3 and §5.

## 8. Revision Log

| Version | Date | Description | Author |
| :--- | :--- | :--- | :--- |
| 1.0 | 18 October 2026 | Created index, conventions and consolidation summary | Project Developer |
