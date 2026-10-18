# Software Requirements Specification (SRS)

**Project:** Rotaract Club of UNZA Website\
**Document ID:** SRS-001\
**Version:** 1.1\
**Status:** Draft baseline v1.1 --- recommendations incorporated; pending stakeholder validation\
**Last updated:** 18 October 2026\
**Target launch:** 30 November 2026\
**Technology preference:** HTML, CSS, JavaScript, PHP, MySQL, PDO

## 1. Introduction

### 1.1 Purpose

This document defines the scope, functional requirements, quality
requirements, constraints, and acceptance conditions for the first
release of the Rotaract Club of UNZA website. It is intended to guide
implementation, testing, stakeholder review, and release acceptance.

### 1.2 Product vision

Provide a professional, responsive, maintainable online presence for the
Rotaract Club of UNZA. The website will present club information,
events, news, service projects, and impact reports, and will allow
authorized users to maintain content through an administrative dashboard
without editing source code.

The application code should be reusable for separate club websites in
the future. The first release will serve one club and will not implement
multi-tenant hosting.

### 1.3 Intended users

-   Public visitors and community partners.
-   Prospective Rotaractors.
-   Club members using public pages.
-   Authorized board members and Public Image Committee members.
-   The project developer and future maintainers.

Member registration and private member accounts are outside the
first-release scope.

### 1.4 Definitions

-   **Administrator:** A person with an authorized account to access
    protected management functions.
-   **Content manager:** An authorized user permitted to manage
    specified public content.
-   **Published content:** Content approved for public display through
    its publication status. In the first release, authorized users can
    publish directly.
-   **SRS:** Software Requirements Specification.
-   **MVP:** Minimum Viable Product.
-   **PDO:** PHP Data Objects, used for database access.
-   **Traceability:** Linking stakeholder needs to requirements,
    implementation components, and tests.

## 2. Project Scope

### 2.1 In scope for the first release

1.  Homepage.
2.  About the Club page.
3.  Public events listing and event detail information.
4.  News and announcements listing and article pages.
5.  Service projects and impact reports.
6.  Public contact form.
7.  Secure administrative login and logout.
8.  Administrative dashboard.
9.  Content management for events, news, projects, and relevant club
    information.
10. Server-side validation and appropriate security controls.
11. Responsive layout and basic accessibility checks.
12. Deployment, documentation, and a database backup procedure.

### 2.2 Out of scope for the first release

-   Member registration, member accounts, and member-only pages.
-   Multi-tenant hosting where multiple clubs use the same running
    instance and database with tenant isolation.
-   Online payments, ticket sales, or donations.
-   Real-time chat or messaging.
-   Native mobile applications.
-   Complex analytics or integrations not explicitly approved.
-   Automated content-approval workflow.
-   A full email marketing system.

### 2.3 Future reuse

The codebase should separate club-specific configuration (such as club
name, logo, colours, contact details, and social links) from core
application logic where practical. Reuse for another club may require
configuration and a separate deployment. A shared multi-club platform is
not required.

## 3. Stakeholders and User Classes

The main stakeholder groups are the Club President, Board Members,
Public Image Committee, Secretary or information owner, club members,
prospective members, public visitors, administrator(s), hosting
provider, and project developer. See `stakeholder-register.md` for
interests, engagement methods, and open questions.

### 3.1 Proposed first-release access model

-   **Public visitor:** May view published public content and submit a
    contact enquiry.
-   **Content manager:** May manage permitted events, news, projects,
    and club content after authentication.
-   **Account administrator:** May create, disable, or reset
    administrator accounts only if this permission is explicitly granted
    by the club.
-   **Project developer / technical maintainer:** Has deployment and
    technical access during development; production access should be
    minimized and documented.

### Recommended permission baseline (proposed for approval)

Use individual accounts and least-privilege permissions. The recommended
initial roles are:

