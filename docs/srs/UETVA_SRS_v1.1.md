**UGANDA ELECTION TALLYING\
AND VISUALISATION APPLICATION**

**SOFTWARE REQUIREMENTS SPECIFICATION**

Comprehensive functional, data, visualisation, security and operational
requirements\
Uganda • All election categories • Results lifecycle • Public
transparency\
\
Version 1.1 \| 5 November 2026\
Draft for stakeholder, Electoral Commission and legal validation

# Document control

  -----------------------------------------------------------------------
  **Field**                           **Value**
  ----------------------------------- -----------------------------------
  Title                               Uganda Election Tallying and
                                      Visualisation Application --
                                      Software Requirements Specification

  Version/date                        1.1 / 5 November 2026

  Status                              Comprehensive baseline draft for
                                      review; not an approved EC system
                                      specification

  Audience                            EC stakeholders, election
                                      operations, legal counsel, ICT
                                      architects, developers, testers,
                                      cybersecurity, auditors, observers
                                      and implementation partners

  Owner / approver                    To be assigned by the sponsoring
                                      authority

  Change control                      All changes to election rules,
                                      forms, workflows, calculations,
                                      data access and publication require
                                      versioning, impact assessment and
                                      approval.
  -----------------------------------------------------------------------

## Revision history

| Version | Date | Summary |
|---|---|---|
| 1.0 | 4 Nov 2026 | Baseline draft. |
| 1.1 | 5 Nov 2026 | Review of the EC *Laws and Regulations* page and 2025/2026 EC publications. Adds: line-up (queue) voting method and Forms EC10/EC11 (§5.3, LIN-001–006); Army Council method for UPDF; chain of custody from polling station through sub-county results centre to district tally centre (CUS-001–008); tally-centre public display and ERTDS/BVVK integration (§10.1); 2025/2026 scale baseline (§2.5); additional legal sources (§3.2); new open decisions (§18.2). Changed text is marked **[v1.1]**. |

## Critical authority statement

This application supports authorised election administration, results
capture, verification, aggregation, reporting and public information. It
does not transfer, replace or diminish the constitutional or statutory
authority of the Electoral Commission (EC), returning officers,
presiding officers, courts or any other competent body. The software
must not independently declare or certify winners. Formal declaration,
certification and publication remain with the legally authorised
authority and must follow applicable law.

No production deployment should proceed until the EC and legal counsel
validate current legislation, election procedures, official forms,
signatures, arithmetic rules, election methods, approval chains,
publication policy, retention requirements and authoritative electoral
geography.

# Contents

-   1\. Executive summary and principles

-   2\. Purpose, scope and objectives

-   3\. Legal and policy baseline

-   4\. Stakeholders and access roles

-   5\. Election coverage and configurable taxonomy

-   6\. End-to-end business workflows

-   7\. Functional requirements

-   8\. Election visualisation requirements

-   9\. Data model and data dictionary

-   10\. Interfaces and integrations

-   11\. Security, privacy and integrity

-   12\. Non-functional requirements

-   13\. Offline operation and synchronisation

-   14\. Reporting and transparency

-   15\. Deployment and operations

-   16\. Testing and acceptance

-   17\. Governance and RACI

-   18\. Risks, assumptions and open decisions

-   19\. Traceability matrix

-   20\. Glossary and source references

# 1. Executive summary

The Uganda Election Tallying and Visualisation Application (UETVA) is a
proposed secure, auditable, configurable platform for managing election
results from source-event capture through verification, exception
resolution, aggregation, controlled release and public visualisation. It
covers the full result lifecycle and supports both direct polling and
legally prescribed representative processes such as conferences,
electoral colleges and council-based elections.

The product must maintain a defensible chain of custody from the source
result form or authorised record to each displayed aggregate. It must
separate data receipt, automated validation, human verification,
approval for release, official declaration and certification. These are
distinct states and must never be conflated.

## 1.1 Outcomes

-   Reduce transcription and aggregation errors without replacing source
    records.

-   Trace every result value to source evidence, event, user, device,
    time and rule version.

-   Enable secure offline capture and conflict-safe synchronisation.

-   Make exceptions visible and resolvable without silent edits.

-   Provide neutral, accessible dashboards, maps, charts and public data
    access.

-   Preserve complete records for audit, legal review, correction and
    post-election analysis.

## 1.2 Core design principles

  -----------------------------------------------------------------------
  **Principle**                       **System interpretation**
  ----------------------------------- -----------------------------------
  Authority first                     Software supports the lawful
                                      process; statutory decisions remain
                                      with authorised officers.

  Evidence-led                        Every value has provenance and
                                      source evidence or an approved
                                      source reference.

  No silent mutation                  Corrections create new versions and
                                      preserve the original.

  Configurable by law                 Election method, form, electorate,
                                      formula, geography and workflow are
                                      versioned configuration.

  Offline-resilient                   Connectivity loss does not destroy
                                      or duplicate captured records.

  Neutral presentation                No partisan endorsement, prediction
                                      or unexplained ranking.

  Least privilege                     Access is restricted by role,
                                      contest, geography, task and time.
  -----------------------------------------------------------------------

# 2. Purpose, scope and objectives

## 2.1 Purpose

Specify the functional, data, interface, security, performance and
operational requirements for procurement, architecture, implementation,
testing, deployment, operation and maintenance.

## 2.2 In scope

-   Election-cycle and contest configuration, legal rule packs,
    candidate/party/option records.

-   Versioned electoral geography, polling-station and non-station
    voting-event registry.

-   Results capture, evidence images/documents, source-form versioning
    and metadata.

-   Automated validation, independent verification, dual entry,
    exception and correction workflows.

-   Secure transmission, offline synchronisation, acknowledgement and
    reconciliation.

-   Aggregation/tallying at configured levels, release approvals and
    publication history.

-   Internal command dashboards, tally-centre workspace, public portal,
    maps, charts, exports and APIs.

-   Identity, role-based access, audit, cybersecurity, privacy, backup,
    DR, monitoring and records management.

## 2.3 Out of scope unless separately authorised

-   Voter registration, voter identity verification, biometric
    processing or management of the National Voters Register.

-   Electronic voting, ballot design/printing, ballot-box management or
    replacement of legally prescribed paper processes.

-   Campaign finance, political advertising, voter persuasion, campaign
    optimisation or election forecasting.

-   Legal adjudication of petitions, determination of candidate
    eligibility or independent ordering of recounts.

-   Collection of individual voter choices or linking a person to a
    ballot selection.

-   Automatic winner declaration without the competent authority\'s
    legally valid approval and declaration.

## 2.4 Proposed measurable targets

  -----------------------------------------------------------------------
  **Area**                            **Proposed baseline target (confirm
                                      during sizing)**
  ----------------------------------- -----------------------------------
  Traceability                        100% of result changes,
                                      verification, approvals,
                                      publication, withdrawal and export
                                      events audited.

  Availability                        99.9% monthly central-service
                                      availability during declared
                                      critical election windows.

  Web performance                     95th percentile ≤3 seconds for
                                      standard authenticated screens
                                      under agreed reference load.

  Submission acknowledgement          ≤10 seconds for normal online
                                      structured submissions, excluding
                                      large evidence uploads.

  Scale assumption                    Capacity plan initially modelled
                                      for 50,000 concurrent public
                                      sessions and 10,000 authenticated
                                      users; validate against EC volumes.

  Recovery                            Proposed result database RPO ≤5
                                      minutes and RTO ≤60 minutes;
                                      confirm by business impact
                                      analysis.

  Accessibility                       WCAG 2.2 AA target for public and
                                      applicable internal interfaces.
  -----------------------------------------------------------------------

## 2.5 Scale baseline from EC 2025/2026 publications [v1.1]

Source: EC Annual Bulletin 2025–2026 and EC press statement of 22 July 2026. Figures are planning inputs and must be reconfirmed per cycle (D-10).

| Item | 2025/2026 figure |
|---|---|
| Registered voters | 21,681,491 |
| Polling stations | 50,739 |
| Districts/cities | 146 |
| Constituencies | 353 |
| Municipalities/city divisions | 56 |
| Sub-counties/towns/municipal divisions | 2,191 |
| Parishes/wards | 10,717 |
| Villages/cells (each is a polling station for LC I) | 71,214 |
| Elective positions | 45,505 |
| Candidates nominated | 83,597 |
| Biometric Voter Verification Kits (BVVK) | 109,142 (primary and backup per station) |

**Design implication:** the contest catalogue must handle tens of thousands of contests per cycle (one per seat), generated in bulk from templates rather than created one by one. Village-level elections alone add more than 71,000 voting events.

# 3. Legal and policy baseline

The following sources establish the initial legal and operational
context. They must be checked against the latest consolidated law,
amendments, statutory instruments, EC guidelines and notices before
configuration.

  -----------------------------------------------------------------------
  **Source**              **Relevant baseline**   **System implication**
  ----------------------- ----------------------- -----------------------
  Constitution of Uganda, Articles 59--68 address Preserve EC authority,
  1995 as amended         voting and EC           source records,
                          functions; Article 61   station-level result
                          assigns the EC          declarations and
                          functions including     controlled publication.
                          organising,             
                          supervising,            
                          ascertaining,           
                          publishing and          
                          declaring results;      
                          Article 68 addresses    
                          polling-station         
                          counting and            
                          declaration records.    

  Electoral Commission    Sets out EC functions   EC-approved
  Act, Cap. 176           and powers including    configuration and
                          polling day, polling    operating procedures;
                          stations, ballot-paper  accessible and secure
                          control, security and   design.
                          accessibility           
                          responsibilities.       

  Presidential Elections  Provides presidential   Separate presidential
  Act, Cap. 179           election procedures,    contest rule pack with
                          counting, tallying,     legal approval.
                          declaration and         
                          challenges.             

  Parliamentary Elections Provides parliamentary  Configurable
  Act, Cap. 177           election procedures and parliamentary seat and
                          refers to Special       SIG contest catalogue.
                          Interest Groups under   
                          Article 78(1)(c).       

  Local Governments Act   Provides local          Versioned local office
  and amendments          government structures   and administrative
                          and election framework. hierarchy.

  EC 2025/2026 roadmap    Lists presidential,     Support different dates
  and revised polling     parliamentary, local    and methods within one
  dates                   government and SIG      cycle; do not hard-code
                          elections at different  one election-day
                          levels and includes     workflow.
                          conference-based        
                          events.                 
  -----------------------------------------------------------------------

## 3.0 Additional sources listed on the EC Laws and Regulations page [v1.1]

| Source | Relevant baseline | System implication |
|---|---|---|
| Local Governments Act, Cap. 243 (as amended), s.111(2) | Village (LC I) and certain council elections are by voters lining up behind the candidate, their representative, portrait or symbol. | Separate LINE_UP method and form set (EC10, EC11); no ballot accounting (§5.3). |
| Political Parties and Organisations Act 2005, amended by Acts No. 2 and No. 4 of 2010 | Registration and status of political parties and organisations. | Party register as master data with registration status and effective dates; party labels only from the register. |
| National Women's Council Act (amended 2010) | Women's councils and committees from village to national level. | Women's council/committee contests at each level with their own registers (village women's council register). |
| National Youth Council Act (amended 2010) | Youth councils and committees from village to national level. | Youth council/committee contests and delegate structures feeding SIG conferences. |
| Referendum and Other Provisions Act | Conduct of referenda. | REFERENDUM method and rule pack. |
| Constitution of Uganda 1995, disability provisions | Rights and representation of persons with disabilities. | PWD contests; accessibility obligations reinforce NFR-006. |

Note: the Presidential Elections Act is not listed on the EC page. It remains a required source and must be obtained from the official consolidated laws.

