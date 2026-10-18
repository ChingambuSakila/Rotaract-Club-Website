# System Architecture Design

## Rotaract Club of UNZA Website

**Document version:** 1.0\
**Status:** Draft for review\
**Requirements baseline:** Version 1.1\
**Target launch:** 30 November 2026\
**Prepared for:** Rotaract Club of UNZA\
**Architecture style:** Modular monolith, organized using layered
architecture and MVC principles

------------------------------------------------------------------------

## 1. Purpose

This document describes the proposed high-level architecture for the
Rotaract Club of UNZA website. It translates the approved direction of
the requirements into a technical design that can guide database design,
UI/UX design, implementation, testing, and deployment.

The first release serves one club: Rotaract Club of UNZA. The code
should be organized so that useful modules can be reused for other clubs
in the future, but the first release is **not** a multi-tenant platform.

## 2. Architectural goals

The architecture should:

1.  Support the public website and a protected administrator dashboard.
2.  Support events, news and announcements, service projects and impact
    reports, contact enquiries, and club settings.
3.  Use HTML, CSS, JavaScript, PHP, and MySQL.
4.  Be feasible for one developer who is learning while building.
5.  Be deployable to managed PHP/MySQL hosting without complex
    infrastructure.
6.  Encourage separation of concerns, testability, and reuse.
7.  Protect administrator accounts, contact enquiries, and uploaded
    media.
8.  Meet the 30 November 2026 target by avoiding unnecessary complexity.

## 3. Selected architecture

### 3.1 Decision

Use a **modular monolith** as the overall application architecture.
Organize the code using **layers** and apply **MVC principles** where
useful.

These are complementary choices:

-   **Modular monolith:** the application is deployed as one unit, but
    is divided into clearly defined business modules.
-   **Layered architecture:** separates presentation,
    application/business logic, and data access.
-   **MVC:** separates request handling/controllers, domain/data models,
    and views/templates.

### 3.2 Why this is suitable

-   The application has several related features but does not need
    independently deployed services.
-   A single PHP application and MySQL database keep hosting and
    deployment manageable.
-   Module boundaries reduce the risk of one large, tangled codebase.
-   The design is practical for a solo developer and a near-term launch.
-   Modules can be reused or extracted later if real needs justify it.

Microservices, Kubernetes, message brokers, and a separate API gateway
are out of scope for the first release.

## 4. System context

``` text
                    +--------------------------+
                    | Public visitor / member  |
                    | Uses browser on phone or |
                    | computer                 |
                    +------------+-------------+
                                 |
                                 | HTTPS
                                 v
                    +--------------------------+
                    | Rotaract Club of UNZA     |
                    | Website                  |
                    | Public pages + Admin UI  |
                    +------------+-------------+
                                 |
                                 | Server-side PHP
                                 v
                    +--------------------------+
                    | MySQL database           |
                    | Content, users, enquiries|
                    +--------------------------+

    Authorized club administrators use the protected dashboard.
    Visitors can view published content and submit contact enquiries.
```

### External actors

-   **Public visitor:** reads published club information, events, news,
    projects, and impact reports; submits a contact form.
-   **Authorized administrator:** signs in and performs actions allowed
    by assigned permissions.
-   **Hosting provider:** runs the PHP application and MySQL database,
    provides HTTPS, and supports backups/restoration.
-   **Club content owners:** supply approved text, event details,
    project results, and images to authorized publishers.

## 5. Logical architecture

``` text
+------------------------------------------------------------------+
|                         PRESENTATION                             |
| Public pages | Admin dashboard | Forms | Shared layouts          |
| HTML templates | CSS | JavaScript                                |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                     HTTP / CONTROLLER LAYER                      |
| Routes | Controllers | Request parsing | Response handling        |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                    APPLICATION / SERVICE LAYER                   |
| Validation | Use cases | Authorization checks | Content workflows |
| Authentication service | Enquiry handling | Club settings        |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                         DOMAIN / MODEL LAYER                     |
| Users and roles | Events | News | Projects | Enquiries | Settings |
| Domain rules and state transitions                                |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                        DATA ACCESS LAYER                         |
| Repositories or focused data-access classes | PDO | Transactions  |
+--------------------------------+---------------------------------+
                                 |
                                 v
+------------------------------------------------------------------+
|                            MYSQL                                 |
| Persistent application data                                      |
+------------------------------------------------------------------+
```

