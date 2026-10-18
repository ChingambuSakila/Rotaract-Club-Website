#  Stakeholder Register — Rotaract Club Website

| Metadata Field | Project Information |
| :--- | :--- |
| **Project Name** | Rotaract Club of UNZA Website |
| **Document ID** | `STK-001` |
| **Version** | `1.1` |
| **Status** |  Draft for stakeholder confirmation |
| **Last Updated** | 18 October 2026 |
| **Target Launch** |  30 November 2026 |
| **Document Owner**| Project Developer |

---

## 1.  Purpose

This register identifies individuals and key groups who influence, use, maintain, or are affected by the website. It outlines their interests, expected involvement level, and the strategies for eliciting and validating system requirements.

> **Note on Project Scope & Constraints:**  
> The developer is building the initial release independently. Because stakeholder availability is expected to be limited, reviews will be focused, time-boxed, and conducted using consolidated documentation.

---

## 2.  Stakeholder Register

| ID | Stakeholder / Group | Role & Interest | Influence | Engagement Approach | Key Requirements / Decisions Needed |
| :---: | :--- | :--- | :---: | :--- | :--- |
| **ST-01** | **Club President** | Accountable club leader; wants a professional online presence and accurate representation of the club. |  **High** | One focused review of project scope and public content; request decisions in writing where possible. | Confirm purpose, launch scope, public information, and admin delegated authorities. |
| **ST-02** | **Club Board Members** | Operational leadership; needs capability to oversee or update official club information. |  **High** | Short targeted review of role permissions and content responsibilities. | Confirm board roles for managing events, news, projects, club details, and admin accounts. |
| **ST-03** | **Public Image Committee** | Primary content managers for news, photography, announcements, and branding. |  **High** *(Content)* | Request list of content types, required metadata fields, branding assets, and publishing expectations. | Confirm content workflows, image usage rights, required fields, and publishing authorization. |
| **ST-04** | **Club Secretary / Info Owner** | Holds official records of events, leadership details, and historical club data. |  **Med–High** | Request verified event details, leadership rosters, and official club contacts. | Confirm information accuracy, update frequency, and deprecation process for old data. |
| **ST-05** | **Club Members** | General audience browsing public content; future users of member-only features. | 🟡 **Medium** | Optional short questionnaire or informal review. *(No member portal in v1.0)* | Identify general usability feedback and log requests for future member account capabilities. |
| **ST-06** | **Prospective Members** | Visitors seeking to understand club activities and submit membership enquiries. |  **Medium** | Review public landing pages and contact flow with representative test users. | Confirm membership path clarity and requirements for a dedicated enquiry form. |
| **ST-07** | **General Public / Partners** | Seek reliable information on community projects, events, and partnership impact. |  **Medium** | Validate clarity of public-facing project impact and contact information. | Confirm which project outcomes and public contact information are approved for publication. |
| **ST-08** | **Website Administrator(s)** | Maintains website platform, handles content moderation, and manages dashboard settings. |  **High** *(Ops)* | Walkthrough of administration tasks, dashboard layout, and permission boundaries. | Confirm authentication, user account lifecycle, content management workflow, and backups. |
| **ST-09** | **Project Developer** | Elicits requirements, designs, builds, tests, deploys, and documents the website. |  **High** *(Tech)* | Maintains requirements register, decision logs, traceability matrix, and release checklists. | Resolve technical feasibility, security architecture, hosting setup, and schedule risks. |
| **ST-10** | **Hosting & Domain Provider** | Provides infrastructure for the web application, web server, and database. |  **Medium** | Comparative evaluation of cost, PHP/MySQL support, HTTPS SSL, and backup options. | Confirm technical capabilities, domain availability, annual renewal costs, and deployment constraints. |

---

## 3.  Confirmed Project Context

| Aspect | Status / Decision |
| :--- | :--- |
| **Target Organization** | Rotaract Club of UNZA (Code base must be modular for reusability across other clubs). |
| **Multi-Tenancy** | Single-instance web application for Release 1.0 (No multi-tenant architecture). |
| **User Authentication** | Public visitors can view all content. **Member accounts are deferred** to a future release. |
| **Content Management** | Board and Public Image Committee manage content. Direct publishing enabled (no multi-step approval workflow for v1.0). |
| **Resource Allocation** | Solo developer model with constrained stakeholder availability. |
| **Target Launch Date** | **30 November 2026** |
| **Budgetary Constraints** | Hosting and domain budget pending approval. |