## 3.1 Source-backed facts vs items requiring confirmation

  -----------------------------------------------------------------------
  **Classification**                  **Statement**
  ----------------------------------- -----------------------------------
  Source-backed                       The Constitution assigns the EC
                                      responsibility to
                                      organise/supervise elections and
                                      ascertain, publish and declare
                                      results.

  Source-backed                       The EC\'s 2025/2026 revised
                                      schedule includes presidential and
                                      parliamentary contests, local
                                      government councils and SIG
                                      contests involving youth, older
                                      persons, workers and PWDs at
                                      different levels; some are
                                      conducted through conferences.

  Design recommendation               Model each contest with its own
                                      electorate, method, form,
                                      geography, formulas, approvals and
                                      publication status.

  EC/legal confirmation               Exhaustive current list of offices,
                                      SIG categories, reserved seats,
                                      delegate groups, local government
                                      tiers and special procedures.

  EC/legal confirmation               Official result forms, required
                                      fields, signatures, seals,
                                      arithmetic, recount rules,
                                      statutory deadlines, declaration
                                      templates and retention periods.

  EC/legal confirmation               Legal standing of electronic
                                      copies, source images,
                                      system-generated reports and
                                      digitally signed records.
  -----------------------------------------------------------------------

## 3.2 Source-backed facts added in v1.1 [v1.1]

| Classification | Statement |
|---|---|
| Source-backed | Polling-station results are recorded on the Declaration of Results (DR) form, displayed at the station, copied to agents and sent sealed with the Official Report Book (EC Annual Bulletin 2025–2026). |
| Source-backed | Results move from the polling station to the sub-county/town/municipal division results centre and then to the District Electoral Returning Officer. Result envelopes may be opened only at the district tally centre, not at sub-county level (EC Chairperson, January 2026). |
| Source-backed | Presidential results are aggregated at the National Election Results Tally Centre and declared within 48 hours of poll closure. Parliamentary and local council results are tallied and declared by District Electoral Returning Officers. |
| Source-backed | The EC used the Election Results Transmission and Dissemination System (ERTDS) to transmit presidential results from districts to the national centre, and tally centres displayed results on screens for agents, observers and media. |
| Source-backed | BVVKs scanned the DR and Accountability of Ballot Papers forms to create a digital backup. Unverifiable ballots set aside during post-voting checks are recorded in the Official Report Book. |
| Source-backed | LC I chairperson elections in July 2026 were by lining up; the result and declaration are recorded on Form EC10 and executive committee appointments on Form EC11. Village women's committees were elected the same way on a separate register. |
| Source-backed | UPDF representatives to Parliament are elected by the Army Council; PWD, Workers, Youth and Older Persons representatives by national or regional conferences. |
| EC/legal confirmation | Exact statutory provisions on electronic display at tally centres and electronic transmission of returns, and whether the platform may act as, replace or feed ERTDS. |

# 4. Stakeholders and user roles

  -----------------------------------------------------------------------
  **Role**                **Responsibilities**    **Access boundary**
  ----------------------- ----------------------- -----------------------
  EC / election authority Owns cycle              Global administration
  administrator           configuration, approved with dual control for
                          rules, publication      critical changes.
                          policy and operational  
                          oversight.              

  National tally          Monitors national       National dashboards and
  supervisor              status, reviews         assigned approvals.
                          authorised aggregation  
                          and release readiness.  

  Returning officer /     Manages assigned        Assigned
  authorised local        geography, reviews      district/constituency
  officer                 returns, resolves       and contests.
                          permitted exceptions    
                          and forwards results.   

  Presiding officer /     Captures or submits     Assigned station/event
  polling officer         results for assigned    only.
                          voting event.           

  Tally operator          Enters results from     Assigned tally centre,
                          official forms and      contest and queue.
                          attaches evidence.      

  Independent verifier    Compares entry with     Read/verify; cannot
                          source evidence and     approve own entry.
                          records outcome.        

  Approver / supervisor   Reviews exceptions and  Assigned scope and
                          authorises permitted    approval rights.
                          transitions.            

  Legal officer           Reviews legal holds,    Case-scoped read
                          disputes, petitions and access.
                          authorised extracts.    

  Auditor                 Reviews audit events,   Read-only audit scope.
                          evidence, changes,      
                          approvals and           
                          reconciliations.        

  Candidate/party agent   Accesses permitted      Restricted, read-only
                          status, copies or       and contest-specific.
                          observations under EC   
                          policy.                 

  Accredited observer     Accesses permitted      Read-only, scoped.
                          information and submits 
                          observations if         
                          authorised.             

  Public/media            Views released          Public portal only.
                          information and         
                          approved downloads.     

  Security administrator  Manages identity, keys, Technical
                          security monitoring and administration; no
                          incident response.      result editing
                                                  authority.

  Records manager         Applies retention,      Records lifecycle
                          legal hold, archive and permissions.
                          disposal schedules.     

  Helpdesk                Resolves user issues    Support metadata; no
                          without changing result uncontrolled
                          values.                 impersonation.
  -----------------------------------------------------------------------

## 4.1 Segregation of duties

-   A user who enters a result cannot independently verify and approve
    that same record, except under a documented, approved emergency
    procedure.

-   System administrators cannot change result values outside
    application controls; privileged activity must be monitored and
    audited.

-   Publication permission is separate from entry, verification and
    tally operations.

-   Rule changes, post-approval corrections, releases, withdrawals and
    emergency access require multi-person approval.

-   Support access is time-bound, case-specific and logged.

# 5. Election coverage and configurable taxonomy

The application must not assume that every contest uses the same
electorate, geography, ballot, voting process or aggregation formula.
The election catalogue is data-driven and approved per cycle.

  ------------------------------------------------------------------------------
  **Election family**     **Examples to configure**      **Specific modelling
                                                         needs**
  ----------------------- ------------------------------ -----------------------
  Presidential            General election;              National contest,
                          constitutionally required      candidates, valid-vote
                          second election/run-off; other denominator, threshold
                          legally required poll.         and run-off rule only
                                                         as legally approved.

  Parliamentary --        Constituency Member of         Constituency
  directly elected        Parliament.                    electorate, candidate
                                                         list and constituency
                                                         aggregation.

  Parliamentary --        District Woman Representative  District-level contest
  district woman          to Parliament.                 distinct from
  representative                                         constituency MP.

  Parliamentary -- SIG    Youth, older persons, PWDs,    Electoral college,
                          workers, UPDF and other        regional/national
                          constitutionally/statutorily   conference, council or
                          recognised categories where    other prescribed
                          applicable.                    method.

  Local government --     District/city chairperson,     Current lawful local
  district/city           councillors, Lord Mayor/Mayor  authority structure and
                          and SIG councillors where      office mapping.
                          applicable.                    

  Local government --     Municipality, city division,   Not every tier or
  urban/local authority   municipal division, town,      office necessarily
                          subcounty and other legally    exists in every area.
                          recognised levels.             

  Referenda / other       Referendum question(s), other  Question/options and
  public elections        elections assigned to EC by    legally prescribed
                          law.                           outcome formula.
  ------------------------------------------------------------------------------

## 5.1 Election method catalogue

  -----------------------------------------------------------------------
  **Code**                **Method**              **Required
                                                  configuration**
  ----------------------- ----------------------- -----------------------
  POLL_STATION            Direct secret-ballot    Station,
                          polling.                electorate/register
                                                  count if authorised,
                                                  candidates/options,
                                                  form, ballot accounting
                                                  and count rules.

  ELECTORAL_COLLEGE       Voting by defined body  College, membership
                          of eligible             source, eligibility
                          electors/delegates.     snapshot, venue, voting
                                                  and count rules.

  CONFERENCE              Regional/national or    Conference, delegate
                          other authorised        credentials, sessions,
                          conference.             ballot method, official
                                                  return.

  COUNCIL_ASSEMBLY        Voting through council  Membership roster,
                          or prescribed           eligibility, meeting
                          representative body.    record and method.

  REFERENDUM              Vote on one or more     Question text, options,
                          questions.              counting formula and
                                                  release rules.

  OTHER_CONFIGURED        Other legally           EC-approved workflow,
                          authorised process.     legal basis and
                                                  acceptance tests before
                                                  activation.
  -----------------------------------------------------------------------

**[v1.1] Additional methods:**

| Code | Method | Required configuration |
|---|---|---|
| LINE_UP | Open vote by lining up behind candidates (LGA Cap. 243 s.111(2)), e.g. LC I chairperson, village women's committee. | Village register snapshot, assembly and verification times, count per candidate line, Form EC10 declaration, Form EC11 committee record. No ballot accounting. |
| ARMY_COUNCIL | UPDF representatives elected by the Army Council. | Treated as a COUNCIL_ASSEMBLY sub-type with restricted membership data and publication of aggregate results only. |

## 5.3 Line-up (queue) elections [v1.1]