| Role | Who should hold it | Recommended permissions |
|---|---|---|
| **Site Administrator** | President plus one named successor (preferably Secretary or another board delegate); keep at least two active administrators | Manage administrator accounts and roles; edit club-wide settings; manage all content; review contact enquiries; configure operational settings available in the application. |
| **Content Manager** | Authorized Board Members and Public Image Committee members | Create, edit, publish, unpublish, and archive events, news, service projects, and impact reports. Edit club pages only if explicitly assigned. No administrator-account management, secrets, or hosting control-panel access. |
| **Enquiry Manager** | Secretary or named contact person; may be combined with another role | View, mark as handled, and delete enquiries according to the retention policy. No administrator-account management by default. |
| **Technical Maintainer** | Developer or future technical support person | Deployment and technical maintenance only as needed. Production access should be time-limited/minimized and handed over to the club. This is not automatically a content or account-management role. |

A person may hold more than one role where necessary, but permissions
must be assigned deliberately. Do not use shared accounts. Do not allow
public administrator registration. The Site Administrator should create
accounts, disable departing office-holders promptly, and arrange password
resets without ever viewing a user's existing password. Server-side
permission checks are mandatory. For launch, a secure manual account
provisioning process is acceptable if building account-management screens
would jeopardize the deadline.

This is the recommended baseline, not formal club approval. The
President/Board should confirm role holders and any exceptions before
production deployment.

## 4. Functional Requirements

Priorities: **Must** = required for the initial release; **Should** =
valuable if schedule permits; **Could** = deferred or optional. All
requirements below are proposed for stakeholder approval unless
explicitly identified as a confirmed project decision.

### 4.1 Public website

**FR-001 --- Homepage (Must)**\
The system shall display a homepage containing the club identity,
introductory information, navigation, and links to relevant content such
as events, news, and service projects.

**Acceptance criteria** - A visitor can open the homepage without
signing in. - The homepage displays the configured club name and logo
when provided. - Main navigation links lead to the appropriate public
sections. - Missing optional featured content does not cause a page
error.

**FR-002 --- About the Club (Must)**\
The system shall display approved information about the club, its
purpose, activities, and relevant contact or social links.

**Acceptance criteria** - Visitors can access the About page from the
main navigation. - The page displays current approved content. - Text
and images render correctly on supported screen sizes.

**FR-003 --- Public events (Must)**\
The system shall display published events with available event details.

**Acceptance criteria** - Each event displays its title, date/time,
location, and description when supplied. - Unpublished events are not
displayed publicly. - Upcoming events are ordered before past events or
otherwise clearly distinguished. - A helpful empty state appears when
there are no relevant events.

**FR-004 --- Event management (Must)**\
The system shall allow authorized content managers to create, view,
edit, publish, unpublish, and delete events, subject to their
permissions.

**Acceptance criteria** - Valid event details can be saved. - Required
fields are validated server-side. - Saved published events appear
publicly. - Unpublished events remain unavailable to public visitors. -
Unauthorized users cannot perform event-management actions.

**FR-005 --- Public news and announcements (Must)**\
The system shall display published news articles and announcements.

**Acceptance criteria** - Visitors can view a list of published articles
and open an individual article. - Unpublished articles are not visible
publicly. - Article titles, publication dates, and content are displayed
where available.

**FR-006 --- News management (Must)**\
The system shall allow authorized content managers to create, edit,
publish, unpublish, and delete news articles.

**Acceptance criteria** - The system validates required article
fields. - An authorized user can publish an article without a separate
approval workflow. - Public pages show updated published content. -
Unauthorized users cannot change article content.

**FR-007 --- Service projects and impact reports (Must)**\
The system shall display published service project information and
associated impact details.

**Acceptance criteria** - Visitors can browse published projects. -
Project pages can display a title, description, date or period,
location, images, and impact measures when supplied. - Missing optional
impact measures do not prevent publication. - Unpublished projects are
not visible publicly.

**FR-008 --- Project management (Must)**\
The system shall allow authorized content managers to create, edit,
publish, unpublish, and delete service project content and impact
reports.

**Acceptance criteria** - Required fields are validated server-side. -
Authorized users can update project content without changing source
code. - Changes to published projects appear on the public site. -
Unauthorized users cannot manage projects.

**FR-009 --- Contact form (Must)**\
The system shall provide a public form through which visitors can submit
enquiries.

**Acceptance criteria** - The form accepts the minimum agreed contact
details and a message. - Required fields and input formats are validated
on the server. - Invalid submissions produce understandable error
messages. - Successful submissions receive a clear confirmation. - Basic
spam protection and safe handling of submitted data are implemented.

