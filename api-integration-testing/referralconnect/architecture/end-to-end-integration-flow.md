ReferralConnect End-to-End Integration Architecture
Overview
ReferralConnect is a fictional multi-system integration project demonstrating API, Single Sign-On (SSO), functional, and end-to-end integration testing.

The project contains three fictional applications:

IntakeConnect – Incident intake application
ClaimCore – Claims management application
ReferralHub – External referral management application
The systems exchange incident, claim, referral, examiner, and referral-status information through multiple integration flows.

1. System Overview
The high-level architecture is:

                     IntakeConnect
                          |
                          |
                          v
                     ReferralHub
                          |
                          |
                          v
                     IntakeConnect
                          |
                     Intake Data
                          |
                          v
                       ClaimCore
                          |
              Incident Converted to Claim
                          |
                          v
                  Referral Available
                          |
              +-----------+-----------+
              |                       |
              v                       v
        SSO / Referral          Claim Updates
              |                       |
              v                       v
         ReferralHub  <----------> ClaimCore

   The integration is bidirectional.
ReferralHub sends referral and referral-status information to ClaimCore.
ClaimCore sends examiner updates to ReferralHub.
2. Integration Flow 1 – Existing Referral During Intake
An incident may already have associated referral information available from ReferralHub during the intake process.
The flow is:
Plain Text
IntakeConnect
|
v
Create Incident
|
v
Request Referral Information
|
v
ReferralHub
|
v
Locate Existing Referral
|
v
Return Referral Information
|
v
IntakeConnect
|
v
Incident + Referral Information
|
v
Intake Integration
      |
      v
ClaimCore

QA Validation Points
Testing validates:
Incident is created successfully.
IntakeConnect sends the applicable referral request.
ReferralHub receives the request.
Correct referral information is identified.
Correct referral information is returned.
IntakeConnect receives the response.
Referral information is associated with the correct incident.
Referral information is included in downstream intake data.
ClaimCore receives the incident.
ClaimCore receives the referral information.
No unrelated referral information is included.
3. Integration Flow 2 – Incident-to-Claim Conversion
After the incident reaches ClaimCore, the incident is converted into a claim.
Plain Text
IntakeConnect
|
v
Incident + Referral
|
v
ClaimCore
|
v
Incident Received
|
v
Validate Incident
|
v
Convert Incident to Claim
|
v
Claim Created
|
v
Referral Associated with Claim
|
v
Referral Displayed in ClaimCore UI
QA Validation Points
Testing validates:
Incident information
Claim association
Referral identifier
Referral information
Claim identifier
Referral-to-claim relationship
UI display
Data persistence
Duplicate referral prevention
The existing referral should become available when the incident is successfully converted into a claim.
4. Integration Flow 3 – Single Sign-On
An authorized user can navigate from ClaimCore to ReferralHub through SSO.
Plain Text
ClaimCore
|
v
Select Referral Function
|
v
SSO Request
|
v
Authentication / Authorization
|
v
ReferralHub
|
v
Correct User Context
|
v
Correct Claim / Referral Context
QA Validation Points
Testing validates:
Authorized user can initiate SSO.
User is not unnecessarily prompted for another login.
ReferralHub opens successfully.
Correct application context is passed.
Correct claim context is available.
Correct referral context is available when applicable.
Unauthorized access is prevented.
Invalid SSO requests are handled appropriately.
No actual authentication tokens or credentials will be included in this portfolio.
5. Integration Flow 4 – New Referral Created in ReferralHub
An authorized user may create a new referral in ReferralHub.
After creation, referral information is transmitted back to ClaimCore.
Plain Text
ClaimCore
|
| SSO
v
ReferralHub
|
v
Create New Referral
|
v
Referral Created
|
v
Referral Integration
|
v
ClaimCore
|
v
Locate Existing Claim
|
v
Add Referral
|
v
Referral Displayed in ClaimCore UI
QA Validation Points
Testing validates:
Referral is created in ReferralHub.
Referral receives the expected identifier.
Referral integration is triggered.
Correct referral information is transmitted.
ClaimCore receives the referral information.
Correct claim is identified.
Referral is associated with the correct claim.
Referral appears in ClaimCore UI.
UI data matches the integration data.
Duplicate referral is not created.
6. Integration Flow 5 – Claim Examiner Synchronization
After a referral exists, the examiner assigned to the claim can change in ClaimCore.
The new examiner information must be sent to ReferralHub.
Plain Text
Existing Claim
|
v
Existing Referral
|
v
Claim Examiner Changed
|
v
ClaimCore Saves New Examiner
|
v
Examiner Update Integration
|
v
ReferralHub
|
v
Locate Existing Referral
|
v
Update Referral Examiner
 
Example
Before:
Plain Text
Claim:
CLM-50001
 
Referral:
REF-70001
 
ClaimCore Examiner:
Examiner-A
 
ReferralHub Examiner:
Examiner-A
Update:
Plain Text
Examiner-A
↓
Examiner-B
Expected after integration:
Plain Text
ClaimCore Examiner:
Examiner-B
 
ReferralHub Examiner:
Examiner-B
 
