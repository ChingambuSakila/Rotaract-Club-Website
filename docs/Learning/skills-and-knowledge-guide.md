# Skills and Knowledge Guide

| Field | Details |
| :--- | :--- |
| **Project** | Rotaract Club of UNZA Website |
| **Document ID** | SKL-001 |
| **Version** | 1.0 |
| **Status** | Draft: personal learning plan |
| **Last updated** | 18 October 2026 |
| **Owner** | Project Developer |
| **Related** | [SRS-001](software-requirements-specification.md), [SAD-001](system-architecture-design.md), [ADR-LOG-001](architecture-decisions.md), [RTM-001](requirements-traceability-matrix.md), `work-breakdown-structure.md` (WBS-001) |

---

## 1. Purpose and How to Use This Guide

This guide lists the skills and knowledge needed for each part of the project, so you can learn exactly what you need, when you need it. It follows the work packages in the Work Breakdown Structure (WBS 1 to 12).

**Learning by doing works best like this:**

1. **Skim** the section for the work package you are about to start.
2. **Learn the minimum** from the "Know first" list (an hour or two, not days).
3. **Build the smallest working version** using the "Practice task".
4. **Check yourself** against the "You are ready when you can…" list.
5. **Move on**, then return to improve it when the schedule allows.

> **Rule of thumb:** only understand code you can explain in your own words. If you use an AI assistant, tutorials, or copied snippets, read each line, run it, break it on purpose, and explain it back before committing it. This matters most for security code.

### 1.1 Proficiency Scale

| Level | Name | Meaning |
| :---: | :--- | :--- |
| 1 | **Aware** | You know the concept exists and can find it in documentation |
| 2 | **Working** | You can do it with references open |
| 3 | **Proficient** | You can do it reliably and explain why |

Each area below states the **target level for this project**. Level 3 is only expected for the areas that carry real risk (security, authorization, data handling).

---

## 2. Skills Overview

| WBS | Project part | Main skill area | Target level | Learning curve | Risk if weak |
| :---: | :--- | :--- | :---: | :---: | :---: |
| 1 | Project management | Planning, estimation, tracking | 2 | Low | Medium |
| 2 | Requirements and decisions | Requirements engineering, communication | 2 | Medium | High |
| 3 | Architecture and design | Layered design, data modelling, UI design | 2 | Medium | High |
| 4 | Environment and foundation | Git, PHP structure, routing, configuration | 2 | Medium | Medium |
| 5 | Core security and authentication | Web security, sessions, access control | **3** | **High** | **High** |
| 6 | Admin dashboard and content management | CRUD, forms, uploads, state handling | 2–3 | Medium–High | High |
| 7 | Public website | HTML, CSS, JavaScript, accessibility | 2 | Medium | Medium |
| 8 | Content, branding, and consent | Content handling, privacy, rights | 2 | Low | Medium |
| 9 | Testing and QA | Test design, security and accessibility testing | 2 | Medium | High |
| 10 | Hosting, deployment, operations | DNS, shared hosting, backups, HTTPS | 2 | Medium | High |
| 11 | Documentation and training | Technical writing, teaching | 2 | Low | Low |
| 12 | Launch and handover | Release and handover practice | 2 | Low | Medium |

**Hardest areas for a first major project:** security and authorization (WBS 5, 6.2 to 6.5), deployment on unfamiliar hosting (WBS 10), and getting decisions from busy stakeholders (WBS 2, 8). Plan extra learning time there.

---

## 3. Cross-Cutting Foundations

You will use these throughout. Spend your first learning days making sure they are solid.