**FR-010 --- Enquiry management (Must)**\
The system shall store submitted enquiries and allow authorized users to
review them in a protected area.

**Acceptance criteria** - A valid enquiry is saved to the database. -
Enquiries are not exposed on public pages. - Only authorized users can
access stored enquiries. - The system handles database errors without
exposing sensitive technical details to visitors.

Whether email notifications are required remains an open decision.

### 4.2 Administration and security

**FR-011 --- Administrator authentication (Must)**\
The system shall provide secure login and logout for authorized
administrators.

**Acceptance criteria** - Valid credentials allow access according to
the account's permissions. - Invalid credentials do not grant access. -
Passwords are stored using PHP's password-hashing functions. - Session
handling follows secure configuration practices. - Logout invalidates
the authenticated session.

**FR-012 --- Authorization (Must)**\
The system shall check permissions on the server for every protected
operation.

**Acceptance criteria** - Unauthenticated users cannot access protected
management functions. - Authenticated users cannot perform actions
outside their assigned permissions. - Directly requesting a protected
URL does not bypass authorization. - Access failures do not disclose
sensitive system details.

**FR-013 --- Administrative dashboard (Must)**\
The system shall provide a protected dashboard with navigation to the
management functions permitted for the current user.

**Acceptance criteria** - The dashboard is accessible only to
authenticated, authorized users. - The dashboard provides links to
applicable content-management functions. - It presents clear success,
validation, and error feedback.

**FR-014 --- Content management without code changes (Must)**\
Authorized content managers shall be able to maintain the agreed public
content through the dashboard.

**Acceptance criteria** - The user can create and update supported
content using forms. - Normal content updates do not require editing
PHP, HTML, or SQL files. - Changes are validated and persisted in the
database.

**FR-015 --- Club information management (Must)**\
The system shall allow an appropriately authorized user to update agreed
club information, such as the club description, contact details, logo,
colours, and social links, to the extent included in the implemented
configuration/content manager.

**Acceptance criteria** - Editable fields are defined and documented. -
Invalid values are rejected or handled safely. - Updated information is
reflected on relevant public pages.

The precise editable fields and whether branding is managed in the
dashboard or configuration files must be confirmed before
implementation.

**FR-016 --- Administrator account management (Should; confirm before
implementation)**\
If approved, a specifically authorized account administrator shall be
able to create and disable administrator accounts and initiate secure
password resets. Public self-registration is not included.

**Acceptance criteria** - Only users with the account-management
permission can perform these actions. - Disabled accounts cannot log
in. - Password reset procedures do not reveal whether arbitrary accounts
exist. - Administrative actions are recorded where practical.

This feature should be reduced to a secure, documented manual
provisioning process if the deadline does not permit a safe
implementation.

### 4.3 Reusability

**FR-017 --- Configurable club identity (Should)**\
The application shall keep key club-specific values, such as the club
name, logo path, colours, contact details, and social links, separate
from core application logic where practical.

**Acceptance criteria** - Club-specific values are documented in a
configuration or content mechanism. - Changing these values does not
require rewriting core business logic. - No multi-tenant functionality
is implied by this requirement.

**FR-018 --- No member accounts in release one (Must --- scope
constraint)**\
The first release shall not require member registration or member-only
accounts. The public site shall remain browsable without login;
authentication is required only for protected administrative operations.

### 4.4 Content-management rules and default fields

The following is the recommended first-release content model. Required
fields should be kept deliberately small so that authorized club members
can publish updates quickly; optional fields may be omitted without
blocking publication.

**Events**
- Required: title, start date/time, location (or an explicit “online / to be confirmed” value), short description, publication status.
- Optional: end date/time, cover image, registration/contact link, organizer/contact details, accessibility or attendance notes.
- Past events should remain available as an archive unless an authorized user archives them.

**News and announcements**
- Required: title, body/summary, publication status.
- System-managed: creation/update timestamps; publication timestamp when published.
- Optional: cover image, category, author/byline, related event/project, caption/alternative text.

