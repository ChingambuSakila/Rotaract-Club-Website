# Stakeholder Register --- Rotaract Club Website

**Project:** Rotaract Club of UNZA Website\
**Document ID:** STK-001\
**Version:** 1.0\
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

## 5. Open Stakeholder Questions

  ------------------------------------------------------------------------------
  ID                Question                 Owner to consult  Status
  ----------------- ------------------------ ----------------- -----------------
  OQ-01             Which board roles may    President / Board Open
                    create, edit, publish,                     
                    archive, and delete                        
                    content?                                   

  OQ-02             Who can create, disable, President / Board Open
                    or reset administrator                     
                    accounts?                                  

  OQ-03             Should contact enquiries Board /           Open
                    be stored only in the    designated        
                    dashboard, or should     contact           
                    email notifications also                   
                    be sent?                                   

  OQ-04             Should content be        Board / Public    Open
                    permanently deleted or   Image Committee   
                    archived/soft-deleted?                     

  OQ-05             What fields are          Public Image      Open
                    mandatory for events,    Committee /       
                    news, and service        Secretary         
                    project reports?                           

  OQ-06             What domain and hosting  President /       Open
                    budget is available?     Treasurer         

  OQ-07             Are all supplied         Public Image      Open
                    photographs and stories  Committee /       
                    approved for public use? content owner     

  OQ-08             Who will maintain the    Board             Open
                    site after the                             
                    developer's initial                        
                    delivery?                                  
  ------------------------------------------------------------------------------

## 6. Approval

This register identifies expected stakeholders and engagement needs. It
does not mean that each stakeholder has approved the requirements.

  Approval role                           Name              Decision / date
  --------------------------------------- ----------------- -----------------
  Club representative                     To be confirmed   Pending
  Public Image Committee representative   To be confirmed   Pending
  Project developer                       To be completed   Pending