The diagram describes logical responsibilities, not necessarily separate
deployed processes. These layers all run within the same PHP
application.

## 6. Application modules

  -----------------------------------------------------------------------
  Module                  Responsibilities        Main data
  ----------------------- ----------------------- -----------------------
  Public Website          Home page, About,       Club settings and
                          navigation, public      published content
                          layouts                 

  Authentication          Login, logout, password Users, password hashes,
                          verification, session   account status
                          lifecycle               

  Authorization           Role and permission     Roles, user-role
                          checks for every        assignments,
                          protected action        permissions if
                                                  implemented

  Events                  Create, edit, publish,  Events
                          unpublish, archive, and 
                          display events          

  News & Announcements    Manage and display      News/articles
                          articles and            
                          announcements           

  Projects & Impact       Manage service          Projects/reports
                          projects, reports, and  
                          approved impact figures 

  Contact Enquiries       Validate submissions,   Contact enquiries
                          store enquiries, track  
                          status, support         
                          retention               

  Administration          Dashboard summaries and Aggregated module data
                          links to management     
                          tasks                   

  Club Settings           Manage approved public  Club settings
                          details and branding    
                          settings                

  Media                   Validate, store, and    Media metadata and file
                          serve uploaded images   storage

  Audit/Logging           Record important        Audit/log records
                          administrative actions  
                          and operational errors  
                          where practical         
  -----------------------------------------------------------------------

For the first release, keep modules focused. Avoid building a generic
content-management framework before the core use cases work.

## 7. Role and permission boundaries

The proposed roles are:

-   **Site Administrator:** can manage administrator accounts, global
    settings, all content, and contact enquiries.
-   **Content Manager:** can create, edit, publish, unpublish, and
    archive events, news, projects, and impact reports.
-   **Enquiry Manager:** can view and update contact enquiries according
    to the club's policy.

A user may hold more than one role. Exact assignments must be confirmed
by the club.

Security rules:

1.  The server must check permissions for every protected action; hiding
    a button is not security.
2.  Public users must not access the dashboard or private enquiries.
3.  Public administrator registration must not exist.
4.  Each administrator must have an individual account.
5.  Site Administrator accounts should be limited to a small, designated
    group.
6.  If a full account-management interface is not feasible before
    launch, create accounts through a documented secure setup process.
7.  Log important actions such as role changes, account creation, and
    permanent deletion where practical.

## 8. Key request flows

### 8.1 Public visitor views an event

1.  Browser requests the Events page over HTTPS.
2.  The route directs the request to the Events controller.
3.  The controller asks the Events service or data-access component for
    published events.
4.  The data-access component queries MySQL using PDO and prepared
    statements.
5.  The controller passes the results to the view.
6.  The view renders HTML; CSS and JavaScript provide presentation and
    optional interaction.
7.  The browser displays published events only.

### 8.2 Administrator publishes news

1.  Administrator submits the login form over HTTPS.
2.  Authentication verifies the account and password hash and starts a
    secure session.
3.  Administrator opens the news-management form.
4.  The server checks the user's permission to create or publish news.
5.  The application validates and normalizes submitted fields.
6.  The data-access layer writes the article to MySQL using prepared
    statements.
7.  The application records publication metadata and, where implemented,
    an audit event.
8.  The public News page displays the article because its status is
    published.

There is no separate approval workflow in the first release: authorized
publishers may publish directly.

### 8.3 Visitor submits a contact enquiry

1.  Visitor submits the contact form over HTTPS.
2.  The server validates required fields and applies basic anti-abuse
    controls.