**Service projects and impact reports**
- Required: project title, description, date or period, publication status.
- Optional but encouraged: location, partner organizations, photographs with captions/alternative text, goals, outputs, outcomes, beneficiary counts, funds/items raised or donated, and a short impact summary.
- Do not force numeric impact measures when they are unavailable or not applicable; clearly distinguish verified figures from estimates.

**Club information**
- Recommended editable fields: club description, current leadership/board listing, public contact email, social-media links, logo, and brand colours. Keep technical settings and secrets outside the content editor. If time is limited, stable branding can be maintained in configuration files for v1.0 and moved into the dashboard later.

**Common management behaviour**
- Every content item should have a draft/unpublished or published state; authorized users may publish directly without a separate approval workflow.
- Prefer archive/soft-delete over immediate permanent deletion for events, news, and projects. Restrict permanent deletion to Site Administrators and retain a clear confirmation step. Confirm whether soft deletion is feasible within the launch schedule.
- Store creator and last-updater references/timestamps where practical. A lightweight audit record for administrator-account changes and content publication/deletion is recommended.
- Validate uploads by file type, size, and image dimensions; generate safe filenames, store uploads in a controlled location, and never trust client-provided file names. Obtain permission for public use of photographs and stories.
- Contact enquiries should include only the minimum necessary sender/contact details and message. Recommended statuses: New, In progress, Handled. Limit access to the Site Administrator and Enquiry Manager roles. Adopt a retention period with the club; a practical starting proposal is to delete or anonymize closed enquiries after 12 months unless a legitimate club need requires longer retention.
- Recommended notification behaviour: store every enquiry in the dashboard and send a notification to the designated club email if reliable SMTP is available. Failure to send email must not lose the saved enquiry. If SMTP is not configured by launch, clearly state that enquiries are dashboard-only and assign someone to check them regularly.

These defaults should be confirmed by the President/Board and Public Image
Committee before database schema and forms are finalized.

## 5. Non-Functional Requirements

**NFR-001 --- Responsive design (Must)**\
Public and administrative pages shall remain usable at representative
mobile, tablet, and desktop viewport sizes. Test at minimum 360 px, 768
px, and 1280 px viewport widths, and fix horizontal overflow or
inaccessible controls.

**NFR-002 --- Usability (Must)**\
The site shall use consistent navigation, readable typography, clear
labels, and understandable validation messages. A small usability review
should verify that representative users can find the Events, Projects,
and Contact sections without assistance.

**NFR-003 --- Database safety (Must)**\
Database operations involving supplied values shall use PDO prepared
statements. SQL shall not be built by concatenating untrusted input.

**NFR-004 --- Password storage (Must)**\
Administrator passwords shall be hashed using PHP `password_hash()` and
checked using `password_verify()`. Plaintext passwords shall never be
stored or logged.

**NFR-005 --- Server-side validation (Must)**\
All untrusted form submissions shall be validated server-side,
regardless of browser-side JavaScript validation.

**NFR-006 --- CSRF protection (Must)**\
State-changing authenticated forms shall use CSRF protection. GET
requests shall not perform destructive or state-changing operations.

**NFR-007 --- XSS protection (Must)**\
User-controlled content shall be contextually escaped when rendered. If
rich text is supported, it shall be sanitized with an appropriate
allowlist rather than trusted as raw HTML.

**NFR-008 --- Authentication and session security (Must)**\
Protected routes shall verify authentication and authorization.
Production sessions shall use appropriate cookie flags, session ID
regeneration after login, inactivity/session-lifetime controls, and
HTTPS.

**NFR-009 --- Secret management (Must)**\
Production credentials and secrets shall be stored outside public web
files and excluded from version control. A `.env.example` may document
required variable names without real secrets.

**NFR-010 --- Maintainability (Must)**\
The application shall separate public entry points, views, business
logic, and database access sufficiently to avoid placing the whole
application in one PHP file. Setup and deployment instructions shall be
documented.

**NFR-011 --- Compatibility (Must)**\
The site shall be tested in current versions of major browsers available
to the developer and on representative mobile and desktop devices. Exact
browser versions shall be recorded during final testing.

**NFR-012 --- Accessibility (Should)**\
The interface shall be designed and tested against WCAG 2.2 AA criteria
where applicable, including semantic structure, keyboard access, visible
focus, form labels, contrast, and text alternatives for meaningful
images. Any known gaps shall be documented.

