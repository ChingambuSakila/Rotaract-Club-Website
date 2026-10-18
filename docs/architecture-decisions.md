# Architecture Decision Records (ADR)

**Project:** Rotaract Club of UNZA Website\
**Document ID:** ADR-INDEX-001\
**Version:** 1.1\
**Status:** Initial decisions plus recommendations; selected items require validation\
**Last updated:** 18 October 2026

## 1. Purpose

This document records significant technical decisions, their context,
the options considered, and their consequences. It is intended to
prevent repeated debate and help future maintainers understand why the
application is structured as it is.

Decisions are marked **Accepted**, **Proposed**, or **Pending**. A
proposed decision should be confirmed before it becomes an
implementation constraint.

------------------------------------------------------------------------

## ADR-001: Use a PHP/MySQL Server-Rendered Application

**Status:** Accepted\
**Decision date:** 18 October 2026

### Context

The developer prefers HTML, CSS, JavaScript, PHP, and MySQL and wants to
balance learning, quality, and delivery speed.

### Decision

Use HTML and CSS for presentation, JavaScript for progressive
client-side interaction, PHP for server-side rendering and business
logic, and MySQL for persistence. Use PHP PDO for database access.

### Alternatives considered

-   React frontend with a separate API.
-   Node.js backend.
-   A PHP framework.
-   A hosted website builder.

### Rationale

A server-rendered PHP application fits the preferred stack and avoids
the additional complexity of maintaining a separate frontend and backend
application. It also provides a practical opportunity to learn
full-stack development.

### Consequences

-   Keep the first release as one deployable application.
-   Avoid adding a JavaScript framework or separate REST API unless a
    demonstrated requirement justifies it.
-   Use prepared statements and centralize database access.
-   Reconsider a framework such as Laravel if authentication, routing,
    migrations, and testing become difficult to implement safely within
    the deadline.

------------------------------------------------------------------------

## ADR-002: Use a Modular Monolith

**Status:** Accepted\
**Decision date:** 18 October 2026

### Context

The site needs public pages, content management, authentication, and
database access, but the project is small and developed by one person.

### Decision

Build one PHP application with clear boundaries between routing/entry
points, views, application logic, and data access. Do not split the
system into microservices.

### Alternatives considered

-   A single large PHP file.
-   Separate services for each content type.
-   A separate frontend and backend.
-   A full microservices architecture.

### Rationale

A modular monolith is easier to develop, test, deploy, and maintain at
this scale while still encouraging good separation of concerns.

### Consequences

-   Avoid placing the whole application in `index.php`.
-   Use shared layouts and reusable components.
-   Keep SQL and database connection code out of view templates.
-   Introduce additional layers only where they improve clarity or
    testability.

------------------------------------------------------------------------

## ADR-003: Build for One Club First, with Reusable Core Code

**Status:** Accepted\
**Decision date:** 18 October 2026

### Context

The first website is for Rotaract Club of UNZA. The developer wants the
code to be reusable for separate club websites later, but does not want
to build a shared multi-tenant platform for the first release.

### Decision

Deploy the first instance for Rotaract Club of UNZA. Keep club-specific
identity and content configuration separate from core application logic
where practical. Future clubs may receive separate deployments using the
same codebase.

### Alternatives considered

-   A single multi-tenant platform with multiple clubs in one database.
-   A bespoke, hardcoded site with no reuse considerations.
-   A configurable application deployed separately for each club.

### Rationale

Separate deployments reduce first-release complexity and avoid the need
to implement and test cross-club data isolation in the initial release.

### Consequences

-   Do not add tenant IDs to every table solely for a multi-tenant
    design that is not required.
-   Document configuration and deployment steps so another club can
    reuse the code.
-   Keep secrets, content, and deployment settings environment-specific.
-   Revisit this ADR if the project changes to a shared platform.

------------------------------------------------------------------------

## ADR-004: Use Role-Based Authorization with Server-Side Checks

**Status:** Proposed --- recommended role mapping pending club confirmation

### Context

Board members and Public Image Committee members should manage website
content. Their exact permissions have not yet been confirmed.

### Decision

Implement authentication for administrators and enforce permissions on
the server for every protected action. Represent roles/permissions
centrally rather than scattering hardcoded role checks throughout the
application.

### Alternatives considered

-   One shared administrator password.
-   Authentication without separate permissions.
-   Hardcoding role names into each page.
-   A centralized role/permission policy.

### Rationale

Centralized authorization reduces the risk of accidentally allowing
unauthorized users to modify content. It supports future reuse and
easier changes to the club's permission model.

### Consequences

