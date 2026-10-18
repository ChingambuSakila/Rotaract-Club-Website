# Requirements Traceability Matrix

**Project:** Rotaract Club of UNZA Website\
**Document ID:** RTM-001\
**Version:** 1.1\
**Status:** Initial baseline v1.1 --- recommendations added; validate before implementation\
**Last updated:** 18 October 2026

## 1. Purpose

This matrix links requirements to their stakeholder need, planned
implementation area, and validation evidence. It helps ensure that each
requirement is addressed and that each implemented feature has a reason
to exist.

Implementation paths and test IDs are proposed and should be updated to
match the final codebase and test suite.

## 2. Traceability Matrix

  -------------------------------------------------------------------------------------------------------------------------------------------------------------
  Stakeholder need Requirement   Requirement summary                    Priority       Planned component               Planned validation evidence  Status
  / source         ID                                                                                                                               
  ---------------- ------------- -------------------------------------- -------------- ------------------------------- ---------------------------- -----------
  Professional     FR-001        Homepage with club identity and        Must           Public homepage / views         TC-PUB-001: homepage content Not started
  public presence                featured content                                                                      and navigation check         

  Visitors need    FR-002        About the Club page                    Must           Public pages / views            TC-PUB-002: page             Not started
  club information                                                                                                     availability and content     
                                                                                                                       check                        

  Visitors need    FR-003        List published events                  Must           Events module                   TC-EVT-001: listing,         Not started
  event                                                                                                                ordering, empty state,       
  information                                                                                                          unpublished event check      

  Club needs to    FR-004        Create/edit/publish/unpublish/delete   Must           Event                           TC-EVT-002: CRUD and         Not started
  maintain event                 events                                                controller/service/repository   validation; TC-SEC-001:      
  details                                                                              and admin views                 authorization                

  Public needs     FR-005        Display published news                 Must           News module                     TC-NEWS-001:                 Not started
  current                                                                                                              published/unpublished        
  announcements                                                                                                        visibility check             

  Club needs to    FR-006        Manage news articles                   Must           News management                 TC-NEWS-002:                 Not started
  maintain                                                                                                             create/edit/publish/delete   
  announcements                                                                                                        and validation               

  Public needs to  FR-007        Display service projects and impact    Must           Projects module                 TC-PRJ-001: project listing  Not started
  understand                     reports                                                                               and detail check             
  service impact                                                                                                                                    

  Club needs to    FR-008        Manage project content                 Must           Project management              TC-PRJ-002:                  Not started
  maintain project                                                                                                     create/edit/publish and      
  records                                                                                                              authorization                

  Visitors need to FR-009        Public contact form                    Must           Contact page and request        TC-CON-001: valid/invalid    Not started
  contact the club                                                                     handler                         input and spam control       

  Club needs to    FR-010        Store and protect submitted enquiries  Must           Enquiry persistence and admin   TC-CON-002: persistence,     Not started
  review enquiries                                                                     view                            access restriction, error    
                                                                                                                       handling                     

  Administrators   FR-011        Login and logout                       Must           Authentication service and      TC-AUTH-001: valid/invalid   Not started
  need protected                                                                       login views                     login and logout/session     
  access                                                                                                               invalidation                 

  Protected        FR-012        Server-side authorization              Must           Authorization                   TC-SEC-001: unauthenticated  Not started
  operations must                                                                      middleware/helpers/policies     and unauthorized request     
  be restricted                                                                                                        tests                        

  Administrators   FR-013        Administrative dashboard               Must           Admin layout and dashboard      TC-ADM-001: dashboard access Not started
  need a                                                                                                               and navigation               
  management                                                                                                                                        
  interface                                                                                                                                         

  Content must be  FR-014        Dashboard content management           Must           Admin forms and content modules TC-ADM-002: update content   Not started
  managed without                                                                                                      through UI and verify public 
  code edits                                                                                                           change                       

  Club information FR-015        Update agreed club details             Must           Settings/content module         TC-SET-001: update approved  Not started
  needs                                                                                                                fields and verify display    
  maintenance                                                                                                                                       

  Controlled       FR-016        Create/disable/reset admin accounts    Should /       Account administration          TC-ADM-003: role restriction Pending
  administrator                                                         pending                                        and disabled-account tests   decision
  management may                                                        confirmation                                                                
  be needed                                                                                                                                         

  Future reuse for FR-017        Keep club identity configurable        Should         Configuration/settings          TC-ARCH-001: change test     Not started
  other clubs                                                                                                          configuration without        
                                                                                                                       core-code rewrite            

  Member portal is FR-018        No member accounts in release one      Must / scope   Scope and route design          TC-SCOPE-001: confirm no     Confirmed
  deferred                                                              constraint                                     public registration or       scope
                                                                                                                       member-only dependency       

  Responsive       NFR-001       Usable at representative viewport      Must           CSS/layout components           TC-UI-001: 360px, 768px,     Not started
  experience                     widths                                                                                1280px viewport checks       

  Consistent,      NFR-002       Navigation and validation usability    Must           Shared layout and forms         TC-UX-001: navigation and    Not started
  understandable                                                                                                       form usability review        
  interface                                                                                                                                         

  Safe SQL         NFR-003       PDO prepared statements for untrusted  Must           Data access layer               TC-SEC-002: code review and  Not started
  execution                      values                                                                                SQL injection tests          

  Secure password  NFR-004       Password hashing and verification      Must           Authentication service          TC-SEC-003: inspect stored   Not started
  storage                                                                                                              hashes and login             
                                                                                                                       verification                 

  Reliable input   NFR-005       Server-side validation                 Must           Request handlers/services       TC-VAL-001: boundary,        Not started
  handling                                                                                                             missing, malformed input     
                                                                                                                       tests                        

  Prevent          NFR-006       CSRF protection for state-changing     Must           Forms/request handlers          TC-SEC-004: missing/invalid  Not started
  cross-site                     forms                                                                                 token tests                  
  request forgery                                                                                                                                   

  Prevent          NFR-007       Contextual output escaping and         Must           Views/content rendering         TC-SEC-005: stored/reflected Not started
  cross-site                     rich-text sanitization if applicable                                                  XSS test cases               
  scripting                                                                                                                                         

  Protect sessions NFR-008       Secure sessions and protected routes   Must           Authentication and deployment   TC-SEC-006: session          Not started
  and routes                                                                           configuration                   fixation, cookie flags,      
                                                                                                                       direct URL tests             

  Protect secrets  NFR-009       No secrets in public files or version  Must           Configuration and deployment    TC-SEC-007: repository and   Not started
                                 control                                                                               deployment configuration     
                                                                                                                       review                       

  Maintainable     NFR-010       Separate presentation, logic, and data Must           Project structure               TC-ARCH-002: code review and Not started
  application                    access                                                                                setup documentation check    

  Browser/device   NFR-011       Test current browsers and              Must           Whole application               TC-COMP-001: documented      Not started
  compatibility                  representative devices                                                                browser/device checklist     

  Accessibility    NFR-012       Design/test against WCAG 2.2 AA where  Should         Whole interface                 TC-A11Y-001: keyboard,       Not started
                                 applicable                                                                            labels, contrast, semantics, 
                                                                                                                       alt-text review              

  Performance      NFR-013       Measure representative pages and       Should         Whole application / assets      TC-PERF-001: documented lab  Not started
                                 document results                                                                      measurements                 

  Recovery from    NFR-014       Backup and tested restore process      Must           Database/deployment operations  TC-OPS-001: restore a backup Not started
  data loss                                                                                                            in a test environment        

  Safe error       NFR-015       No sensitive internal details in       Must           Error handling and logging      TC-SEC-008: trigger          Not started
  handling                       production errors                                                                     controlled errors and        
                                                                                                                       inspect response/logs        
  -------------------------------------------------------------------------------------------------------------------------------------------------------------