---

## 4.  Engagement Plan

| Step | Focus Area | Target Stakeholders | Delivery Method |
| :---: | :--- | :--- | :--- |
| **1** | **Brand Assets & Copy** | Public Image Committee, Secretary | Form request for high-res logos, brand guides, contact details, and initial project stories. |
| **2** | **Scope Alignment** | Club President, Board Representatives | Single consolidated review document covering launch scope and public pages. |
| **3** | **Content Schemas** | Public Image Committee | Brief schema review covering required vs. optional fields for news, events, and projects. |
| **4** | **Security & Roles** | Club Board | Approval of proposed 3-tier administrator role and access control model. |
| **5** | **Issue Management** | Developer / All | Open questions logged centrally in Open Issues Register rather than making silent assumptions. |
| **6** | **SRS Sign-Off** | President, Developer | Written baseline sign-off prior to full feature construction. |

---

## 5.  Recommended Decisions for Review

| Category | Proposal Details | Rationale / Constraint |
| :--- | :--- | :--- |
| **1. Admin Roles & Access** | • **Site Admin:** President + 1 Successor (Global controls, user management).<br>• **Content Manager:** Board/PIC (Events, News, Projects management).<br>• **Enquiry Manager:** Secretary/Designated Contact (Form submissions). | Individual accounts only. No shared credentials or public account registration for administrative areas. |
| **2. Hosting & Infrastructure** | Managed shared PHP/MySQL hosting.<br>• Estimated Budget: **US $50 – $100 / year**.<br>• Requires comparison of at least 2 vendors. | Guarantees affordability, ease of transfer, and verified ownership by official club representatives. |
| **3. Content Requirements** | • **Events:** Title, Date/Time, Location, Short Description.<br>• **News:** Title, Body Content.<br>• **Projects:** Title, Description, Date/Period.<br>• *Images and metrics are optional.* | Keeps data entry simple for content managers. Prefers soft-delete/archive mechanisms over hard deletion. |
| **4. Enquiry Handling** | Log all contact form enquiries to the web dashboard database.<br>• Optional: Send immediate email notification if SMTP is configured.<br>• Data Retention: **12 Months** after issue resolution. | Ensures lead capture even if email delivery fails. Maintains compliance and clean data lifecycle. |

---

## 6.  Open Stakeholder Questions

| ID | Question / Decision Required | Designated Owner | Current Status |
| :---: | :--- | :--- | :---: |
| **OQ-01** | Approve proposed Site Administrator, Content Manager, and Enquiry Manager role boundaries. | President / Board |  Proposed |
| **OQ-02** | Confirm named Site Administrators and specify if full user-management UI is required for launch. | President / Board |  Proposed |
| **OQ-03** | Should contact submissions trigger automated email notifications in addition to dashboard logging? | Board / Contact Person |  Open |
| **OQ-04** | Approve the archive/soft-delete default model vs. permanent record deletion privileges. | Board / Public Image |  Proposed |
| **OQ-05** | Approve recommended mandatory and optional field schemas for events, news, and project posts. | Public Image / Secretary |  Proposed |
| **OQ-06** | Select preferred domain name and hosting provider based on upfront and annual renewal pricing. | President / Treasurer |  Open |
| **OQ-07** | Confirm that initial photographs, success stories, and metrics are cleared for public display. | Public Image Committee |  Open |
| **OQ-08** | Designate responsible party/role for site maintenance and updates after initial developer handover. | Board |  Open |

---

## 7.  Approvals & Sign-Off

> *Note: Signing below confirms review and acceptance of the Stakeholder Register parameters and engagement approach.*

| Approval Role | Nominated Stakeholder | Decision / Status | Date |
| :--- | :--- | :---: | :---: |
| **Club Representative** | *To be confirmed* |  Pending | — |
| **Public Image Committee Rep** | *To be confirmed* |  Pending | — |
| **Project Developer** | *To be completed* |  Pending | — |