-   Never rely only on hiding buttons or links in the interface.
-   Verify authorization for direct HTTP requests as well as dashboard
    navigation.
-   Use individual administrator accounts rather than shared
    credentials.
-   Use the recommended roles: Site Administrator (President plus one named
    successor), Content Manager (authorized Board/PIC users), and Enquiry
    Manager (Secretary/designated contact). Role combinations are allowed
    only when deliberately assigned.
-   Content Managers may create, edit, publish, unpublish, and archive
    content but cannot manage administrator accounts, secrets, or hosting.
-   Only Site Administrators may create/disable administrator accounts or
    change global settings. Enquiry access is limited to Site Administrators
    and the Enquiry Manager.
-   Account-management privileges should be restricted to specifically
    authorized users; use individual accounts, no public self-registration,
    and at least two active Site Administrators.

------------------------------------------------------------------------

## ADR-005: Defer Member Accounts

**Status:** Accepted\
**Decision date:** 18 October 2026

### Context

The user selected administrator accounts first and member accounts
later.

### Decision

The first release will not implement public member registration, member
profiles, or member-only pages. Public content will be accessible
without login; only administrative operations require authentication.

### Consequences

-   Avoid building member registration and profile tables in the first
    release unless another approved requirement makes them necessary.
-   Reassess member accounts as a future feature based on a documented
    club need.

------------------------------------------------------------------------

## ADR-006: Authorized Content Managers Can Publish Directly

**Status:** Accepted\
**Decision date:** 18 October 2026

### Context

The user selected immediate publication by authorized users rather than
a content approval workflow.

### Decision

Authorized users may publish content directly. Content must still pass
server-side validation, and the system must distinguish published from
unpublished content.

### Consequences

-   Do not build a multi-step approval workflow in the first release.
-   Provide clear publish/unpublish controls.
-   Ensure unpublished content is not exposed through public pages or
    public detail routes.
-   Confirm who may delete content and whether deletion should be
    permanent or archival.

------------------------------------------------------------------------

## ADR-007: Use MySQL with PDO and Parameterized Queries

**Status:** Accepted\
**Decision date:** 18 October 2026

### Context

The selected stack uses PHP and MySQL, and the site will accept user
input through administration forms and a public contact form.

### Decision

Use MySQL for relational persistence and PHP PDO for database access.
Use parameterized queries for values supplied by users or other
untrusted sources.

### Consequences

-   Keep credentials out of source control and public directories.
-   Use foreign keys and constraints where appropriate.
-   Track schema changes through migrations or a documented
    schema-change process.
-   Validate inputs on the server and escape output appropriately.
-   Do not assume prepared statements replace authorization, validation,
    or output escaping.

------------------------------------------------------------------------

## ADR-008: Keep the Web Document Root in `public/`

**Status:** Proposed

### Context

The project will contain configuration, application code, database
scripts, and documentation that must not be directly served by the web
server.

### Decision

Configure the hosting document root to point to the project's `public/`
directory.

### Consequences

-   Application code, database scripts, `.env` files, and internal
    documentation should remain outside the publicly served directory.
-   If the chosen host does not permit changing the document root,
    select a safe alternative deployment layout and verify that
    sensitive files cannot be requested over HTTP.

------------------------------------------------------------------------

## ADR-009: Store Secrets Outside Version Control

**Status:** Accepted\
**Decision date:** 18 October 2026

### Context

The application needs database credentials and may require production
service credentials.

### Decision

Use environment-specific configuration or environment variables for
secrets. Commit only a `.env.example` containing placeholder variable
names, never actual secrets. Add local secret files to `.gitignore`.

### Consequences

-   Rotate any credential that is accidentally committed.
-   Use different credentials for local development and production.
-   Restrict database privileges to what the application needs.

------------------------------------------------------------------------

## ADR-010: Use Incremental Development and Traceability

**Status:** Accepted\
**Decision date:** 18 October 2026

### Context

The project has a fixed target date of 30 November 2026 and is being
developed by one person while learning.

### Decision

Implement the website in small, end-to-end increments. Link requirements
to implementation areas and test evidence using the requirements
traceability matrix. Use Git for version control.

### Consequences

-   Prioritize the Must-have requirements.
-   Test features as they are built rather than postponing all testing
    until the end.
-   Keep a small backlog and record changes to scope.
-   Defer optional features if they threaten security, testing, or
    launch readiness.

------------------------------------------------------------------------

## ADR-011: Hosting and Domain Provider

**Status:** Proposed --- provider and quote pending

### Context

The hosting/domain budget is unknown.

### Decision

