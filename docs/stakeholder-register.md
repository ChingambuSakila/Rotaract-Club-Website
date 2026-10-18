# Stakeholder Register --- Rotaract Club Website

**Project:** Rotaract Club of UNZA Website\
**Document ID:** STK-001\
**Version:** 1.1\
**Status:** Draft for stakeholder confirmation\
**Last updated:** 18 October 2026\
**Target launch:** 30 November 2026\
**Owner:** Project Developer

## 1. Purpose

This register identifies people and groups who influence, use, maintain,
or are affected by the website. It records their interests, expected
involvement, and the approach for eliciting and validating requirements.

The developer is building the first release independently. Stakeholder
availability is expected to be limited, so reviews should be focused and
time-boxed.

## 2. Stakeholder Register

  ----------------------------------------------------------------------------------------------------------
  ID          Stakeholder /      Role and         Influence     Engagement approach Requirements / decisions
              group              interest                                           needed
  ----------- ------------------ ---------------- ------------- ------------------- ------------------------
  ST-01       Club President     Accountable club High          One focused review  Confirm purpose, launch
                                 leader; wants a                of project scope    scope, public
                                 professional                   and public-facing   information, and who may
                                 online presence                content; request    administer content.
                                 and accurate                   decisions in        
                                 representation                 writing where       
                                 of the club.                   possible.           

  ST-02       Club Board Members Club governance  High          Short review of     Confirm which board
                                 and operational                role permissions    roles can manage events,
                                 leadership; may                and content         news, projects, club
                                 need to update                 responsibilities.   details, and
                                 or oversee club                                    administrator accounts.
                                 information.                                       

  ST-03       Public Image       Likely           High for      Request a list of   Confirm content
              Committee members  contributors and content       content types,      responsibilities, image
                                 managers of      workflow      required fields,    usage, required fields,
                                 news,                          branding assets,    and whether publishing
                                 photographs,                   and publishing      may occur immediately.
                                 announcements,                 expectations.       
                                 and                                                
                                 public-facing                                      
                                 content.                                           

  ST-04       Club Secretary /   May hold         Medium to     Request verified    Confirm content
              designated         accurate records high          event details,      accuracy, who supplies
              information owner  of events,                     leadership          updates, and how
                                 leadership, and                information, and    outdated information
                                 club                           official club       should be handled.
                                 information.                   details.            

  ST-05       Club members       May browse       Medium        Optional short      Identify usability
                                 public content                 questionnaire or    issues and future
                                 and later                      informal review; no member-account needs.
                                 benefit from                   member portal in    
                                 member-only                    the first release.  
                                 features.                                          

  ST-06       Prospective        Need to          Medium        Review the public   Confirm whether
              members            understand the                 pages and           membership information
                                 club and how to                contact/enquiry     and a membership enquiry
                                 make an enquiry.               flow with a small   category are needed.
                                                                number of           
                                                                representative      
                                                                users if feasible.  

  ST-07       General public /   Need reliable    Medium        Validate clarity of Confirm which project
              community partners information                    public content and  outcomes and public
                                 about projects,                contact details.    contact information may
                                 events, and                                        be published.
                                 community                                          
                                 impact.                                            

  ST-08       Website            Maintain website High          Walk through        Confirm login, account
              administrator(s)   content and      operational   proposed            creation/deactivation,
                                 access the       influence     administration      content management,
                                 dashboard.                     tasks and           backups, and recovery
                                                                permission          expectations.
                                                                boundaries.         

  ST-09       Project developer  Elicits,         High          Maintain            Resolve technical
                                 specifies,       technical     requirements        feasibility, security,
                                 designs,         influence     register, decision  architecture, and
                                 implements,                    log, traceability   schedule risks.
                                 tests, deploys,                matrix, and release 
                                 and documents                  checklist.          
                                 the website.                                       

  ST-10       Hosting/domain     Provides         Medium        Compare cost,       Confirm hosting
              provider (to be    infrastructure                 PHP/MySQL support,  capabilities, domain
              selected)          for the public                 HTTPS, backups, and availability, renewal
                                 website and                    support before      costs, backup options,
                                 database.                      selection.          and deployment
                                                                                    constraints.
  ----------------------------------------------------------------------------------------------------------