| Foundation | What to know | Why it matters here |
| :--- | :--- | :--- |
| **HTTP and the web** | Request and response, methods (GET, POST), status codes (200, 301, 403, 404, 500), headers, cookies, HTTPS | Every route, form, session, and security control rests on it |
| **SQL** | `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `JOIN`, `WHERE`, `ORDER BY`, `LIMIT`, primary and foreign keys, indexes, constraints, transactions | All content and accounts are stored in MySQL |
| **PHP language** | Variables, arrays, functions, classes and objects, namespaces, exceptions, `include`/`require`, type declarations | The whole backend |
| **Git and GitHub** | Commit, branch, merge, pull request, `.gitignore`, undoing mistakes | Version control and secret protection (NFR-009) |
| **Debugging** | Reading error messages, `error_log`, browser developer tools (Network, Console), `var_dump` and a step debugger | You will spend a lot of time here; it is a skill, not a failure |
| **Reading documentation** | Using the official PHP, MySQL, and MDN references | Faster and more accurate than random tutorials |
| **Security mindset** | Treat all input as untrusted; deny by default; test your own attacks | Required by NFR-003 to NFR-009 |

---

## 4. Skills by Project Part

### WBS 1: Project Management (24 h)

**What you will do:** plan the schedule, run stakeholder reviews, track progress, manage risks and changes.

| | |
| :--- | :--- |
| **Know first** | Work breakdown and estimation; MoSCoW prioritization; critical path idea; risk register; change control; reading an RTM |
| **Skills to practice** | Breaking work into small tasks; estimating then comparing to actuals; writing a short weekly status note; saying "no" or "later" with a documented reason |
| **Practice task** | Turn the WBS into a task board (for example GitHub Projects). Estimate each task, then record the actual hours for the first week and adjust the plan |
| **You are ready when you can…** | Show which tasks are on the critical path; name what you would cut first if you fall behind (see WBS §8 descoping levers); update RTM status without help |
| **Common pitfalls** | Estimating only coding time; treating the 256 h plan as exact; accepting extra features without logging the change |

### WBS 2: Requirements Validation and Decisions (14 h)

**What you will do:** close open issues, confirm field lists, get the SRS validated.

| | |
| :--- | :--- |
| **Know first** | Functional versus non-functional requirements; writing testable acceptance criteria; ambiguity and how to spot it; stakeholder analysis; traceability |
| **Skills to practice** | Asking closed, specific questions; running a 30-minute review with a prepared one-page summary; recording decisions in writing; separating what stakeholders *want* from what they *need* |
| **Practice task** | Take two open issues (for example OI-03 email notifications and OI-05 field lists). For each, write the question, two options with trade-offs, and your recommendation, then get a written answer |
| **You are ready when you can…** | Rewrite a vague requirement ("easy to use") into a testable one; explain every `Must` requirement and its test case |
| **Common pitfalls** | Silent assumptions; asking open-ended questions busy people will not answer; starting to build before sign-off on roles and fields |

### WBS 3: Architecture and Design (22 h)

**What you will do:** finalize the architecture, design the database, plan security, create wireframes and a UI kit.

| | |
| :--- | :--- |
| **Know first** | Layered architecture, MVC, modular monolith (read SAD-001 §3 to §6 and ADR-001, ADR-002); separation of concerns; database normalization (up to third normal form); entity-relationship diagrams; one-to-many and many-to-many relationships; soft delete and status columns; role-based access control (RBAC); mobile-first layout; colour contrast |
| **Skills to practice** | Drawing an ERD; choosing keys and data types; naming consistently; sketching wireframes for phone first; deciding what belongs in a controller, a service, a repository, and a view |
| **Practice task** | Draw the ERD for users, roles, events, news, projects, enquiries, settings, and media using SRS §5 as input. Then sketch the Events list, Event detail, and the admin Event form at 360 px width |
| **You are ready when you can…** | Explain why SQL does not belong in templates; justify each table and foreign key; show where a permission check will sit for any admin action |
| **Common pitfalls** | Designing a generic framework instead of what the SRS needs; storing roles as free text; no plan for Draft, Published, and Archived states |

### WBS 4: Environment and Project Foundation (14 h)

**What you will do:** set up Git, a local PHP/MySQL stack, the project skeleton, configuration, database scripts, and error logging.

| | |
| :--- | :--- |
| **Know first** | Git branching and `.gitignore`; a local stack (for example XAMPP, Laragon, or Docker); PHP object-oriented basics; namespaces and autoloading (PSR-4, optionally with Composer); the front-controller pattern and simple routing; environment variables and `.env` files (ADR-009); PDO connection and exceptions; plain-SQL migration scripts; PHP error reporting versus production logging (NFR-015) |
| **Skills to practice** | Writing a minimal router; loading configuration safely; creating a repeatable database setup script; keeping `.env` out of Git |
| **Practice task** | Build a "Hello" app: `public/index.php` routes `/` and `/about` to controllers that render a shared layout, reads the database name from `.env`, and logs an error to a file when you trigger one on purpose |
| **You are ready when you can…** | Rebuild the database from scripts in under five minutes; prove `.env` is not in the repository; show that a PHP error shows a generic page to the visitor and details in the log |
| **Common pitfalls** | Committing secrets; building a mini-framework too elaborate for the schedule (ADR-001 allows re-evaluating a framework if this becomes unsafe); display of PHP errors on in production |

### WBS 5: Core Security and Authentication (24 h): highest priority

**What you will do:** sessions, login, role checks, CSRF protection, validation, and output escaping.

| | |
| :--- | :--- |
| **Know first** | **Sessions and cookies:** `HttpOnly`, `Secure`, `SameSite`, session fixation, `session_regenerate_id()`, inactivity expiry. **Passwords:** `password_hash()`, `password_verify()`, `password_needs_rehash()`, why never to write your own hashing. **Authorization:** RBAC, deny by default, object-level checks (can *this* user change *this* record?). **CSRF:** synchronizer tokens, `random_bytes()`, `hash_equals()`. **XSS:** escaping with `htmlspecialchars()` (including quotes and encoding) and why context matters (HTML, attributes, URLs, JavaScript). **Validation:** server-side rules, allowlists, difference between validating and sanitizing. **Login throttling** (CR-02). The OWASP Top 10 at a high level |
| **Skills to practice** | Writing a reusable validator; building middleware that rejects unauthorized requests; generating and verifying CSRF tokens; escaping consistently with one helper; attacking your own forms |
| **Practice task** | Build login and logout with two test users and two roles. Then try: visiting an admin URL while logged out, changing a role in a form, submitting without a CSRF token, and entering `<script>alert(1)</script>` in a text field. Fix everything that works |
| **You are ready when you can…** | Explain what each cookie flag prevents; show a failed direct-URL access as a logged-out user; explain why hiding a button is not security; point to the single place every protected route checks permission |
| **Common pitfalls** | Checking permission only in the interface; storing passwords or tokens in plain text; trusting hidden form fields; forgetting to escape output in "internal" admin pages; regenerating the session ID only sometimes |

### WBS 6: Administrative Dashboard and Content Management (46 h)

**What you will do:** admin layout, event, news, and project management, image uploads, club settings, enquiries, and optional account management.

| | |
| :--- | :--- |
| **Know first** | CRUD patterns; the Post-Redirect-Get pattern; flash messages; pagination; state transitions (Draft, Published, Archived; SRS §5.1); soft delete; date and time handling and time zones (Zambia uses Central Africa Time, UTC+2); database transactions; **file upload security:** extension and type allowlists, `finfo` MIME checks, `getimagesize()`, size limits, generated filenames, storage outside executable paths (CR-01); image resizing with GD; handling rich text (limit to plain text or sanitize); **insecure direct object references (IDOR)** |
| **Skills to practice** | Reusing one CRUD pattern across modules; returning validation errors without losing user input; testing every action as each role; safe file handling |
| **Practice task** | Build **Events** completely first (list, create, edit, publish, unpublish, archive, permanent delete for Site Administrator only). Treat it as the template, then copy the pattern to News and Projects |
| **You are ready when you can…** | Show that a Content Manager cannot permanently delete; show that a Draft event is not reachable at its public URL; upload a `.php` file renamed to `.jpg` and see it rejected |
| **Common pitfalls** | Building three modules in parallel instead of finishing one; forgetting object-level checks (IDOR); trusting the file extension; losing form data on a validation error; showing timestamps in the wrong time zone |

### WBS 7: Public Website (30 h)

**What you will do:** shared layout, homepage, About, events, news, projects, and the contact form.

| | |
| :--- | :--- |
| **Know first** | Semantic HTML5 (`header`, `nav`, `main`, `article`, `footer`); CSS Flexbox and Grid; mobile-first design and media queries; CSS custom properties for club branding (FR-017); JavaScript basics and progressive enhancement; PHP templating with partials; image optimization (`srcset`, lazy loading, modern formats); accessibility basics: labels, alt text, focus order, skip links, contrast (NFR-012); empty states; contact-form abuse controls (honeypot field, rate limit, CSRF) |
| **Skills to practice** | Building at 360 px first, then enlarging; navigating with the keyboard only; reading Lighthouse and WAVE reports; writing meaningful alt text |
| **Practice task** | Build the homepage and Events page at 360 px width with real or placeholder content. Check them with keyboard-only navigation and a Lighthouse run |
| **You are ready when you can…** | Show no horizontal scrolling at 360 px; tab through every page in a logical order; explain why each image has the alt text it has |
| **Common pitfalls** | Designing for desktop first; relying on JavaScript for essential features; low-contrast text; huge images hurting load time (NFR-013) |

### WBS 8: Content, Branding and Consent (12 h)

**What you will do:** collect launch content, verify branding, secure rights to publish photos and stories.

| | |
| :--- | :--- |
| **Know first** | Basics of copyright and image rights; why consent matters for photos and personal stories; data privacy fundamentals (check the Zambia Data Protection Act of 2021 and ask someone qualified, such as your institution or club advisers; this guide is not legal advice); writing for the web; brand consistency |
| **Skills to practice** | Making clear, small content requests; keeping a content tracker (item, owner, rights confirmed, status); writing short, plain-language page copy and alt text |
| **Practice task** | Create a one-page content checklist for the Public Image Committee listing each item, who supplies it, and whether publication rights are confirmed (OI-07) |
| **You are ready when you can…** | Show, for any published photo or statistic, who approved it |
| **Common pitfalls** | Publishing photos of people without consent; waiting until the last week for content; using unapproved logos or colours |

### WBS 9: Testing and Quality Assurance (30 h)

**What you will do:** write and run test cases, security tests, browser and accessibility checks, and user acceptance testing.

| | |
| :--- | :--- |
| **Know first** | Test design (equivalence classes, boundary values); mapping tests to requirements (RTM-001 §4); manual security testing (SQL injection, XSS, CSRF, session handling, access-control bypass, IDOR); unit testing basics, optionally PHPUnit for validators and permission rules; browser developer tools and device emulation; Lighthouse, WAVE, or axe; defect reporting; regression testing; running a user acceptance session |
| **Skills to practice** | Writing a test before or beside each feature; using a checklist for each role; recording evidence (screenshots, notes); reproducing and describing defects precisely |
| **Practice task** | For FR-004, write the test case TC-EVT-002 and TC-SEC-001 steps for all four roles and an unauthenticated user, run them, and record pass or fail with evidence |
| **You are ready when you can…** | Show a passing test with evidence for every Must requirement; explain what each TC-SEC test proves; log a defect that someone else could reproduce |
| **Common pitfalls** | Leaving all testing to the final week; testing only the "happy path"; testing only in one browser; forgetting to retest after fixes |

### WBS 10: Hosting, Deployment and Operations (18 h)

**What you will do:** pick and set up hosting and domain, configure production, deploy, back up, and smoke test.

| | |
| :--- | :--- |
| **Know first** | DNS basics (A records, nameservers, propagation); domain registration and ownership; shared hosting and control panels; SFTP; PHP version and settings on the host; file permissions; Apache `.htaccess` basics (denying access, rewrites); HTTPS and Let's Encrypt; production versus development configuration (errors off, debug off); database export and import (`mysqldump` or phpMyAdmin); backup and restore testing (NFR-014); smoke testing |
| **Skills to practice** | Comparing providers against the ADR-011 checklist; deploying a test version before the real one; running a full backup-and-restore drill; reading server error logs |
| **Practice task** | Deploy the Hello app from WBS 4 to a free or cheap trial host. Confirm HTTPS works, secrets are not web-accessible, errors are hidden, then take a database backup and restore it into a fresh database |
| **You are ready when you can…** | Prove no source files, `.env`, or logs can be requested in a browser; restore a backup without help; explain what to do if the host cannot set `public/` as the web root (ADR-008 fallback) |
| **Common pitfalls** | Buying hosting before checking PHP 8.x, PDO, backups, and document-root options; accounts registered only to you personally; never testing a restore |

### WBS 11: Documentation and Training (14 h)

| | |
| :--- | :--- |
| **Know first** | Writing for different readers (administrators versus developers); Markdown; step-by-step instructions with screenshots; runbooks; simple diagrams; running a short hands-on training session |
| **Skills to practice** | Writing steps that start with a verb and have one action each; testing instructions on someone who has never seen the system; making a one-page quick reference |
| **Practice task** | Write the "Publish a news article" guide, then ask a non-technical friend to follow it without your help and note every place they get stuck |
| **You are ready when you can…** | Hand a new maintainer the README and runbook and have them set up, deploy, back up, and restore on their own |
| **Common pitfalls** | Writing documentation last, from memory; assuming the reader knows technical terms |

### WBS 12: Launch, Handover and Closure (8 h)

| | |
| :--- | :--- |
| **Know first** | Release checklists; rollback planning; first-week monitoring; knowledge transfer; credential hygiene (individual accounts, a password manager, no shared logins); retrospectives |
| **Skills to practice** | Running a go-live checklist calmly; transferring ownership cleanly; writing down what you learned |
| **Practice task** | Prepare a one-page launch checklist and a rollback note (how to restore the last backup and put the old site back) before launch day |
| **You are ready when you can…** | Show that a named maintainer exists (OI-08), the club owns the domain and hosting, and your own access has been reduced to what is needed |
| **Common pitfalls** | Launching without a rollback plan; keeping the only copy of credentials; no named owner after you leave |

---

## 5. Suggested Learning Plan Aligned to the Schedule

Learn just in time, in step with the milestones in WBS-001 §5. The hours are *inside* the work packages, not extra.

| Week | Dates | Build focus (WBS) | Learn first |
| :---: | :--- | :--- | :--- |
| 1 | 19 to 25 Oct | 1, 2, 3 (start) | Requirements writing, HTTP basics, SQL refresh, ERD design, Git workflow |
| 2 | 26 Oct to 1 Nov | 3, 4, 5 (start) | PHP OOP and namespaces, PDO, routing and front controller, `.env`, logging |
| 3 | 2 to 8 Nov | 5, 6 (start) | Sessions and cookies, password hashing, CSRF, XSS escaping, RBAC |
| 4 | 9 to 15 Nov | 6, 7 (start) | CRUD and PRG patterns, file uploads, time zones, semantic HTML and CSS layout |
| 5 | 16 to 22 Nov | 7, 8, 9 (start), 10.1 to 10.3 | Accessibility checks, Lighthouse, security testing, DNS, hosting setup, HTTPS |
| 6 | 23 to 29 Nov | 9, 10, 11, 12 | Backup and restore, deployment, user testing, writing guides, launch checklist |

**Protect your learning time:** the plan is tight (about 256 hours over six weeks). If you fall behind, descope according to WBS-001 §8 rather than skipping security learning or testing.

---

## 6. Priorities: Where to Spend Learning Effort

| Priority | Learn deeply (target level 3) | Learn enough to work (level 2) | Just know it exists (level 1) |
| :--- | :--- | :--- | :--- |
| Skills | Sessions and authentication; authorization and IDOR; SQL injection prevention with PDO; XSS escaping; CSRF; secure uploads | PHP structure and routing; CRUD patterns; responsive CSS; accessibility; testing; deployment and backups; requirements writing | Frameworks (for example Laravel, see ADR-001); CI pipelines; PHPUnit beyond basics; multi-tenancy; caching; JavaScript frameworks |

---

## 7. Reference Sources

Prefer official and well-maintained references.

| Topic | Source |
| :--- | :--- |
| PHP language, PDO, sessions, `password_hash()` | PHP Manual (php.net); "PHP: The Right Way" |
| HTML, CSS, JavaScript, accessibility basics | MDN Web Docs (developer.mozilla.org) |
| MySQL and SQL | MySQL Reference Manual (dev.mysql.com) |
| Web security | OWASP Top 10 and OWASP Cheat Sheet Series (owasp.org) |
| Accessibility standards | W3C WAI: WCAG 2.2 and "How to Meet WCAG" quick reference (w3.org/WAI) |
| Performance | web.dev (Lighthouse and Core Web Vitals) |
| Git | *Pro Git* book (git-scm.com) |
| HTTPS | Let's Encrypt documentation (letsencrypt.org) |
| Requirements engineering | ISO/IEC/IEEE 29148, or your course notes on requirements |

---

## 8. Self-Assessment Checklist

Rate yourself 1 (Aware), 2 (Working), or 3 (Proficient) at the start and at the end of each phase. The goal is steady progress, not perfection.

| Skill | Target | Start | Mid-project | End |
| :--- | :---: | :---: | :---: | :---: |
| Writing testable requirements and acceptance criteria | 2 | | | |
| Drawing an ERD and designing tables | 2 | | | |
| Git branching and keeping secrets out of the repository | 2 | | | |
| PHP OOP, namespaces, routing, configuration | 2 | | | |
| PDO prepared statements and transactions | 3 | | | |
| Session security and authentication | 3 | | | |
| Authorization and IDOR prevention | 3 | | | |
| CSRF and XSS defence | 3 | | | |
| Secure file uploads | 3 | | | |
| CRUD with state handling and validation | 2 | | | |
| Responsive HTML and CSS | 2 | | | |
| Accessibility checks | 2 | | | |
| Writing and running test cases | 2 | | | |
| DNS, hosting, HTTPS | 2 | | | |
| Backup and restore | 2 | | | |
| Technical and user documentation | 2 | | | |
| Stakeholder communication | 2 | | | |

## 9. Revision Log

| Version | Date | Description | Author |
| :--- | :--- | :--- | :--- |
| 1.0 | 18 October 2026 | Initial skills and knowledge guide aligned to the WBS, SRS, and SAD | Project Developer |