Use managed shared PHP/MySQL hosting for the first release. Do not use a
VPS unless a concrete need arises. Hostinger is one candidate to compare,
not a committed provider. The selected plan must support a currently
supported PHP 8.x release, MySQL/MariaDB, PDO MySQL, HTTPS/SSL, SFTP,
custom domains, database export/import, and a practical backup/restore
process. Prefer automated backups and a document root that can point to
`public/`.

Use **US$50–100 as a first-year planning envelope** for domain plus shared
hosting, excluding paid email and extras; this is not a quote. Obtain at
least two current quotes, record upfront and renewal prices, and allow
for currency conversion/taxes. Choose a one-year term unless the club
approves longer prepayment. Keep the domain and hosting account under
club-controlled ownership and retain a separate backup copy.

### Required follow-up

-   Confirm the available budget with the club.
-   Compare total first-year and renewal costs.
-   Verify that the provider supports the required PHP version and MySQL
    features.
-   Test deployment and database restoration before launch.

------------------------------------------------------------------------

## ADR-012: Content Management, Deletion and Enquiry Retention

**Status:** Pending

### Context

The website will store published content and contact enquiries.
Permanent deletion, archiving, and enquiry retention rules have not been
agreed.

### Decision

Use minimal required content fields and allow optional details to be
omitted. Recommended required fields are title/date-time/location/short
description for events; title/body for news; and title/description/date
or period for service projects. Use draft/unpublished and published
states, with direct publishing by authorized users. Prefer archive/soft
delete for public content, with permanent deletion restricted to Site
Administrators and a confirmation step. Do not retain contact enquiries
indefinitely; a proposed starting policy is deletion/anonymization of
closed enquiries after 12 months, subject to club confirmation.

### Required follow-up

-   Decide whether events, news, and projects can be archived.
-   Confirm archive and permanent-delete permissions.
-   Confirm the proposed 12-month enquiry retention period and whether
    email notifications are required.
-   Limit enquiry access to Site Administrators and the Enquiry Manager.
-   Provide a safe way to remove data when required.

------------------------------------------------------------------------

## 2. Proposed Project Structure

The following is a suggested target structure. Add directories
incrementally as the application needs them.

``` text
rotaract-website/
├── public/
│   ├── index.php
│   └── assets/
│       ├── css/
│       ├── js/
│       └── images/
├── app/
│   ├── Controllers/
│   ├── Models/
│   ├── Repositories/
│   ├── Services/
│   ├── Views/
│   └── Helpers/
├── config/
├── database/
│   ├── migrations/
│   └── seeds/
├── tests/
├── docs/
├── .env.example
├── .gitignore
└── README.md
```

## 3. Decision Review Triggers

Revisit an architecture decision if: - The site becomes a shared
platform for multiple clubs. - Member accounts become a required
feature. - Multiple developers begin working on the application. - The
application requires substantial integrations or a public API. - Hosting
constraints prevent the planned deployment layout. - A decision creates
unacceptable security or maintenance risk.

Record each change with the decision date, reason, alternatives, and
impact on the SRS and traceability matrix.


------------------------------------------------------------------------

## ADR-013: Role Model and Content Publishing

**Status:** Proposed --- club confirmation required

### Context

Board Members and Public Image Committee members need to maintain public
content, while the site should prevent ordinary content contributors from
changing administrator access or technical settings.

### Decision

Adopt least-privilege roles: Site Administrator, Content Manager, and
Enquiry Manager. Use individual accounts. Authorized Content Managers may
publish immediately without a separate approval workflow. Prefer archive/
soft-delete for content; reserve permanent deletion and account management
for Site Administrators. If account-management UI cannot be delivered safely
before launch, provision accounts manually and document the process.

### Consequences

- Enforce permissions server-side for each action.
- Provide no public admin registration and no shared administrator login.
- Keep a record of content creator/updater and timestamps where practical.
- Have the club confirm named role holders and retention rules before launch.

------------------------------------------------------------------------

## ADR-014: Content Fields and Contact Enquiries

**Status:** Proposed --- club confirmation required

### Decision

Use small required field sets for events, news, and service projects, with
impact measures and media optional. Store contact enquiries in the
protected dashboard. If SMTP is configured and tested, also send a notice
to the designated club email; email failure must not prevent storage. If
notifications are not ready by launch, label the workflow dashboard-only
and assign a person to check it regularly. A 12-month retention period for
closed enquiries is the proposed default, subject to club approval.

### Consequences

- Content forms remain quick to use and support partial information.
- Images require file validation and permission for public use.
- Enquiry access and retention must be implemented consistently.