## 3. Confirmed Project Context

-   The first website is for the **Rotaract Club of UNZA**.
-   The code should be reusable for separate club websites in the
    future.
-   The first release is not a multi-tenant platform.
-   Public visitors and registered club members may use the
    public-facing site, but member accounts are deferred.
-   Board members and Public Image Committee members are intended to
    manage website content; exact permissions still require
    confirmation.
-   Authorized content managers may publish directly; an approval
    workflow is not planned for the first release.
-   The developer is working alone, with limited club-member input.
-   The target launch date is **30 November 2026**.
-   The hosting and domain budget is not yet known.

## 4. Engagement Plan

1.  Request official logo files, brand colours, club contact details,
    current leadership information, social links, and approved project
    content.
2.  Ask the President or a delegated board member to review the scope
    and public content.
3.  Ask the Public Image Committee to confirm the content types and
    day-to-day publishing needs.
4.  Ask the relevant board representative to approve the administrator
    permission model.
5.  Record unanswered questions in the open-issues register rather than
    silently treating assumptions as confirmed.
6.  Obtain written approval of the SRS baseline before implementation of
    the full feature set.

Because stakeholder availability is limited, use one consolidated review
document and a short decision meeting or written response rather than
scheduling many separate meetings.

## 5. Recommended Decisions for Stakeholder Review

These recommendations reduce ambiguity and keep the first release achievable by one developer before the 30 November 2026 target. They are proposals, not approvals.

1. **Administrator permissions:** Site Administrator (President plus a named successor) controls admin accounts and global settings; Content Managers from the Board/PIC publish and manage public content; Enquiry Manager (Secretary/designated contact) handles contact submissions. Individual accounts only; no public admin registration or shared passwords.
2. **Hosting/budget:** managed shared PHP/MySQL hosting for the first year; compare at least two providers; plan around US$50–100 for domain and hosting combined, excluding paid email and extras. Confirm renewal pricing before payment and ensure the club owns the accounts.
3. **Content management:** events require title, date/time, location, short description; news requires title and body; projects require title, description, date/period. Keep impact measures and images optional. Publish directly for authorized users; prefer archive/soft-delete over permanent deletion.
4. **Enquiries:** save every enquiry to the protected dashboard; send an email notification only if SMTP is configured and tested. A proposed starting retention period is 12 months after closure, subject to club approval.

## 6. Open Stakeholder Questions

| ID | Question / decision | Owner to consult | Status |
|---|---|---|---|
| OQ-01 | Approve proposed Site Administrator, Content Manager, and Enquiry Manager roles and role boundaries | President / Board | Proposed for approval |
| OQ-02 | Confirm named Site Administrators and whether account-management UI is needed for launch | President / Board | Proposed for approval |
| OQ-03 | Should contact enquiries be stored in the dashboard only, or also trigger tested email notifications? | Board / designated contact | Open |
| OQ-04 | Approve archive/soft-delete default and permanent-deletion rights | Board / Public Image Committee | Proposed for approval |
| OQ-05 | Approve recommended required/optional event, news, and project fields | Public Image Committee / Secretary | Proposed for approval |
| OQ-06 | Select domain/hosting provider and approve upfront and annual renewal price | President / Treasurer / Developer | Open |
| OQ-07 | Are supplied photographs, stories, and impact figures accurate and approved for public use? | Public Image Committee / content owner | Open |
| OQ-08 | Who will maintain the site and its content after the developer's initial delivery? | Board | Open |

## 7. Approval

This register identifies expected stakeholders and engagement needs. It
does not mean that each stakeholder has approved the requirements.

  Approval role                           Name              Decision / date
  --------------------------------------- ----------------- -----------------
  Club representative                     To be confirmed   Pending
  Public Image Committee representative   To be confirmed   Pending
  Project developer                       To be completed   Pending