**NFR-013 --- Performance (Should)**\
Measure representative public pages before release. Aim for good Core
Web Vitals where feasible, including LCP of 2.5 seconds or less at the
75th percentile when sufficient field data exists. For a new site
without field data, use documented lab tests and treat this as a target
rather than a guaranteed result.

**NFR-014 --- Backup and recovery (Must)**\
Document a repeatable database backup process and verify that a backup
can be restored in a test environment before launch. Backup frequency
and retention must be agreed with the hosting plan and club's
operational needs.

**NFR-015 --- Error handling (Must)**\
Production pages shall not expose stack traces, database credentials,
SQL statements, or detailed internal errors. Errors shall be logged
securely and shown to visitors in a helpful, non-sensitive form.

## 6. Data Requirements

### 6.1 Initial data entities

-   **Administrator:** identifier, name, email/username, password hash,
    status, role/permissions, timestamps.
-   **Event:** identifier, title, description, start/end date and time
    as needed, location, publication status, optional image, timestamps.
-   **News article:** identifier, title, body, publication status/date,
    optional image, author reference if implemented, timestamps.
-   **Service project:** identifier, title, description, date/period,
    location, outcomes/impact details, publication status, optional
    images, timestamps.
-   **Enquiry:** identifier, sender name, contact detail, message,
    submission timestamp, status if implemented.
-   **Club settings/content:** club name, logo reference, approved
    contact details, social links, and agreed branding values.

These are logical entities, not the final physical database schema.
Field types, constraints, indexes, relationships, deletion behavior, and
migration scripts must be decided during database design.

### 6.2 Data handling

-   Collect only information necessary for the enquiry or
    content-management purpose.
-   Do not collect passwords or sensitive personal data through the
    public contact form.
-   Restrict enquiries and administrator data to authorized users.
-   Define a retention/deletion approach for enquiries before launch.
-   Obtain approval for photographs, personal profiles, and other
    content before publishing them.
-   Use foreign keys and appropriate constraints where relationships
    exist.
-   Use migrations or another documented process to track schema
    changes.

## 7. Hosting and budget recommendation

### 7.1 Recommended deployment

Use reputable **managed shared PHP/MySQL hosting** for the first release,
rather than a VPS or cloud server that the developer must administer.
The website is a small, server-rendered club site and does not initially
need dedicated infrastructure. The provider must support a currently
supported PHP 8.x release, MySQL/MariaDB, PDO MySQL, HTTPS/SSL, SFTP,
custom domains, database export/import, scheduled or manual backups, and
secure configuration of secrets. Prefer a plan with automated backups
and the ability to keep application code outside the public document
root. Confirm all features against the actual plan before purchase.