3.  The application stores the enquiry in MySQL.
4.  The visitor sees a generic success confirmation that does not expose
    internal data.
5.  An authorized Enquiry Manager can review and update the enquiry
    status.
6.  Email notifications are optional and should only be enabled after
    configuration and testing.

## 9. Data and persistence design principles

The detailed schema will be produced in the Database Design phase. The
architecture anticipates these core entities:

-   Users
-   Roles and user-role assignments
-   Events
-   News/articles
-   Projects and impact reports
-   Contact enquiries
-   Club settings
-   Media metadata
-   Optional audit log

Data principles:

-   Use primary keys and foreign keys where appropriate.
-   Store timestamps for creation and updates; published content should
    also record publication time.
-   Represent content status explicitly, such as draft, published,
    unpublished, or archived, as appropriate for each content type.
-   Use transactions for operations that must succeed or fail together.
-   Use PDO prepared statements for database access.
-   Do not store plaintext passwords.
-   Store uploaded files separately from their metadata; do not store
    executable uploads in a public directory.
-   Prefer archive/soft-delete behavior for content; restrict permanent
    deletion.
-   Define enquiry retention and deletion rules with the club before
    launch.

## 10. Deployment architecture

``` text
                  Visitor / Administrator Browser
                               |
                               | HTTPS
                               v
                    +----------------------+
                    | DNS + Domain         |
                    | Managed PHP Hosting  |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Web Server            |
                    | Apache or equivalent  |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | PHP Application       |
                    | Public + Admin modules|
                    +----------+-----------+
                               |
                         Private DB access
                               |
                               v
                    +----------------------+
                    | MySQL / MariaDB       |
                    +----------------------+

     Backups: database export + media/application backup
     Secrets: hosting environment/config outside public web root
```

### Hosting recommendation

Use managed shared PHP/MySQL hosting for the first release, subject to
verification that the provider supports a currently supported PHP 8.x
version, MySQL/MariaDB, HTTPS, backups and restoration, secure file
transfer, and configuration outside the public web root.

The selected provider, domain, final price, and backup schedule remain
decisions to confirm. The domain and hosting accounts should be owned by
the club or an officially designated club representative, not solely by
the developer's personal account.

## 11. Security architecture

The first release should include these controls:

-   HTTPS for all public and administrative traffic.
-   `password_hash()` and `password_verify()` for passwords.
-   Server-side authentication and authorization checks.
-   PDO prepared statements.
-   Server-side validation of all inputs.
-   Context-appropriate output escaping to mitigate XSS.
-   CSRF tokens for state-changing forms.
-   Secure session cookie settings, session ID regeneration after login,
    and logout/session expiration.
-   Login rate limiting or throttling appropriate to the hosting
    environment.
-   Strict upload allowlists, size limits, MIME/type checks, generated
    filenames, and storage outside the executable/public path where
    possible.
-   Secrets excluded from version control and kept outside the public
    web root.
-   Generic user-facing errors; detailed diagnostics written to
    protected logs.
-   Backups with a tested restoration procedure.
-   Restricted access to contact enquiries and administrative logs.

Security controls must be tested; listing them in a design document does
not by itself make the implementation secure.

## 12. Quality attributes and design responses

  -----------------------------------------------------------------------
  Quality attribute                   Architectural response
  ----------------------------------- -----------------------------------
  Maintainability                     Separate modules, clear layer
                                      responsibilities, shared layouts
                                      and utilities

  Security                            Central authorization checks, PDO,
                                      validation, escaping, CSRF and
                                      session controls

  Usability                           Responsive public pages, clear
                                      navigation, consistent
                                      administration forms

  Performance                         Efficient queries, pagination where
                                      needed, optimized images,
                                      appropriate indexes

  Availability                        Managed hosting, backups,
                                      documented recovery process

  Portability                         Avoid unnecessary provider-specific
                                      services and document configuration

  Reusability                         Keep club-specific values in
                                      configuration/settings and modules
                                      reusable

  Testability                         Keep business rules out of
                                      templates; test validation,
                                      permissions, and core use cases
  -----------------------------------------------------------------------