| ID | Requirement | Priority | Acceptance criterion |
|---|---|---|---|
| LIN-001 | Support LINE_UP contests where each village is a voting event and the count is per candidate line. | MUST | A line-up contest activates without ballot fields. |
| LIN-002 | Capture Form EC10 values (count per candidate, chairperson-elect as recorded by the official) and Form EC11 committee appointments as a record, not a vote count. | MUST | EC11 data never enters vote aggregation. |
| LIN-003 | Record the declared person exactly as stated on the form and flag, without altering, any mismatch with the counts. | MUST | Mismatch raises an exception; no automatic winner. |
| LIN-004 | Support very high event volumes (more than 71,000 villages) with bulk contest and event generation from templates. | MUST | Bulk generation of a full village cycle passes the load test. |
| LIN-005 | Allow capture at the sub-county or district office from the paper form where the village has no device. | MUST | Capture location recorded separately from event location. |
| LIN-006 | Keep register snapshots (village council register, village women's council register) restricted. | MUST | Public outputs are aggregate only. |

## 5.2 Configuration controls

-   Every cycle, election, contest, office, candidate, voting event,
    geography version, form and rule pack has a unique ID.

-   Configuration lifecycle: draft, reviewed, approved, active,
    superseded and archived.

-   Once official results are accepted, rule packs are immutable;
    changes require a new version and controlled impact assessment.

-   All legal formulas and thresholds carry a legal citation, effective
    date, test cases and named approvers.

-   Activation is blocked if required configuration is incomplete,
    contradictory or unsigned.

# 6. End-to-end business workflows

## 6.1 Preparation

1.  Create cycle and record legal basis, scope, dates and owner.

2.  Import or configure approved geography and boundary version.

3.  Create elections and contests; assign office, method, electorate and
    geographical scope.

4.  Register candidates, parties/independents and referendum options as
    applicable.

5.  Configure official forms, validation rules, formulas, statuses and
    approvals.

6.  Register polling stations or non-station events; assign officers,
    tally centres and reporting routes.

7.  Register users/devices, train users and conduct readiness checks and
    simulation.

8.  Freeze configuration, record approval and cryptographic
    snapshot/hash.

## 6.2 Capture and verification

9.  Authorised user opens assigned station/event and contest.

10. System validates user, device, contest, geography, schedule and
    configuration version.

11. User enters values from official source form and records form
    serial/reference and prescribed metadata.

12. User captures/uploads source evidence; original image/document is
    preserved.

13. System runs field, arithmetic, duplicate and cross-field validation.

14. Submission is saved locally if offline or sent securely with unique
    ID and server receipt.

15. Independent verifier compares entered values with evidence and marks
    matched, mismatch, illegible or not applicable.

16. Discrepancies enter exception queue; unresolved blocking issues are
    excluded from verified aggregation.

17. Authorised officer reviews and approves/rejects according to the
    workflow.

18. Approved records feed the appropriate aggregation snapshot.

## 6.3 Release

19. Select contest, geography, status and release scope.

20. Run completeness, validation, duplicate, exception, evidence and
    approval checks.

21. Generate release candidate with snapshot, rule version, totals,
    exceptions and hash.

22. Obtain required independent approvals.

23. Publish approved dataset with explicit status, timestamp, scope and
    release identifier.

24. Preserve every prior release; corrections create a new release and
    identify superseded/withdrawn versions.

# 7. Functional requirements

Priority: MUST = mandatory baseline; SHOULD = expected unless formally
waived; COULD = optional. Acceptance criteria are testable and shall be
traced to test cases.

## 7.1 Election administration and configuration

  --------------------------------------------------------------------------
  **ID**            **Requirement**      **Priority**      **Acceptance
                                                           criterion**
  ----------------- -------------------- ----------------- -----------------
  ADM-001           Create election      MUST              Cycle can be
                    cycle with name,                       created, approved
                    legal basis, dates,                    and archived; all
                    jurisdiction, owner                    changes audited.
                    and status.                            

  ADM-002           Support multiple     MUST              Different contest
                    elections/contests                     schedules coexist
                    in one cycle with                      without
                    different dates,                       collision.
                    methods and                            
                    statuses.                              

  ADM-003           Maintain versioned   MUST              Calendar change
                    calendar milestones,                   retains history
                    owner, due date and                    and notifies
                    dependencies.                          assignees.

  ADM-004           Block activation     MUST              Incomplete
                    without approved                       activation is
                    legal basis, method,                   rejected with
                    electorate,                            missing items.
                    geography, form,                       
                    rules and approvals.                   

  ADM-005           Import/export        SHOULD            Invalid package
                    configuration                          rejected with
                    through validated,                     error report.
                    versioned packages.                    

  ADM-006           Create immutable     MUST              Snapshot and hash
                    configuration                          can be retrieved
                    snapshot and hash on                   and verified.
                    activation.                            

  ADM-007           Allow authorised     MUST              Suspension blocks
                    suspension with                        configured
                    reason, scope,                         operations.
                    expiry/review date                     
                    and audit.                             

  ADM-008           Maintain change      MUST              Full lineage is
                    register with                          queryable.
                    requester,                             
                    rationale, impact,                     
                    approver and                           
                    effective time.                        
  --------------------------------------------------------------------------

## 7.2 Electoral geography and voting-event registry

  --------------------------------------------------------------------------
  **ID**            **Requirement**     **Priority**      **Acceptance
                                                          criterion**
  ----------------- ------------------- ----------------- ------------------
  GEO-001           Maintain versioned  MUST              Supports approved
                    hierarchy of                          contest-specific
                    regions,                              hierarchies.
                    districts/cities,                     
                    counties,                             
                    constituencies,                       
                    subcounties, towns,                   
                    municipalities,                       
                    city divisions,                       
                    parishes/wards,                       
                    villages and                          
                    polling stations as                   
                    applicable.                           

  GEO-002           Do not assume every MUST              Invalid
                    administrative unit                   parent-child
                    exists in every                       mapping rejected.
                    electoral                             
                    hierarchy.                            

  GEO-003           Store official      MUST              Unique code and
                    code, name, type,                     history validation
                    parent, source,                       passes.
                    effective dates,                      
                    approval and                          
                    optional geometry.                    

  GEO-004           Preserve historical MUST              Historical results
                    boundaries and                        resolve to
                    station assignments                   original geography
                    after boundary                        version.
                    change.                               

  GEO-005           Register polling    MUST              Duplicate active
                    stations and                          ID rejected.
                    non-station voting                    
                    events with unique                    
                    ID, type, venue,                      
                    geography,                            
                    schedule, status                      
                    and responsible                       
                    officer.                              

  GEO-006           Support approved    MUST              Prior and
                    station/event                         replacement links
                    relocation,                           retained.
                    closure, split,                       
                    merge or                              
                    replacement without                   
                    deleting history.                     

  GEO-007           Assign contests to  MUST              Invalid assignment
                    events only when                      blocked.
                    allowed by approved                   
                    configuration.                        

  GEO-008           Report missing,     MUST              Readiness report
                    inactive or                           identifies every
                    unassigned expected                   gap.
                    voting events.                        
  --------------------------------------------------------------------------

## 7.3 Contest, candidate and option management

  --------------------------------------------------------------------------------
  **ID**            **Requirement**         **Priority**      **Acceptance
                                                              criterion**
  ----------------- ----------------------- ----------------- --------------------
  CON-001           Define contest office,  MUST              Contest cannot
                    type, method,                             activate with
                    jurisdiction,                             missing mandatory
                    electorate, geography,                    values.
                    form, rule pack and                       
                    status.                                   

  CON-002           Represent candidates,   MUST              Entity type
                    independent candidates,                   validation passes.
                    parties/organisations                     
                    and referendum options                    
                    as distinct records.                      

  CON-003           Store approved          MUST              Only approved
                    candidate identifiers,                    nominees enter
                    names, party status,                      active contest
                    ballot label/reference                    snapshot.
                    and nomination status.                    

  CON-004           Record withdrawal,      MUST              Change has authority
                    disqualification or                       reference and
                    other lawful status                       effective date.
                    change without deleting                   
                    prior data.                               

  CON-005           Support approved        SHOULD            Published labels
                    display labels and                        match approved
                    language variants.                        configuration.

  CON-006           Freeze contest-specific MUST              Post-freeze change
                    candidate order and                       requires reapproval.
                    ballot reference with                     
                    version history.                          

  CON-007           Support                 MUST              Eligibility snapshot
                    delegate/college                          is
                    eligibility datasets                      access-restricted.
                    for authorised                            
                    representative                            
                    elections.                                

  CON-008           Prevent public exposure MUST              Public API contains
                    of delegate identity                      aggregate-only
                    and other restricted                      approved data.
                    personal data.                            
  --------------------------------------------------------------------------------

## 7.4 Identity and access management

  -------------------------------------------------------------------------------
  **ID**            **Requirement**     **Priority**      **Acceptance
                                                          criterion**
  ----------------- ------------------- ----------------- -----------------------
  IAM-001           Use named accounts; MUST              Every privileged action
                    prohibit shared                       attributable to person.
                    accounts for                          
                    privileged/result                     
                    processing roles.                     

  IAM-002           Enforce role-based  MUST              Unassigned station
                    and                                   access denied.
                    attribute-scoped                      
                    access by                             
                    geography, contest,                   
                    task and time.                        

  IAM-003           Require MFA for     MUST              MFA enforcement
                    privileged, remote,                   verified.
                    approval and                          
                    publication roles.                    

  IAM-004           Support approved    SHOULD            Lifecycle and
                    identity-provider                     break-glass activity
                    integration and                       audited.
                    controlled                            
                    emergency accounts.                   

  IAM-005           Enforce separation  MUST              Conflicting role
                    between capture,                      assignment blocked or
                    verification,                         exception-controlled.
                    approval,                             
                    publication and                       
                    system                                
                    administration.                       

  IAM-006           Support activation, MUST              Revoked account cannot
                    suspension, expiry,                   initiate a new session.
                    reassignment and                      
                    revocation.                           

  IAM-007           Log failed logins,  MUST              Events available to
                    access denial,                        security monitoring.
                    privilege                             
                    escalation and                        
                    break-glass use.                      

  IAM-008           Enforce session     MUST              Policy tests pass.
                    timeout and                           
                    reauthentication                      
                    for high-risk                         
                    actions.                              
  -------------------------------------------------------------------------------

## 7.5 Result capture and evidence

  -------------------------------------------------------------------------
  **ID**            **Requirement**     **Priority**      **Acceptance
                                                          criterion**
  ----------------- ------------------- ----------------- -----------------
  CAP-001           Render              MUST              Displayed fields
                    contest-specific                      match approved
                    entry form from                       form.
                    approved                              
                    template/version.                     

  CAP-002           Capture             MUST              Required fields
                    candidate/option                      and data types
                    values and all                        validated.
                    other fields                          
                    required by that                      
                    official form.                        

  CAP-003           Record event,       MUST              Complete
                    contest, form                         provenance shown.
                    serial, capture                       
                    user/device/time,                     
                    server receipt and                    
                    config version.                       

  CAP-004           Attach original     MUST              Original bytes
                    source form                           and hash
                    images/documents                      retained.
                    with immutable                        
                    content hash.                         

  CAP-005           Support multi-page  SHOULD            Reviewer can
                    evidence and                          navigate
                    page/section                          evidence.
                    references.                           

  CAP-006           OCR may suggest     MUST              OCR values
                    prefill but cannot                    clearly marked as
                    verify or submit                      suggestions.
                    values without                        
                    human confirmation.                   

  CAP-007           Use approved        MUST              Unconfigured
                    candidate/option                      candidate cannot
                    list; prohibit                        receive count.
                    free-text vote                        
                    recipient.                            

  CAP-008           Support draft,      MUST              State transition
                    submit, authorised                    permissions
                    withdrawal,                           enforced.
                    rejection and                         
                    resubmission                          
                    states.                               

  CAP-009           Prevent duplicate   MUST              Idempotency and
                    event/contest                         duplicate tests
                    submissions; allow                    pass.
                    authorised                            
                    replacement version                   
                    only.                                 

  CAP-010           Retain evidence     MUST              Retention/hold
                    according to                          tests pass.
                    approved retention                    
                    and legal hold                        
                    policy.                               
  -------------------------------------------------------------------------

## 7.6 Validation and arithmetic

  ---------------------------------------------------------------------------
  **ID**            **Requirement**       **Priority**      **Acceptance
                                                            criterion**
  ----------------- --------------------- ----------------- -----------------
  VAL-001           Run versioned         MUST              Rule version and
                    validation rules on                     outcome stored.
                    entry and every                         
                    subsequent change.                      

  VAL-002           Classify errors as    MUST              Blocking error
                    blocking, warning or                    prevents submit;
                    informational.                          warnings follow
                                                            policy.

  VAL-003           Calculate totals      MUST              Independent
                    using approved                          recomputation
                    formulas and preserve                   matches.
                    formula                                 
                    trace/version.                          

  VAL-004           Check all arithmetic  MUST              Valid/invalid
                    relationships defined                   test fixtures
                    by the official form.                   pass.

  VAL-005           Detect duplicate      MUST              Duplicate creates
                    station/event                           exception.
                    submissions, form                       
                    serials and                             
                    conflicting versions.                   

  VAL-006           Flag                  MUST              Flag does not
                    unusual/cross-field                     silently reject
                    values for review                       or change data.
                    without treating flag                   
                    as proof of fraud or                    
                    automatic invalidity.                   

  VAL-007           Permit authorised     MUST              Override
                    warning override with                   immutable and
                    reason, evidence and                    attributable.
                    approval if                             
                    configured.                             

  VAL-008           Never silently round, MUST              Integer and range
                    truncate, infer or                      validation
                    auto-correct vote                       enforced.
                    counts.                                 

  VAL-009           Revalidate historical SHOULD            Impact report
                    records after rule                      identifies
                    change only through                     affected records.
                    controlled process.                     
  ---------------------------------------------------------------------------

## 7.7 Verification and review

  ------------------------------------------------------------------------
  **ID**            **Requirement**    **Priority**      **Acceptance
                                                         criterion**
  ----------------- ------------------ ----------------- -----------------
  VER-001           Route submitted    MUST              Entry user cannot
                    records to                           verify own
                    independent                          record.
                    verifier by                          
                    scope/workload.                      

  VER-002           Compare values     MUST              Field-level
                    with evidence and                    discrepancy
                    record matched,                      supported.
                    mismatch,                            
                    illegible or N/A                     
                    per field/form.                      

  VER-003           Record verifier    MUST              Verification
                    identity, time,                      event immutable.
                    device, outcome,                     
                    comments and                         
                    evidence                             
                    reference.                           

  VER-004           Block verified     MUST              Unresolved result
                    aggregation while                    excluded from
                    blocking                             verified totals.
                    discrepancy                          
                    remains                              
                    unresolved.                          

  VER-005           Support second     MUST              Escalation
                    review, supervisor                   lineage retained.
                    escalation and                       
                    legal/operations                     
                    referral.                            

  VER-006           Support blind      SHOULD            Second operator
                    independent double                   cannot see first
                    entry for                            entry before
                    configured                           submission.
                    contests or                          
                    samples.                             

  VER-007           Report             MUST              Report reconciles
                    verification                         to record state.
                    completeness and                     
                    discrepancy trends                   
                    by contest,                          
                    geography and                        
                    user.                                
  ------------------------------------------------------------------------

## 7.8 Exceptions, corrections, recounts and disputes

  ----------------------------------------------------------------------------
  **ID**            **Requirement**        **Priority**      **Acceptance
                                                             criterion**
  ----------------- ---------------------- ----------------- -----------------
  EXC-001           Maintain exception ID, MUST              Full lifecycle
                    source record, type,                     queryable.
                    severity, owner, due                     
                    date, status and                         
                    evidence.                                

  EXC-002           Support configurable   MUST              Types
                    types: arithmetic,                       configurable and
                    illegible form,                          versioned.
                    missing evidence,                        
                    duplicate, incomplete,                   
                    sync conflict,                           
                    rejection and approval                   
                    issue.                                   

  EXC-003           Prohibit in-place edit MUST              Change creates
                    of submitted/approved                    new version.
                    values.                                  

  EXC-004           Require reason,        MUST              Required audit
                    authority reference,                     fields present.
                    evidence, initiator,                     
                    reviewer and approver                    
                    for correction.                          

  EXC-005           Preserve original and  MUST              Ordinary users
                    all                                      cannot delete
                    intermediate/current                     history.
                    versions.                                

  EXC-006           Represent recount,     MUST              Original event
                    repeat poll,                             preserved and
                    court-ordered change                     linked.
                    or other authorised                      
                    process as linked new                    
                    events.                                  

  EXC-007           Apply legal hold to    MUST              Held records
                    records, evidence,                       cannot be
                    logs and exports.                        disposed.

  EXC-008           Close exception only   MUST              Incomplete
                    with disposition,                        closure blocked.
                    reason, evidence and                     
                    authorised approval.                     

  EXC-009           Show unresolved        MUST              Release gate
                    exceptions and impact                    reflects
                    on release readiness.                    configured
                                                             blockers.
  ----------------------------------------------------------------------------

## 7.9 Transmission and synchronisation

  --------------------------------------------------------------------------
  **ID**            **Requirement**     **Priority**      **Acceptance
                                                          criterion**
  ----------------- ------------------- ----------------- ------------------
  TRN-001           Transmit over       MUST              Transport
                    authenticated                         configuration scan
                    encrypted channels.                   passes.

  TRN-002           Assign immutable    MUST              Retry does not
                    submission ID and                     create duplicate.
                    idempotency key.                      

  TRN-003           Return verifiable   MUST              Client verifies
                    receipt with ID,                      receipt/hash.
                    server time, status                   
                    and content hash.                     

  TRN-004           Support             MUST              Offline sync
                    store-and-forward                     simulation passes.
                    with queued, sent,                    
                    acknowledged,                         
                    rejected and                          
                    conflict states.                      

  TRN-005           Detect tampering,   MUST              Invalid payload
                    invalid signatures,                   quarantined and
                    stale configuration                   alerted.
                    and revoked                           
                    devices.                              

  TRN-006           Retain transmission MUST              Timeline
                    attempts, network                     reconstructable.
                    errors,                               
                    acknowledgements                      
                    and retries.                          

  TRN-007           Never resolve       MUST              Conflict is
                    result conflict                       quarantined.
                    using                                 
                    last-write-wins.                      

  TRN-008           Support rate        SHOULD            Outage recovery
                    limits, backoff and                   load test passes.
                    priority queues.                      
  --------------------------------------------------------------------------

## 7.10 Tallying and aggregation

  ----------------------------------------------------------------------------------------------
  **ID**            **Requirement**                    **Priority**      **Acceptance
                                                                         criterion**
  ----------------- ---------------------------------- ----------------- -----------------------
  TAL-001           Aggregate only records eligible    MUST              Pending/unverified
                    under approved rule pack and                         records excluded from
                    workflow state.                                      verified total.

  TAL-002           Aggregate across configured        MUST              Hierarchy matches
                    station/event, parish/ward,                          contest config.
                    subcounty/municipality/division,                     
                    constituency, district/city,                         
                    regional and national levels as                      
                    applicable.                                          

  TAL-003           Preserve raw event values and      MUST              Drill-down returns
                    lineage to every contributing                        contributors.
                    aggregate.                                           

  TAL-004           Support contest-specific           MUST              Boundary-case formula
                    plurality, threshold, referendum                     tests pass.
                    and college formulas only when                       
                    approved and tested.                                 

  TAL-005           Calculate reporting completeness   MUST              Numerator/denominator
                    separately from votes and vote                       displayed.
                    share.                                               

  TAL-006           Show excluded, pending, rejected,  MUST              No silent exclusion.
                    disputed and superseded records in                   
                    internal views.                                      

  TAL-007           Recalculate from reproducible      MUST              Rerun produces
                    input snapshot and rule version.                     identical total.

  TAL-008           Never independently declare a      MUST              No software-only
                    winner; display only calculations                    declaration.
                    and authority-approved statuses.                     

  TAL-009           Isolate simulation/test tallies    MUST              Environment isolation
                    from production data.                                verified.
  ----------------------------------------------------------------------------------------------

## 7.11 Approval and publication

  ----------------------------------------------------------------------------
  **ID**            **Requirement**        **Priority**      **Acceptance
                                                             criterion**
  ----------------- ---------------------- ----------------- -----------------
  REL-001           Implement configurable MUST              Only assigned
                    approval stages for                      approver can
                    verified totals, local                   transition.
                    review, national                         
                    review and release                       
                    where authorised.                        

  REL-002           Require authorised     MUST              Unauthorised
                    sign-off for release,                    release blocked.
                    withdrawal,                              
                    replacement and                          
                    correction.                              

  REL-003           Support distinct       MUST              Status model
                    statuses: draft,                         configurable and
                    submitted, received,                     approved.
                    validation failed,                       
                    pending verification,                    
                    verified, under                          
                    review, approved for                     
                    release, provisional                     
                    published, officially                    
                    declared, certified,                     
                    superseded and                           
                    withdrawn.                               

  REL-004           Separate technical     MUST              UI does not
                    processing state from                    equate software
                    legal result state.                      verification with
                                                             declaration.

  REL-005           Publish only fields    MUST              Public/private
                    approved for audience                    field tests pass.
                    and release scope.                       

  REL-006           Generate manifest with MUST              Manifest
                    scope, snapshot, rule                    retrievable.
                    version, data time,                      
                    approvers and hash.                      

  REL-007           Preserve previous      MUST              Release history
                    releases and show                        retained.
                    superseded/withdrawn                     
                    notice per policy.                       

  REL-008           Support                SHOULD            Unauthorised
                    embargo/scheduled                        bypass
                    release only under                       impossible.
                    documented policy and                    
                    dual approval.                           
  ----------------------------------------------------------------------------

## 7.12 Notifications and public API

  -------------------------------------------------------------------------
  **ID**            **Requirement**    **Priority**      **Acceptance
                                                         criterion**
  ----------------- ------------------ ----------------- ------------------
  NOT-001           Notify assigned    MUST              Delivery/failure
                    users of tasks,                      logged.
                    rejection,                           
                    exception, overdue                   
                    work, approval and                   
                    security events.                     

  NOT-002           Support approved   SHOULD            Channel
                    in-app, email, SMS                   configurable.
                    or other channels.                   

  NOT-003           Do not expose vote MUST              Template privacy
                    totals, signatures                   review passes.
                    or personal data                     
                    in insecure                          
                    message previews.                    

  PUB-001           Public portal      MUST              Unreleased data
                    exposes only                         inaccessible.
                    approved releases                    
                    and displays                         
                    status, time,                        
                    scope and source                     
                    note.                                

  PUB-002           Provide contest    MUST              Navigation follows
                    and geography                        configured
                    drill-down where                     geography.
                    approved.                            

  PUB-003           Clearly separate   MUST              User test confirms
                    reporting                            understanding.
                    progress, vote                       
                    share and legal                      
                    finality.                            

  PUB-004           Provide approved   SHOULD            API contract test
                    downloads and                        passes.
                    documented,                          
                    versioned                            
                    machine-readable                     
                    API.                                 

  PUB-005           Rate-limit, cache  MUST              Load/abuse tests
                    and protect public                   pass.
                    endpoints against                    
                    abuse/DoS.                           

  PUB-006           Never expose       MUST              Privacy scan
                    individual voter                     passes.
                    choices,                             
                    restricted                           
                    delegate data,                       
                    private contact                      
                    details or                           
                    signatures                           
                    publicly.                            

  PUB-007           Provide data       MUST              Every result view
                    definitions,                         links to
                    methodology,                         explanatory
                    update time and                      metadata.
                    correction                           
                    history.                             
  -------------------------------------------------------------------------

## 7.13 Chain of custody and tally centres [v1.1]

| ID | Requirement | Priority | Acceptance criterion |
|---|---|---|---|
| CUS-001 | Model the physical result route per contest: polling station → sub-county/town/municipal division results centre → district tally centre (Returning Officer) → national tally centre where applicable. | MUST | Route configured per contest and level. |
| CUS-002 | Record custody events for each sealed result envelope: sealed, handed over, received, seal checked, opened, with seal/envelope number, persons, place and time. | MUST | Envelope timeline reconstructable. |
| CUS-003 | Block "opened" and result capture from the envelope at any level not authorised to open it (default: district tally centre only). | MUST | Opening at sub-county is rejected and audited. |
| CUS-004 | Record seal anomalies (broken, missing, number mismatch) as exceptions linked to the envelope and results. | MUST | Exception created; results flagged. |
| CUS-005 | Capture the DR form, the Accountability of Ballot Papers form and relevant Official Report Book entries (including set-aside unverifiable ballots) as evidence. | MUST | Each document type selectable and hashed. |
| CUS-006 | Provide a tally-centre public display mode showing results as they are tallied, with status, denominator and time, for agents, observers and media. | MUST | Display shows only data permitted at that centre. |
| CUS-007 | Track statutory declaration deadlines (e.g. presidential 48 hours from poll close) with a visible countdown and escalation. | SHOULD | Deadline configured per contest; alerts fire. |
| CUS-008 | Handle multiple contests polled at the same station on the same day as separate DR forms and envelopes. | MUST | One custody chain per contest per station. |

# 8. Election visualisation requirements

Visualisation is a core capability with separate internal operational
and public transparency experiences. All visualisations must be neutral,
accessible, explain the underlying metric and show the legal/processing
status of data.

## 8.1 Audience-specific views

  -------------------------------------------------------------------------------------------------
  **View**                **Audience**                        **Minimum content**
  ----------------------- ----------------------------------- -------------------------------------
  National command        EC leadership/authorised national   Cycle and contest status,
  dashboard               operations                          expected/received/verified/approved
                                                              units, unresolved exceptions,
                                                              released totals, system health.

  District/city dashboard Returning officers/local officials  Assigned contests, station/event
                                                              progress, missing returns,
                                                              discrepancies, verification and
                                                              escalation queues.

  Tally-centre operations Tally supervisors/operators         Intake queue, evidence, capture,
  board                                                       verification workload, errors, sync
                                                              and device connectivity.

  Public results portal   Public/media/candidates/observers   Released results, reporting progress,
                                                              map, candidate/option totals, status,
                                                              update time and release history.

  Audit/investigation     Auditors/legal officers             Lineage, versions, evidence,
  view                                                        approvals, exceptions, transmissions
                                                              and access logs.

  ICT/security dashboard  Platform/security operations        Availability, API latency, sync lag,
                                                              queue depth, device status,
                                                              incidents, backup and DR.
  -------------------------------------------------------------------------------------------------

## 8.2 Dashboard components

-   Election-cycle and contest selectors with election date, method and
    status.

-   National metrics: expected, received, verified, approved and
    exception-affected reporting units.

-   Progress percentage with numerator, denominator and calculation
    timestamp.

-   Candidate/option vote totals and percentages only for
    authorised/released datasets, with explicit denominator.

-   Valid/invalid and ballot-accounting metrics only where official form
    defines them.

-   Map with contest-specific geographic hierarchy, layer selector and
    accessible legend.

-   Receipt and verification trend over time; no speculative voter
    preference or outcome forecast.

-   Internal exception summary by type, severity, age and assigned
    queue.

-   Data freshness, last update, release ID and source/methodology note.

-   Export controls limited by user role and publication policy.

## 8.3 Interactive map and drill-down

  ----------------------------------------------------------------------------------------------------
  **ID**            **Requirement**                                **Priority**      **Acceptance
                                                                                     criterion**
  ----------------- ---------------------------------------------- ----------------- -----------------
  VIS-001           Render national map with selectable areas for  MUST              Geometry and
                    selected contest\'s geography version.                           contest geography
                                                                                     match.

  VIS-002           Support drill-down national → district/city →  MUST              Only valid
                    constituency/subcounty/municipality/division →                   hierarchy paths
                    parish/ward → station/event where applicable.                    displayed.

  VIS-003           Support map layers for reporting status,       MUST              Each layer has a
                    verification, approval and released vote                         metric definition
                    metrics.                                                         and legend.

  VIS-004           Map shading must not imply a predicted winner; MUST              Usability test
                    legend states exact encoded metric.                              validates
                                                                                     interpretation.

  VIS-005           Provide keyboard navigation, text alternative, MUST              WCAG audit
                    table equivalent and non-colour status                           passes.
                    indicators.                                                      

  VIS-006           Distinguish not expected, not applicable,      MUST              All states
                    missing, not reported and suppressed units.                      separately
                                                                                     rendered.

  VIS-007           Area selection updates charts/tables without   MUST              Cross-filter
                    losing contest context.                                          tests pass.

  VIS-008           Public polling-station detail is enabled only  MUST              Server-side
                    under approved EC publication policy.                            access policy
                                                                                     enforced.
  ----------------------------------------------------------------------------------------------------

## 8.4 Charts and tables

-   Candidate/option horizontal bars: raw votes, share of valid votes
    and denominator.

-   Candidate/option table: approved label, party/independent status,
    votes, share and status.

-   Reporting progress chart: expected, received, verified and approved
    units over time.

-   Ballot-accounting chart: only official-form categories such as
    valid, invalid, rejected, spoilt or unused where applicable.

-   Geographic comparison: same contest and same metric; cross-contest
    comparison requires explicit methodological warning.

-   Completeness matrix: missing/pending forms by geography and status.

-   Internal exception ageing and verification workload charts.

-   Conference/college view: event-level counts and aggregate results
    without exposing restricted delegate identities.

## 8.5 Filters and interaction

  -----------------------------------------------------------------------
  **Filter**                          **Behaviour**
  ----------------------------------- -----------------------------------
  Cycle/date                          Select active or historical cycle;
                                      never silently switch cycle.

  Election/contest/office             Show only contests in selected
                                      cycle and display
                                      method/electorate.

  Geography                           Only valid areas in the contest\'s
                                      geography version.

  Candidate/party/option              Filter chart series; clearly label
                                      filtered view while preserving full
                                      totals.

  Status                              Public sees released states only;
                                      internal roles see permitted
                                      workflow states.

  Reporting state                     Not expected, not received,
                                      received, verification pending,
                                      verified, approved, published.

  Update time                         Specify whether time refers to
                                      receipt, verification or
                                      publication.

  Event method                        Polling station, conference,
                                      electoral college, council or other
                                      configured event.
  -----------------------------------------------------------------------

## 8.6 Visualisation integrity rules

-   Missing values must never be displayed as zero.

-   Unverified and verified figures must not be combined without
    explicit segmentation and explanation.

-   Do not label a candidate elected/winner/declared based solely on
    software calculation.

-   Always show reporting denominator and timestamp.

-   Do not compare percentages across different electorates/methods
    without context.

-   Use neutral consistent visual encoding; colours must not imply
    endorsement.

-   Every export includes release ID, timestamp, status, scope and
    methodology.

# 9. Data requirements and logical data model

The following is a logical model; physical schema, partitioning, indexes
and storage technology are architecture-stage decisions.

  -------------------------------------------------------------------------------
  **Entity**                **Minimum attributes**  **Relationships / controls**
  ------------------------- ----------------------- -----------------------------
  ElectionCycle             cycle_id, name,         Parent to elections/contests.
                            legal_basis, dates,     
                            status, version         

  Election                  election_id, cycle_id,  One cycle may contain many
                            type, method family,    elections.
                            schedule, jurisdiction, 
                            status                  

  Contest                   contest_id,             Unit of tally/release.
                            election_id, office,    
                            type, method,           
                            electorate,             
                            geography_version,      
                            rule_pack,              
                            form_template           

  RulePack                  id, version,            Immutable after use;
                            legal_reference,        versioned.
                            formulas, validations,  
                            effective date,         
                            approvals               

  GeographyNode             id, official_code,      Versioned hierarchy.
                            name, type, parent,     
                            geometry, effective     
                            dates, source, version  

  VotingEvent               id, contest,            Polling station, conference,
                            event_type,             college, council etc.
                            venue/station,          
                            geography, date,        
                            officer, status         

  Candidate/Option          id, name/label,         Assigned via
                            party/independent,      ContestParticipant.
                            ballot ref, status      

  ContestParticipant        contest_id, entity_id,  Frozen contest snapshot.
                            ballot order,           
                            nomination status,      
                            dates                   

  User/Role/Scope           user_id, role_id, scope RBAC and attribute scope.
                            type/id, validity,      
                            status                  

  ResultSubmission          id, event, contest,     Logical result may have
                            form version, version,  immutable versions.
                            status, capture user,   
                            hash                    

  ResultLine                id, submission,         Structured counts and form
                            candidate/option, field fields.
                            code, value             

  EvidenceObject            id, submission, URI,    Immutable source attachment.
                            hash, media type,       
                            capture metadata        

  ValidationRun             id, submission, rule    Every validation attempt
                            version, outcome,       retained.
                            messages, timestamp     

  Verification              id, submission,         Independent verification.
                            verifier, outcome,      
                            field findings,         
                            timestamp               

  Exception                 id, source, type,       Linked to result/release.
                            severity, owner,        
                            status, disposition,    
                            due date                

  Correction/RecountEvent   id, original event,     New linked event; no
                            authority reference,    overwrite.
                            reason, versions,       
                            approvals               

  AggregationSnapshot       id, contest, level,     Reproducible output.
                            geography, input hash,  
                            formula version, totals 

  Release                   id, scope, snapshot,    Public versioned snapshot.
                            status, approvals,      
                            hash, publish time      

  AuditEvent                id, actor, action,      Append-only/tamper-evident.
                            object, before/after    
                            hash, time,             
                            device/session          

  Device                    id, owner, certificate, Revocable field device.
                            platform, status, last  
                            seen, scope             

  LegalHold/Retention       id, scope, authority,   Prevents disposal.
                            start/end, policy       
                            version                 
  -------------------------------------------------------------------------------

## 9.1 Data quality rules

-   Official IDs unique in their namespace; imported records retain
    source references.

-   Master-data imports record source, date, version, validation report
    and approver.

-   Geography names/codes and relationships are effective-dated.

-   Candidate labels preserve approved spelling and ballot order.

-   Vote counts use non-negative integers unless a legally prescribed
    process requires otherwise.

-   Store UTC timestamps; render Africa/Kampala local time and
    distinguish device, source and server time.

-   Evidence is content-hashed; replacement creates a new version.

# 10. Interfaces and integrations

  -----------------------------------------------------------------------
  **Integration**         **Purpose**             **Control
                                                  requirements**
  ----------------------- ----------------------- -----------------------
  Identity provider       Authentication, MFA and Approved protocol,
                          user lifecycle.         least privilege,
                                                  fail-closed for
                                                  privileged access.

  EC master data          Geography, stations,    Schema validation,
                          candidates, contests    reconciliation,
                          and schedule imports.   versioning and
                                                  approval.

  Official form           Approved forms and      Version pinning;
  repository              instructions.           version visible during
                                                  capture.

  SMS/email gateway       Notifications and       Approved provider,
                          escalation.             privacy-safe content
                                                  and delivery status.

  GIS/map service         Approved boundaries and Contest-version match
                          map geometry.           and licensed geometry.

  Evidence/document store Source forms and        Encryption, hash,
                          attachments.            malware scan, retention
                                                  and legal hold.

  Public API              Released result         Versioned schema,
                          datasets.               authentication for
                                                  restricted routes, rate
                                                  limits and provenance.

  SIEM/security           Security, audit and     Time synchronisation,
  monitoring              infrastructure events.  tamper-evident
                                                  forwarding.

  Backup/DR               Database, object,       Immutable copies,
                          config and key          restore tests and
                          recovery.               reconciliation.

  Analytics/reporting     Approved aggregate      Read-only access; no
                          analysis.               direct write-back to
                                                  result records.
  -----------------------------------------------------------------------

External systems must use documented, authenticated, versioned
interfaces with schema validation, correlation IDs and error handling.
No external integration may write directly to official result tables
outside controlled application workflows.

## 10.1 Existing EC systems [v1.1]

| Integration | Purpose | Control requirements |
|---|---|---|
| ERTDS (Election Results Transmission and Dissemination System) | Existing EC system for transmitting presidential results from districts to the national centre. | Decide whether UETVA replaces, feeds or reads from ERTDS (D-16). Any exchange is a signed, reconciled import/export; neither system silently overwrites the other. |
| BVVK scans | BVVKs scan DR and Accountability of Ballot Papers forms. | Import scans as evidence objects with device ID and hash; link to the matching submission; never treat a scan as verified values. |

# 11. Security, privacy and integrity

## 11.1 Security objectives

-   Confidentiality of personal, restricted operational and security
    data.

-   Integrity/authenticity of source forms, results, rules, approvals
    and releases.

-   Availability and recoverability during network or infrastructure
    disruption.

-   Accountability through named identities, audit logs and independent
    review.

-   Non-repudiation for high-risk approvals where legally/technically
    appropriate.

## 11.2 Security requirements

  -----------------------------------------------------------------------------
  **ID**            **Requirement**        **Priority**      **Acceptance
                                                             criterion**
  ----------------- ---------------------- ----------------- ------------------
  SEC-001           Encrypt all network    MUST              Configuration scan
                    traffic with currently                   passes.
                    approved secure                          
                    protocols/ciphers;                       
                    disable insecure                         
                    legacy protocols.                        

  SEC-002           Encrypt databases,     MUST              Storage and key
                    backups, evidence                        configuration
                    stores and portable                      reviewed.
                    device data at rest.                     

  SEC-003           Manage keys with       MUST              Key lifecycle
                    separation of duties,                    evidence.
                    rotation, recovery and                   
                    audit.                                   

  SEC-004           Apply secure SDLC:     MUST              Release gate
                    threat modelling, peer                   evidence.
                    review, dependency                       
                    scanning, SAST/DAST                      
                    and penetration                          
                    testing.                                 

  SEC-005           Protect privileged     MUST              Privileged access
                    access with MFA,                         review.
                    monitored elevation                      
                    and break-glass                          
                    controls.                                

  SEC-006           Maintain               MUST              Tamper detection
                    tamper-evident logs                      test.
                    for result,                              
                    configuration,                           
                    permission and                           
                    publication actions.                     

  SEC-007           Protect against        MUST              Independent
                    injection, broken                        security test.
                    access control, CSRF,                    
                    XSS, SSRF, insecure                      
                    deserialisation and                      
                    unsafe uploads.                          

  SEC-008           Validate uploaded      MUST              Malicious upload
                    files, restrict                          test.
                    type/size and store                      
                    outside executable                       
                    paths.                                   

  SEC-009           Register field devices MUST              Revoked device
                    and revoke lost/stolen                   cannot sync.
                    device credentials.                      

  SEC-010           Provide central        MUST              Tabletop exercise.
                    security monitoring,                     
                    alerting, incident                       
                    playbooks and evidence                   
                    preservation.                            

  SEC-011           Perform vulnerability  MUST              Remediation/risk
                    assessment before each                   register.
                    election cycle and                       
                    after material                           
                    changes.                                 

  SEC-012           Separate production,   MUST              Environment
                    test, training and                       separation test.
                    development; use                         
                    synthetic/anonymised                     
                    data outside                             
                    production.                              
  -----------------------------------------------------------------------------

## 11.3 Privacy

-   Collect only personal data required for lawful election
    administration.

-   Never store individual voter selections or link voter identity to
    ballot choice.

-   Keep delegate credentials and eligibility records separate from
    public results.

-   Restrict signatures, phone numbers, identity documents and staff
    data to authorised roles.

-   Define retention/disposal under law and approved records policy;
    legal holds override disposal.

-   Conduct privacy impact assessment before large-scale personal-data
    processing or new analytics.

-   Use synthetic or irreversibly anonymised data for training and
    testing.

## 11.4 Threat model

  --------------------------------------------------------------------------------
  **Threat**              **Impact**                       **Mitigation**
  ----------------------- -------------------------------- -----------------------
  Unauthorised alteration Incorrect totals and legal       RBAC, MFA, dual
                          challenge.                       control, immutable
                                                           versions, hashes and
                                                           independent
                                                           verification.

  Insider misuse          Manipulation, suppression or     Segregation, scoped
                          disclosure.                      access, privileged
                                                           monitoring and audit.

  Device theft            Exposure or manipulation of      Encryption, device
                          offline records.                 certificates,
                                                           revocation and
                                                           short-lived
                                                           credentials.

  Network                 Tampered submissions.            Authenticated encrypted
  spoofing/interception                                    transport, signed
                                                           payloads and server
                                                           receipts.

  Denial of service       Unavailable portal or            DDoS controls,
                          operations.                      cache/CDN, queueing,
                                                           failover and rate
                                                           limiting.

  Ransomware              Service disruption/data loss.    Segmentation, endpoint
                                                           protection, immutable
                                                           backups and recovery
                                                           drills.

  Fake/duplicate evidence Incorrect result accepted.       Form serial controls,
                                                           evidence, duplicate
                                                           detection and
                                                           verification.

  Configuration           Wrong                            Signed config releases,
  compromise              candidates/formulas/geography.   dual approvals, version
                                                           lock and test fixtures.

  Data leakage            Privacy and safety impact.       Field-level publication
                                                           allowlists, privacy
                                                           tests and monitoring.
  --------------------------------------------------------------------------------

# 12. Non-functional requirements

  ------------------------------------------------------------------------------
  **ID**            **Requirement**        **Priority**      **Acceptance
                                                             criterion**
  ----------------- ---------------------- ----------------- -------------------
  NFR-001           Central services       MUST              Synthetic
                    target 99.9% monthly                     monitoring and
                    availability during                      monthly report.
                    declared critical                        
                    election periods.                        

  NFR-002           95th percentile        SHOULD            Production-like
                    standard authenticated                   performance test.
                    page response ≤3                         
                    seconds under agreed                     
                    load.                                    

  NFR-003           Online structured      SHOULD            Load test and
                    submission                               telemetry.
                    acknowledgement ≤10                      
                    seconds under target                     
                    load, excluding                          
                    evidence upload.                         

  NFR-004           Scale capture,         MUST              2x agreed forecast
                    verification,                            scale test.
                    aggregation, evidence                    
                    and public read                          
                    workloads                                
                    independently.                           

  NFR-005           Initial capacity       SHOULD            Capacity test.
                    model: 50,000                            
                    concurrent public                        
                    sessions and 10,000                      
                    authenticated users,                     
                    subject to EC volume                     
                    confirmation.                            

  NFR-006           Conform to WCAG 2.2 AA MUST              Independent
                    where applicable.                        accessibility
                                                             audit.

  NFR-007           Field capture uses     MUST              Observed task
                    guided workflow, clear                   usability tests.
                    error messages and low                   
                    interaction burden.                      

  NFR-008           Support English and    SHOULD            Localization test.
                    architecture for                         
                    additional approved                      
                    Ugandan languages.                       

  NFR-009           APIs and exports use   MUST              Contract test.
                    documented, versioned                    
                    formats and stable                       
                    identifiers.                             

  NFR-010           Modules independently  MUST              Architecture/ops
                    testable, documented                     review.
                    and deployable with                      
                    controlled                               
                    migration/rollback.                      

  NFR-011           Audit/result history   MUST              Retention test.
                    retained under                           
                    EC-approved legal                        
                    retention schedule; no                   
                    arbitrary deletion.                      

  NFR-012           Store UTC time,        MUST              Time consistency
                    display                                  test.
                    Africa/Kampala,                          
                    monitor clock drift                      
                    and preserve                             
                    source/device/server                     
                    times.                                   

  NFR-013           Retries idempotent;    MUST              Failure-injection
                    partial failure cannot                   test.
                    corrupt result                           
                    records.                                 

  NFR-014           Provide documented     SHOULD            Exit/restore test.
                    data export and                          
                    recovery to reduce                       
                    unnecessary vendor                       
                    lock-in.                                 

  NFR-015           Expose health, logs,   MUST              Observability
                    metrics, traces,                         review.
                    queues and sync lag                      
                    without leaking result                   
                    or personal data.                        
  ------------------------------------------------------------------------------

## 12.1 Proposed recovery objectives

  ----------------------------------------------------------------------------------------
  **Service/data**             **Proposed RPO**  **Proposed RTO**  **Validation**
  ---------------------------- ----------------- ----------------- -----------------------
  Result database/audit        ≤5 minutes        ≤60 minutes       Restore and reconcile.

  Evidence store               ≤15 minutes       ≤4 hours          Object count/hash
                                                                   reconciliation.

  Public portal/read cache     ≤15 minutes       ≤2 hours          Failover/repopulation
                                                                   test.

  Identity/configuration/key   ≤5 minutes        ≤2 hours          Dual-control recovery
  management                                                       test.

  Analytics/reporting          ≤24 hours         ≤24 hours         Rebuild from
                                                                   authoritative records.
  ----------------------------------------------------------------------------------------

These are engineering targets for planning, not statutory deadlines. The
EC must approve final values after business impact analysis.

# 13. Offline operation and synchronisation

Offline capability is required for field environments with intermittent
connectivity. It must preserve identity, integrity, provenance and
segregation of duties.

  ------------------------------------------------------------------------
  **ID**            **Requirement**   **Priority**      **Acceptance
                                                        criterion**
  ----------------- ----------------- ----------------- ------------------
  OFF-001           Download only     MUST              Offline package
                    assigned contest,                   matches user
                    geography, event                    scope.
                    and approved form                   
                    configuration.                      

  OFF-002           Sign/encrypt      MUST              Expired/tampered
                    offline package                     package rejected.
                    and enforce                         
                    expiry.                             

  OFF-003           Store draft       MUST              Device inspection
                    submissions and                     confirms
                    evidence in                         encryption.
                    encrypted local                     
                    storage.                            

  OFF-004           Assign unique     MUST              Retries do not
                    client                              duplicate.
                    transaction ID                      
                    and monotonic                       
                    local sequence.                     

  OFF-005           Show unsent,      MUST              User can identify
                    sending,                            pending items.
                    acknowledged,                       
                    rejected and                        
                    conflict states.                    

  OFF-006           On reconnect,     MUST              Reconnect test
                    authenticate,                       passes.
                    check                               
                    configuration                       
                    validity and                        
                    synchronise in                      
                    controlled order.                   

  OFF-007           Quarantine        MUST              Conflict review
                    conflicts; never                    queue created.
                    use                                 
                    last-write-wins                     
                    for results.                        

  OFF-008           Revoke            MUST              Revocation test
                    device/offline                      passes.
                    package; reject                     
                    subsequent sync                     
                    from revoked                        
                    credentials.                        

  OFF-009           Provide encrypted SHOULD            Recovery action
                    recovery bundle                     audited.
                    only under                          
                    approved support                    
                    procedure.                          

  OFF-010           Report last sync, MUST              Operations view
                    queue size,                         reconciles.
                    oldest pending                      
                    item and device                     
                    health.                             
  ------------------------------------------------------------------------

## 13.1 Conflict-resolution workflow

25. Detect conflict using event, contest, submission ID, version and
    hash.

26. Freeze conflicting versions from automatic aggregation.

27. Create exception linking payloads and transmission receipts.

28. Assign independent reviewer and authorised decision-maker.

29. Compare source evidence and authorised history.

30. Approve a current version or create corrected version under policy.

31. Preserve all payloads and decision evidence.

32. Recalculate affected aggregates and identify releases needing
    correction/withdrawal.

# 14. Reporting and transparency

  -----------------------------------------------------------------------
  **Report**              **Audience**            **Minimum contents**
  ----------------------- ----------------------- -----------------------
  Election readiness      EC/operations           Configuration,
                                                  stations/events,
                                                  candidates, users,
                                                  devices, forms,
                                                  training and gaps.

  Results register        Authorised officers     Event/contest, status,
                                                  form reference, totals
                                                  and evidence
                                                  availability.

  Intake report           Tally operations        Received, pending,
                                                  rejected, duplicate,
                                                  queued and acknowledged
                                                  submissions.

  Verification report     Supervisors/auditors    Verified, mismatched,
                                                  pending, verifier,
                                                  discrepancy, age and
                                                  disposition.

  Exception register      Operations/legal        ID, type, severity,
                                                  status, owner, age,
                                                  authority and
                                                  disposition.

  Aggregation report      Authorised officers     Totals by level,
                                                  contributing records,
                                                  exclusions, formula
                                                  version and snapshot.

  Release report          Publication team        Release ID, scope,
                                                  status, approvals,
                                                  timestamp, hash and
                                                  correction history.

  Audit report            Auditors/legal          User actions,
                                                  before/after values,
                                                  access, approvals,
                                                  device and correlation
                                                  ID.

  System health           ICT/security            Availability, latency,
                                                  sync lag, incidents,
                                                  backups and DR.

  Post-election report    EC leadership           Completeness,
                                                  processing times,
                                                  exception trends,
                                                  incidents and lessons.
  -----------------------------------------------------------------------

-   PDF/spreadsheet exports include title, generated time, contest,
    geography, status, release ID and definitions.

-   Restricted exports require permission and are logged with purpose
    and scope.

-   Large exports are asynchronous, access-controlled and expire.

-   Public exports contain approved aggregate data only; no private
    identifiers.

# 15. Deployment and operations

## 15.1 Logical architecture

  -----------------------------------------------------------------------
  **Layer**                           **Responsibilities**
  ----------------------------------- -----------------------------------
  User experience                     Admin console, field capture,
                                      tally-centre workspace, audit
                                      console and public portal.

  API/access                          Authentication, authorisation, rate
                                      limiting, validation and routing.

  Configuration                       Cycle, contest, method, geography,
                                      form, rule pack and workflow
                                      configuration.

  Results services                    Capture, validation, verification,
                                      exceptions, tally, release and
                                      reporting.

  Evidence service                    Upload, malware scan, hashing,
                                      metadata, retrieval and retention.

  Workflow/notifications              Task assignment, approvals,
                                      escalation and notification.

  Data services                       Transactional database, immutable
                                      audit, object store, read models,
                                      cache and analytics.

  Security/operations                 Identity, key management,
                                      monitoring, SIEM, backup, DR and
                                      incident response.
  -----------------------------------------------------------------------

## 15.2 Environments

-   Development -- synthetic data only.

-   Integration test -- synthetic/anonymised data.

-   UAT/training -- simulation cycles and approved training records.

-   Pre-production -- production-like configuration and load.

-   Production -- hardened, monitored, segregated and change-controlled.

-   Disaster recovery -- isolated recovery environment with
    failover/failback procedures.

## 15.3 Operations requirements

  -------------------------------------------------------------------------
  **ID**            **Requirement**     **Priority**      **Acceptance
                                                          criterion**
  ----------------- ------------------- ----------------- -----------------
  OPS-001           Monitor             MUST              Health dashboard
                    application,                          and alerts.
                    database, storage,                    
                    network, identity,                    
                    queues and field                      
                    devices centrally.                    

  OPS-002           Alert on outage,    MUST              Alert and
                    queue growth, sync                    escalation tests.
                    delay, replication                    
                    lag, backup failure                   
                    and security                          
                    incidents.                            

  OPS-003           Deploy signed       MUST              Release evidence
                    artefacts with                        retained.
                    approval, versioned                   
                    migrations and                        
                    rollback plan.                        

  OPS-004           Maintain encrypted, MUST              Restore report.
                    immutable/offline                     
                    backup copies and                     
                    test restoration                      
                    periodically.                         

  OPS-005           Maintain runbooks   MUST              Operational
                    for startup,                          readiness review.
                    shutdown, failover,                   
                    failback, key                         
                    recovery and                          
                    incident response.                    

  OPS-006           Provide             MUST              Support
                    election-period                       simulation.
                    service desk,                         
                    severity model,                       
                    response targets                      
                    and on-call                           
                    escalation.                           

  OPS-007           Record production,  MUST              Change audit.
                    emergency and                         
                    rollback changes.                     

  OPS-008           Support controlled  MUST              Archived election
                    archival and                          remains immutable
                    read-only                             and queryable.
                    post-election mode.                   
  -------------------------------------------------------------------------

# 16. Testing and acceptance

## 16.1 Test levels

-   Unit tests for formulas, validations, permissions and state
    transitions.

-   Integration tests for identity, APIs, evidence, notifications, GIS,
    monitoring and backup.

-   System tests across every configured election method and end-to-end
    workflow.

-   Security testing: penetration, privilege escalation, access control,
    device compromise and data leakage.

-   Performance, load, spike, soak, failover and recovery testing.

-   Offline capture, interrupted network, duplicate retry and conflict
    resolution testing.

-   Accessibility and usability testing with representative users and
    assistive technologies.

-   EC-led UAT, parallel tally simulation and full operational dress
    rehearsal.

## 16.2 Core UAT scenarios

  ------------------------------------------------------------------------------
  **ID**                  **Scenario**                   **Expected result**
  ----------------------- ------------------------------ -----------------------
  UAT-001                 Create presidential contest    Correct rule/form
                          with approved candidates and   pinned; activation
                          polling-station form.          approval enforced.

  UAT-002                 Create constituency MP and     Distinct geography and
                          district woman representative  tally scopes
                          contests in same cycle.        maintained.

  UAT-003                 Configure SIG contest using    Workflow does not force
                          conference/electoral-college   polling-station
                          method.                        process.

  UAT-004                 Enter result with arithmetic   Error/exception raised;
                          mismatch.                      no silent correction.

  UAT-005                 Upload illegible source form.  Evidence quality
                                                         flagged and routed for
                                                         review.

  UAT-006                 Two operators independently    Mismatch detected;
                          enter different values.        verification blocked
                                                         pending resolution.

  UAT-007                 Retry same payload after       No duplicate
                          timeout.                       submission.

  UAT-008                 Reconnect offline client after Stale package flagged
                          config version changed.        and controlled review
                                                         initiated.

  UAT-009                 Attempt direct edit of         Blocked; correction
                          approved result.               workflow required.

  UAT-010                 Attempt release with blocking  Release prevented.
                          exception or missing approval. 

  UAT-011                 Drill national map to station  Map/chart/table values
                          for released contest.          and status agree.

  UAT-012                 Public API request for         No unreleased data
                          unreleased data.               exposed.

  UAT-013                 Recalculate historical         Identical input/rule
                          snapshot.                      version reproduces same
                                                         totals.

  UAT-014                 Apply legal hold to evidence   Deletion prevented.
                          and audit data.                

  UAT-015                 Restore database and evidence  Hashes and
                          in DR.                         reconciliation pass
                                                         within approved
                                                         RPO/RTO.

  UAT-016                 Revoke lost field device.      Device cannot
                                                         authenticate or
                                                         synchronise.

  UAT-017                 Public portal peak traffic     Agreed performance met
                          simulation.                    without impairing tally
                                                         services.

  UAT-018                 Lawfully correct released      New release references
                          result.                        superseded version;
                                                         history preserved.
  ------------------------------------------------------------------------------

## 16.3 Acceptance gates

  -----------------------------------------------------------------------
  **Gate**                **Evidence**            **Approvers**
  ----------------------- ----------------------- -----------------------
  G1 Requirements         Approved SRS, legal     EC sponsor, legal,
                          mapping, process maps,  operations, ICT.
                          data protection review, 
                          open issue register.    

  G2 Design               Architecture, threat    Architecture/security
                          model, data model,      board.
                          interface and DR        
                          designs.                

  G3 Build                Traceability,           Product owner/technical
                          unit/integration tests, lead.
                          code review, deployment 
                          and user docs.          

  G4 Security             Pen test, remediation,  Security authority.
                          key management,         
                          incident response and   
                          access review.          

  G5 Operations           Load, backup/restore,   Operations/ICT.
                          failover, device        
                          readiness, training and 
                          runbooks.               

  G6 Simulation           End-to-end simulation   EC-appointed simulation
                          across election methods committee.
                          and representative      
                          geographies.            

  G7 Go-live              Signed readiness        Competent EC authority.
                          certificate, approved   
                          configuration, release  
                          controls and rollback   
                          plan.                   
  -----------------------------------------------------------------------

# 17. Governance, RACI and change control

  ----------------------------------------------------------------------------------------------------
  **Activity**        **EC        **Election   **Legal**   **ICT/security**   **Delivery   **Audit**
                      sponsor**   ops**                                       team**       
  ------------------- ----------- ------------ ----------- ------------------ ------------ -----------
  Approve election    A           R            C           C                  I            I
  rules/forms                                                                              

  Approve             A           R            C           C                  I            I
  geography/station                                                                        
  data                                                                                     

  Configure rule pack A           R            C           C                  R            I

  Capture source      A           R            I           I                  I            I
  results                                                                                  

  Verify/approve      A           R            C           I                  I            C
  results                                                                                  

  Approve public      A/R         C            C           C                  I            I
  release                                                                                  

  Operate             A           C            I           R                  R            C
  platform/security                                                                        

  Audit               A           C            C           C                  I            R
  controls/records                                                                         

  Approve material    A           C            C           R                  R            C
  system change                                                                            
  ----------------------------------------------------------------------------------------------------

R = Responsible; A = Accountable; C = Consulted; I = Informed. Final
RACI must be aligned to EC structure and statutory delegations.

-   Every change request identifies affected requirements, legal basis,
    rationale, risk, cost, test plan, date and rollback.

-   Changes to method, formulas, thresholds, candidates, geography,
    forms or publication status require EC/legal approval.

-   Emergency changes require documented authority, dual control and
    retrospective review.

-   Each release is traceable to source code, configuration, tests and
    deployment approvals.

# 18. Risks, assumptions, dependencies and open decisions

  -------------------------------------------------------------------------------------
  **ID**            **Risk / assumption /        **Impact**           **Mitigation /
                    dependency**                                      decision**
  ----------------- ---------------------------- -------------------- -----------------
  R-01              Rules/procedures change or   Wrong                Obtain legal rule
                    differ by contest.           workflow/formula.    catalogue and EC
                                                                      approval.

  R-02              Official forms vary by       Invalid/incomplete   Obtain
                    contest/revision.            capture.             authoritative
                                                                      form register.

  R-03              Boundaries/station lists     Misaggregation.      Effective-dated
                    change.                                           geography and
                                                                      approved master
                                                                      data.

  R-04              Connectivity intermittent.   Delayed              Offline-first,
                                                 transmission.        store-forward and
                                                                      sync monitoring.

  R-05              Public traffic spikes.       Portal outage.       Separate read
                                                                      path, cache/CDN
                                                                      and load test.

  R-06              Insider/cyberattack.         Integrity,           Dual control,
                                                 confidentiality or   least privilege,
                                                 availability loss.   monitoring and
                                                                      independent
                                                                      tests.

  R-07              Partial reporting misread as Misinformation.      Prominent status,
                    final.                                            denominator,
                                                                      timestamp and
                                                                      methodology.

  R-08              SIG methods vary.            Incorrect workflow.  Configurable
                                                                      method catalogue
                                                                      and EC
                                                                      confirmation.

  R-09              Over-collection/disclosure   Privacy exposure.    Data minimisation
                    of personal data.                                 and privacy
                                                                      impact
                                                                      assessment.

  R-10              External systems lack APIs.  Delivery delay.      Interface
                                                                      discovery and
                                                                      controlled import
                                                                      fallback.

  A-01              EC/competent authority       Needed for           Confirm owners
                    provides authoritative       configuration.       and dates.
                    forms, data, rules and                            
                    approvers.                                        

  A-02              Platform is supporting       Governance           Reflect in
                    technology, not statutory    constraint.          contracts, UX and
                    decision-maker.                                   training.

  D-01              Approved identity, hosting,  Architecture         Technical
                    network and key management.  dependency.          discovery.

  D-02              Approved retention,          Lifecycle            Legal/records
                    disclosure and archival      dependency.          approval.
                    policy.                                           
  -------------------------------------------------------------------------------------

## 18.1 Decisions before detailed design

33. Which institution owns and operates the platform and who approves
    each contest configuration?

34. Which source record is authoritative for each workflow: signed paper
    form, approved electronic record or both?

35. What is the legally approved status vocabulary and release authority
    for each contest?

36. What is the exhaustive current list of offices, SIG categories,
    delegate groups and methods?

37. Which official forms, signatures, seals, fields and formulas apply
    to every contest?

38. Which geographic codes, boundaries, station lists and
    conference/college registers are authoritative?

39. Which result fields may be published at station, constituency,
    district and national levels, and when?

40. What retention, legal hold, data-subject and archival requirements
    apply?

41. What hosting, data residency, connectivity, device and DR
    constraints apply?

42. What are actual event counts, users, submissions, evidence sizes and
    peak public traffic?

## 18.2 Decisions added in v1.1 [v1.1]

- D-15: Which local elections use LINE_UP and which use secret ballot in the current cycle, and are EC10/EC11 values captured centrally at all?
- D-16: Relationship with ERTDS: replace, feed or consume? Which system's figures are authoritative at the national tally centre?
- D-17: Can BVVK scans of DR forms be used as the primary evidence image?
- D-18: Exact rules for envelope opening, re-sealing and custody at each level, and what counts as a seal anomaly.
- D-19: Statutory declaration deadlines per contest type.
- D-20: Which data the tally-centre display may show before declaration.

# 19. Traceability matrix

  ------------------------------------------------------------------------------------------------
  **Domain**             **Requirement     **Design artefact**    **Verification**
                         IDs**                                    
  ---------------------- ----------------- ---------------------- --------------------------------
  Administration         ADM-001--008      Configuration          Config tests and audit review
                                           model/workflow         

  Geography              GEO-001--008      Geography/event model  Hierarchy/effective-date tests

  Contest                CON-001--008      Contest/participant    Contest and scope tests
                                           model                  

  Identity               IAM-001--008      RBAC/ABAC matrix       Permission/MFA/SoD tests

  Capture                CAP-001--010      Form                   Capture/hash/duplicate tests
                                           templates/evidence     
                                           service                

  Validation             VAL-001--009      Rule engine/catalogue  Formula/boundary tests

  Verification           VER-001--007      Verification workflow  Dual-entry/discrepancy tests

  Exceptions             EXC-001--009      Exception/correction   Immutable history/legal hold
                                           model                  

  Transmission/offline   TRN-001--008;     Sync protocol/client   Outage/replay/conflict tests
                         OFF-001--010                             

  Tallying               TAL-001--009      Aggregation engine     Reconciliation/reproducibility

  Release/public         REL-001--008;     Release/API/portal     Approval/withdrawal/access tests
                         PUB-001--007                             

  Visualisation          VIS-001--008      Dashboard/map/chart UX Consistency/accessibility tests

  Security               SEC-001--012      Threat/security        Pen test/incident exercise
                                           architecture           

  NFR/operations         NFR-001--015;     SLOs/runbooks/DR       Performance/DR/ops readiness
                         OPS-001--008                             
  ------------------------------------------------------------------------------------------------


**[v1.1] additions:**

| Domain | Requirement IDs | Design artefact | Verification |
|---|---|---|---|
| Line-up elections | LIN-001–006 | Method catalogue, EC10/EC11 templates | Bulk-generation and record-only tests |
| Custody/tally centres | CUS-001–008 | Custody model, display mode | Envelope timeline and opening-rule tests |
| Existing systems | §10.1 | ERTDS/BVVK adapters | Reconciliation tests |
# 20. Glossary

  -----------------------------------------------------------------------
  **Term**                            **Definition**
  ----------------------------------- -----------------------------------
  Aggregation                         Combining eligible lower-level
                                      results into a higher-level total
                                      using approved rules.

  Candidate/option                    Nominated person or referendum
                                      choice in a contest.

  Contest                             Specific office, seat, question or
                                      position being determined.

  Electoral college                   Legally defined body of eligible
                                      electors/delegates.

  Evidence object                     Source form image, document or
                                      other approved record linked to a
                                      result.

  Official declaration                Declaration by competent authority
                                      in form/manner required by law.

  Polling station                     Authorised location where voting
                                      takes place.

  Provisional result                  Status used only where approved
                                      law/policy permits; not equivalent
                                      to certification.

  Rule pack                           Versioned formulas, validations,
                                      thresholds, workflow and legal
                                      references.

  SIG                                 Special Interest Group as defined
                                      by Constitution and applicable
                                      legislation.

  Tallying                            Receiving, checking and aggregating
                                      recorded votes/results.

  Voting event                        Polling station, conference,
                                      electoral college, council or other
                                      configured event.

  Verified                            Configured verification completed;
                                      not itself legal declaration.

  Release                             Approved, versioned dataset made
                                      available to an intended audience.
  -----------------------------------------------------------------------

# Appendix A -- Candidate result-form field catalogue

The exact field set must be derived from each current official form.
This is a checklist, not an assertion that every field applies to every
contest.

  ---------------------------------------------------------------------------------------------------
  **Group**                           **Potential fields**
  ----------------------------------- ---------------------------------------------------------------
  Identity                            Cycle/election/contest IDs; office; geography codes;
                                      station/event code/name; form type/version/serial.

  Event                               Polling/event date; count time where required; presiding
                                      officer reference; tally centre/returning officer.

  Ballot accounting                   Received, issued, unused, spoilt, rejected, found in box,
                                      valid, invalid and other prescribed categories.

  Candidate totals                    Candidate/option ID; ballot order; vote count; total valid
                                      votes; display percentage.

  Participation                       Registered/eligible count; voters who voted; turnout only with
                                      approved definition and denominator.

  Attestation                         Names/signatures, agent presence/observation fields, time and
                                      seal/form evidence as prescribed.

  Evidence                            Image pages, hash, device, capture user, upload time, quality
                                      status and reviewer notes.

  Workflow                            Draft/submitted/received/validated/verified/approved/rejected
                                      state, reasons and approvers.

  Audit                               Version, before/after, actor, action, device, time, source,
                                      session and correlation ID.
  ---------------------------------------------------------------------------------------------------

# Appendix B -- Minimum result status model

  ------------------------------------------------------------------------
  **Status**              **Meaning**             **Public visibility**
  ----------------------- ----------------------- ------------------------
  Draft                   Work in progress.       Never public.

  Submitted               Sent by source/field    Normally internal.
                          user.                   

  Received                Server acknowledged     Progress indicator only
                          receipt; not verified.  if policy allows.

  Validation failed       Blocking validation     Internal; excluded from
                          rule failed.            verified totals.

  Pending verification    Awaiting independent    Internal or
                          comparison.             received-only public
                                                  status if approved.

  Verified                Configured verification Not automatically
                          completed.              declared.

  Under review            Exception/supervisory   Public only if policy
                          review ongoing.         allows.

  Approved for release    Authorised for          Internal.
                          publication, not yet    
                          published.              

  Published provisional   Released under approved Public with prominent
                          provisional status.     label.

  Officially declared     Competent authority has Public with
                          formally declared       authority/time/source.
                          result.                 

  Certified               Certification status    Public only as approved.
                          where legally           
                          applicable.             

  Superseded/withdrawn    Prior release replaced  History/notice shown as
                          or withdrawn.           policy requires.
  ------------------------------------------------------------------------

# Appendix C -- Minimum release checklist

-   Contest/geography versions approved and frozen.

-   Expected voting events accounted for or formally dispositioned.

-   Included records meet required status threshold.

-   Blocking arithmetic, duplicate, evidence and approval exceptions
    resolved or formally dispositioned.

-   Aggregation snapshot reproducible and reconciled to source records.

-   Candidate/option list matches approved configuration.

-   Public fields pass privacy and publication checks.

-   Release label accurately states provisional, declared or certified
    status.

-   Approvers are authorised and independent as required.

-   Manifest, hash, timestamp, scope and history stored.

# Appendix D -- Official source references

-   **Electoral Commission -- Elections / electoral process ---**
    https://www.ec.or.ug/elections. EC describes election cycle and
    activities including voter registration, voter education,
    nominations, campaigns, polling and declaration.

-   **Electoral Commission -- Laws and Regulations ---**
    https://www.ec.or.ug/laws-and-regulations. EC-hosted list of
    principal electoral legislation and related instruments.

-   **Electoral Commission -- 2025/2026 General Elections ---**
    https://www.ec.or.ug/election/20252026-general-elections. EC roadmap
    and cycle information.

-   **EC -- Revised 2026 polling dates ---**
    https://www.ec.or.ug/news/polling-dates-presidential-parliamentary-and-local-government-councils-elections-2026-revised.
    Notice lists presidential, parliamentary, local government and SIG
    contests at different levels, including conference-based elections.

-   **Constitution of Uganda -- ULII ---**
    https://ulii.org/en/akn/ug/act/statute/1995/constitution/eng%402023-12-31.
    Constitutional provisions on voting, EC functions, polling,
    declaration and local government.

-   **Electoral Commission Act, Cap. 176 -- ULII ---**
    https://ulii.org/en/akn/ug/act/1997/3/eng%402023-12-31. EC powers
    and functions.

-   **Presidential Elections Act, Cap. 179 -- ULII ---**
    https://ulii.org/sw/akn/ug/act/2005/16/eng%402023-12-31.
    Presidential election procedures, tallying, declaration and
    challenges.

-   **Parliamentary Elections Act, Cap. 177 -- ULII ---**
    https://ulii.org/en/akn/ug/act/2005/17/eng%402024-12-23.
    Parliamentary election procedures and SIG references.

-   **[v1.1] EC Annual Bulletin 2025--2026 ---**
    https://www.ec.or.ug/sites/default/files/docs/EC%20ANNUAL%20BULLETIN%202025-2026.pdf.
    Scale figures, 2026 election schedule and methods, results route,
    ERTDS and BVVK.

-   **[v1.1] EC press statement, 22 July 2026 ---**
    https://www.ec.or.ug/sites/default/files/press/Press%20Statement%20of%20Wednesday%2022.7.2026.pdf.
    LC I and women's committee elections by lining up; Forms EC10/EC11.

-   **[v1.1] EC Laws and Regulations page ---**
    https://ec.or.ug/laws-and-regulations. Political Parties and
    Organisations Act 2005 and 2010 amendments, National Women's Council
    and National Youth Council (Amendment) Acts 2010, Referendum and Other
    Provisions Act, Local Governments Act, disability provisions. Most
    linked PDFs could not be retrieved in this review and must be read
    directly by legal counsel.

-   **[v1.1] Reports on the January 2026 tally ---**
    https://allafrica.com/stories/202601090444.html and
    https://allafrica.com/stories/202601160006.html. Results announced
    at station, district and national levels; envelopes opened only at
    district tally centres.

Validation note: Online EC notices and legal texts were consulted as an
initial baseline on 4 November 2026. Legal counsel must verify current
consolidated laws, amendments, statutory instruments, EC guidelines,
official forms and subsequent notices before procurement or
implementation. This SRS is a technical baseline, not legal advice.

# Appendix E -- Recommended implementation phases

  -----------------------------------------------------------------------
  **Phase**               **Scope**               **Exit criteria**
  ----------------------- ----------------------- -----------------------
  0 Discovery/legal       EC process workshops,   Signed process maps,
  validation              law, forms, data,       legal rule catalogue,
                          geography, volume,      data inventory,
                          security and hosting.   approved scope.

  1 Foundation            Identity, roles, audit, Configuration and
                          election configuration, access-control
                          geography, contests and acceptance.
                          events.                 

  2 Capture/verification  Forms, evidence,        End-to-end field
                          offline client,         simulation.
                          validation,             
                          verification and        
                          exceptions.             

  3 Tally/release         Aggregation, approvals, Reproducible tally and
                          release manifests,      publication controls
                          correction history and  accepted.
                          reporting.              

  4 Visualisation         Internal dashboards,    Accessibility, privacy
                          maps, charts, public    and load tests pass.
                          portal and APIs.        

  5 Hardening/pilot       Security, DR, devices,  Go-live readiness
                          training, pilot and     signed.
                          dress rehearsal.        

  6 Rollout/post-election National rollout,       Post-cycle review and
                          support, monitoring,    controlled archive.
                          audit, archive and      
                          lessons learned.        
  -----------------------------------------------------------------------