Referral Count:
1
QA Validation Points
Testing validates:
Examiner change saves successfully in ClaimCore.
Examiner integration is triggered.
Correct claim/referral is referenced.
Correct new examiner information is transmitted.
ReferralHub locates the existing referral.
Existing referral is updated.
No new referral is created.
Old examiner is replaced appropriately.
ClaimCore and ReferralHub remain synchronized.
7. Integration Flow 6 – Referral Status Synchronization
Referral status may change in ReferralHub.
The changed status must be sent to ClaimCore.
Plain Text
Existing Referral
|
v
ReferralHub
|
v
Referral Status Changed
|
v
Status Update Integration
|
v
ClaimCore
|
v
Locate Existing Claim / Referral
|
v
Update Existing Referral
|
v
Display Updated Status in UI
QA Validation Points
Testing validates:
Status changes successfully in ReferralHub.
Status integration is triggered.
Correct referral identifier is transmitted.
Correct status is transmitted.
ClaimCore identifies the existing referral.
Existing referral is updated.
ClaimCore UI displays the new status.
No duplicate referral is created.
Expected consistency:
Plain Text
ReferralHub Status
=
Integration Status
=
ClaimCore Referral Status
=
ClaimCore UI Status
8. Bidirectional Integration Model
ReferralConnect includes multiple integration directions.
ReferralHub → IntakeConnect
Plain Text
Existing Referral Information
IntakeConnect → ClaimCore
Plain Text
Incident + Referral Information
ReferralHub → ClaimCore
Plain Text
New Referral
Referral Status Update
ClaimCore → ReferralHub
Plain Text
Claim Examiner Update
ClaimCore → ReferralHub through SSO
Plain Text
User / Claim / Referral Context
This creates the following conceptual flow:
Plain Text
IntakeConnect
↙ ↘
ReferralHub ClaimCore
↖ ↕
↖ ↕
└───────────┘
Bidirectional
Integration
9. API / Integration Validation Model
For each applicable integration, testing follows:
Plain Text
Business Trigger
↓
Integration Trigger
↓
Request / Message
↓
Field Validation
↓
Target System
↓
Processing Result
↓
Response / Acknowledgement
↓
Target Data
↓
UI Validation
Testing validates:
Correct trigger
Correct target
Required fields
Data types
Field mappings
Valid values
Missing values
Invalid values
Existing-record updates
Duplicate prevention
Error handling
Request/response correlation
UI reconciliation
10. Referral Data Validation
Representative fictional referral information may include:
Plain Text
Referral ID
Claim ID
Referral Type
Referral Status
Referral Date
Assigned Examiner
Vendor Reference
Created Date
Updated Date
Testing will validate the applicable fields across:
Plain Text
ReferralHub
↓
Integration
↓
ClaimCore
↓
ClaimCore UI
11. Existing Referral Reconciliation
For a referral already available during incident intake:
Plain Text
ReferralHub
↓
IntakeConnect
↓
Intake Integration
↓
ClaimCore
↓
Claim Created
↓
Referral UI
Expected:
The same referral identifier and applicable referral information remain traceable throughout the workflow.
12. New Referral Reconciliation
For a referral created after the claim exists:
Plain Text
ReferralHub Referral
=
Integration Referral
=
ClaimCore Referral
=
ClaimCore UI Referral
Testing verifies field-level consistency.
13. Examiner Reconciliation
After examiner reassignment:
Plain Text
ClaimCore Examiner
=
Outbound Integration Examiner
=
ReferralHub Examiner
The update must modify the existing ReferralHub referral.
14. Status Reconciliation
After a status change:
Plain Text
ReferralHub Status
=
Inbound Integration Status
=
ClaimCore Status
=
ClaimCore UI
15. Negative Integration Paths
The architecture will also support testing scenarios such as:
Existing Referral Not Found
Plain Text
IntakeConnect
↓
ReferralHub Search
↓
No Referral
Expected:
Incident processing follows the appropriate no-referral path.
Claim Not Found
Plain Text
ReferralHub
↓
Referral Integration
↓
ClaimCore
↓
Claim Not Found
Expected:
The referral is not incorrectly associated with another claim.
Duplicate Referral
Plain Text
Same Referral
↓
Integration Reprocessed
↓
ClaimCore
Expected:
An unintended duplicate referral is not created.
Invalid Examiner
Plain Text
ClaimCore
↓
Examiner Update
↓
ReferralHub
Expected:
Invalid examiner information follows the applicable validation/error process.
Unknown Referral Status
Plain Text
ReferralHub
↓
Unknown Status
↓
ClaimCore
Expected:
Unsupported status does not incorrectly overwrite valid ClaimCore referral information.
16. End-to-End Referral Lifecycle
The flagship integration workflow is:
Plain Text
Create Incident in IntakeConnect
↓
Retrieve Existing Referral from ReferralHub
↓
Include Referral in Intake Data
↓
Send Incident to ClaimCore
↓
Convert Incident to Claim
↓
Validate Referral on Claim
↓
SSO from ClaimCore to ReferralHub
↓
Create / Review Referral
↓
Referral Data Sent to ClaimCore
↓
Validate Referral in ClaimCore UI
↓
Change Claim Examiner
↓
Validate Examiner Update in ReferralHub
↓
Change Referral Status in ReferralHub
↓
Validate Status Update in ClaimCore
17. Architecture Testing Layers
ReferralConnect will be validated at multiple levels:
Application UI
SSO
API / Integration message
Request
Response
Field mapping
Application data
Existing-record update
Cross-system synchronization
End-to-end business workflow
18. Portfolio Disclaimer
ReferralConnect, IntakeConnect, ClaimCore, ReferralHub, integration services, application flows, claims, incidents, referrals, examiner names, statuses, interfaces, requests, responses, fields, authentication behavior, identifiers, business rules, and test data presented in this document are fictional or generalized.
This architecture demonstrates API, SSO, bidirectional integration, cross-application synchronization, UI/API reconciliation, and end-to-end QA practices.
No confidential employer, client, vendor, claimant, production endpoint, credential, token, API key, or proprietary system information is included.