A shared-hosting provider such as Hostinger is one candidate to compare,
not a mandatory selection. Its published plan details show PHP/MySQL
hosting and SSL options, but advertised introductory rates can require
long prepayment periods and renewal rates can be higher; compare the
full amount due now and the renewal price, not only the monthly headline.
See the provider's current [official plan comparison](https://www.hostinger.com/pricing)
and [hosting-plan limits](https://support.hostinger.com/en/articles/6976044-parameters-and-limits-of-hosting-plans).

### 7.2 Planning budget (estimate, not a quote)

For planning purposes, target **US$50–100 for the first year** for a
basic domain plus suitable shared hosting, excluding any paid email
service, premium theme, or developer labour. Keep a contingency of about
20% if club funds allow. This is a budgeting envelope only; domain
extension, tax, currency conversion, promotions, contract length, and
renewal pricing can change the final cost. Obtain at least two current
quotes and record the total first-year cost and annual renewal cost in
the decision log before the club pays.

Budget rules:
- Start with one year unless the club explicitly approves a longer commitment.
- Register the domain and hosting account using a club-controlled email and ensure the club retains ownership and recovery access.
- Enable free SSL/HTTPS; do not buy a paid certificate unless a demonstrated need arises.
- Use a free transactional SMTP option or included email only if its limits and deliverability meet the need; do not promise email alerts until tested.
- Keep a separate backup copy outside the hosting account. Do not rely solely on the host's backup feature.
- Avoid paid plugins, VPS hosting, and unnecessary recurring services in v1.

### 7.3 Minimum deployment and backup checks

Before launch, verify HTTPS, PHP version/extensions, database connectivity,
file-upload limits, SMTP behaviour if used, scheduled/manual backup,
restoration into a test database, and that configuration files and
backups cannot be downloaded publicly. Record the account owner, renewal
date, renewal price, recovery method, and the person responsible for
renewal. Reassess hosting only if traffic, reliability, or functionality
outgrows the shared plan.

## 8. Constraints and Assumptions

### 8.1 Constraints

-   Target launch date: 30 November 2026.
-   Development is primarily undertaken by one developer.
-   Preferred stack: HTML, CSS, JavaScript, PHP, and MySQL.
-   Hosting/domain budget is not yet confirmed; planning target is US$50–100 for year one, subject to current quotes and club approval.
-   Club-member input is expected to be limited.
-   The first release is for Rotaract Club of UNZA.

### 8.2 Assumptions requiring confirmation

-   The club can supply approved branding, leadership details, contact
    information, and initial content before launch.
-   A hosting provider compatible with the chosen PHP and MySQL versions
    can be selected within available resources.
-   A designated club representative will review and accept the website.
-   Administrator accounts will be provisioned by a controlled process
    rather than public self-registration.
-   The club will identify a person responsible for maintaining content
    after launch.

If an assumption proves false, record the impact and revise scope or
schedule rather than silently changing the requirement.

## 9. Acceptance and Release Criteria

The first release is ready for acceptance when:

1.  All Must-have functional requirements are implemented or explicitly
    accepted as deferred by the authorized club representative.
2.  Public pages display approved content and work at representative
    mobile and desktop sizes.
3.  Authorized users can manage the agreed content types through the
    dashboard.
4.  Authentication and server-side authorization checks pass the agreed
    tests.
5.  Contact enquiries can be submitted and reviewed securely.
6.  Security tests cover prepared statements, input validation, output
    escaping, CSRF, session handling, and access control.
7.  No known critical or high-severity release-blocking defects remain.
8.  Production configuration uses HTTPS and does not expose credentials
    or debug details.
9.  A database backup has been created and a restore has been tested.
10. Deployment, setup, backup, and maintenance instructions are
    delivered.
11. A designated club representative reviews the site and records
    acceptance.

## 10. Open Issues

| ID | Issue / decision needed | Owner | Status |
|---|---|---|---|
| OI-01 | Confirm named Site Administrators, Content Managers, and Enquiry Manager and approve role boundaries | President / Board | Proposed for approval |
| OI-02 | Confirm whether administrator-account management UI is required or secure manual provisioning is sufficient for launch | President / Board / Developer | Proposed for approval |
| OI-03 | Confirm whether enquiry email notifications are required; test SMTP if selected | Board / designated contact | Open |
| OI-04 | Confirm archive/soft-delete policy and permanent-deletion permissions | Board / Public Image Committee | Proposed for approval |
| OI-05 | Approve required and optional fields for events, news, and projects | Public Image Committee / Secretary | Proposed for approval |
| OI-06 | Select hosting/domain provider and approve upfront and renewal costs | President / Treasurer / Developer | Open |
| OI-07 | Confirm public-use permission and accuracy for supplied photos, stories, and impact figures | Public Image Committee / content owner | Open |
| OI-08 | Assign post-launch content maintenance and technical ownership | Board | Open |
| OI-09 | Confirm whether club branding/settings must be editable in the dashboard or may remain in configuration for launch | President / Developer | Open |
| OI-10 | Approve enquiry retention period; proposed default is 12 months after closure | Board / designated contact | Proposed for approval |

The recommendations in v1.1 are planning defaults, not formal club
approval. Confirm the named role holders, hosting quote/renewal price,
enquiry workflow and retention, and content readiness before launch.

## 11. Approval

This SRS is a **draft baseline pending stakeholder validation**, not
evidence of formal club approval.

  Role                                    Name              Status / date
  --------------------------------------- ----------------- ---------------
  Club representative                     To be confirmed   Pending
  Public Image Committee representative   To be confirmed   Pending
  Project developer                       To be completed   Pending