## 13. Suggested source-code structure

This is a starting point; it can be adjusted during implementation.

``` text
rotaract-unza-website/
├── public/                    # Only publicly accessible web files
│   ├── index.php              # Front controller/entry point
│   └── assets/
│       ├── css/
│       ├── js/
│       └── images/
├── app/
│   ├── Core/                  # Router, database connection, helpers
│   ├── Middleware/            # Authentication and authorization checks
│   ├── Modules/
│   │   ├── Auth/
│   │   ├── Users/
│   │   ├── Events/
│   │   ├── News/
│   │   ├── Projects/
│   │   ├── Contact/
│   │   ├── Admin/
│   │   ├── Settings/
│   │   └── Media/
│   └── Shared/                # Shared layouts, validation, utilities
├── config/                    # Non-secret defaults; secrets injected securely
├── database/
│   ├── migrations/
│   └── seeds/
├── storage/                   # Logs and non-public uploads where supported
├── tests/
├── docs/
├── .gitignore
└── README.md
```

**Deployment rule:** Configure the hosting document root to point to
`public/` if the provider permits it. If not, adapt deployment carefully
so that application source, credentials, logs, and private uploads
cannot be requested directly through the browser.

## 14. Out of scope for the first release

To protect the launch target, the first release excludes:

-   Microservices or a distributed service architecture.
-   A public member registration portal.
-   Multi-club tenancy or a shared platform for multiple clubs.
-   A complex approval workflow for content.
-   Online payments and membership-fee processing.
-   Event ticketing/registration unless separately prioritized.
-   Native mobile applications.
-   A complex API for third-party integrations.
-   Advanced analytics beyond practical dashboard summaries.

These may be considered later if stakeholders identify a clear need.

## 15. Risks and mitigations

  -----------------------------------------------------------------------
  Risk                                Mitigation
  ----------------------------------- -----------------------------------
  Scope grows before launch           Prioritize required public pages,
                                      admin login, content publishing,
                                      and contact enquiries

  Permissions are too broad           Confirm role matrix and test each
                                      protected action

  Hosting restrictions prevent safe   Verify document-root and
  file separation                     configuration options before
                                      purchasing hosting

  Content or images arrive late       Agree content owners and use
                                      approved placeholder content during
                                      development

  Lost data or accidental deletion    Backups, archive-first behavior,
                                      and a restoration test

  Security work is postponed          Include security checks in each
                                      module's implementation and test
                                      plan

  Future reuse becomes difficult      Keep club identity/configuration
                                      separate from reusable business
                                      modules
  -----------------------------------------------------------------------

## 16. Open decisions

Before implementation is finalized, confirm:

1.  Final role assignments and whether Site Administrators may
    permanently delete content.
2.  Hosting provider, domain, renewal budget, and backup schedule.
3.  Whether email notifications are required for contact enquiries.
4.  The content fields and statuses required for events, news, projects,
    and reports.
5.  The enquiry retention period.
6.  Whether club settings can be edited in the dashboard or only through
    configuration.
7.  Who owns content updates and technical maintenance after launch.

## 17. Architecture acceptance checklist

-   [ ] The public website and protected admin area are represented.
-   [ ] Each business module has a clear responsibility.
-   [ ] All protected actions require server-side permission checks.
-   [ ] Database access uses PDO prepared statements.
-   [ ] The design supports secure authentication and sessions.
-   [ ] Private application files and secrets are not publicly
    accessible.
-   [ ] The first release is a single-club application.
-   [ ] Hosting supports the selected PHP/MySQL design.
-   [ ] Backup and restoration arrangements are documented.
-   [ ] The architecture is checked against the SRS and traceability
    matrix.

------------------------------------------------------------------------

**Next design deliverable:** Database Entity-Relationship Diagram (ERD),
followed by the page wireframes.\
**Approval note:** This document is a proposed design and should be
reviewed against the Version 1.1 requirements and confirmed by the club
before it is treated as the final baseline.