## 3. Recommended Baseline Decisions to Validate

| Area | Recommended baseline | Validation evidence |
|---|---|---|
| Administrator permissions | Least privilege; individual accounts; Site Administrator manages accounts/settings; Content Managers publish public content; Enquiry Manager handles contact enquiries | Written confirmation of role holders and permissions; tests for permitted and denied actions |
| Hosting and budget | Managed shared PHP/MySQL hosting; compare at least two providers; US$50–100 planning envelope for domain + hosting in year one, excluding paid email/extras | Quote comparison includes upfront and renewal prices, SSL, backups, PHP/MySQL compatibility, and club ownership |
| Content fields | Minimal required fields for events/news/projects; optional impact measures and images; direct publishing by authorized users | Public Image Committee and Secretary review forms and sample content |
| Deletion and enquiries | Prefer archive/soft-delete; permanent deletion restricted to Site Administrators; save enquiries in dashboard; email notification only after SMTP test; proposed 12-month retention after closure | Tests for role restrictions, archive behavior, enquiry access, and approved retention policy |

These are proposed defaults to make implementation concrete. They must
not be marked accepted until the club representative confirms them.

## 4. Verification and Validation Rules

-   **Verification** checks whether implementation and requirements
    conform to their specifications.
-   **Validation** checks whether the requirements and delivered website
    meet stakeholder needs.
-   Every Must requirement should have at least one test or inspection
    before release.
-   A failed test should be linked to a defect or a formally accepted
    exception.
-   Requirements changed after approval should be recorded with a date,
    reason, and impact on schedule, design, and tests.
-   Do not mark a requirement complete solely because code exists;
    record test evidence.

## 5. Status Values

Use these status values consistently:

-   **Not started:** No implementation work has begun.
-   **In progress:** Work is underway.
-   **Implemented:** Code is present but not yet fully validated.
-   **Tested:** Required test evidence has passed.
-   **Accepted:** The relevant acceptance criteria have been met and,
    where needed, the stakeholder has accepted the result.
-   **Deferred:** Explicitly postponed with an owner and reason.
-   **Pending decision:** Work depends on an unresolved stakeholder or
    design decision.

## 6. Change Log

  -----------------------------------------------------------------------
  Version                 Date                    Change
  ----------------------- ----------------------- -----------------------
  1.0                     18 October 2026         Initial traceability
                                                  baseline created from
                                                  the draft SRS.
  1.1                     18 October 2026         Added proposed role,
                                                  hosting/budget, and
                                                  content-management
                                                  baselines for review.

  -----------------------------------------------------------------------
