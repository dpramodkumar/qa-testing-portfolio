ReferralConnect Single Sign-On Test Cases
ClaimCore to ReferralHub SSO Integration
Project: ReferralConnect
Source Application: ClaimCore
Target Application: ReferralHub
Environment: QA Environment
Testing Type: SSO / Security / Integration / Context Validation

1. Purpose
This document defines detailed Single Sign-On (SSO) test cases for the fictional ReferralConnect project.

The objective is to verify that authorized ClaimCore users can securely access ReferralHub while maintaining the applicable user, claim, and referral context.

Testing includes:

Authorized access
Unauthorized access
User-context validation
Claim-context validation
Referral-context validation
Reauthentication behavior
Session handling
Invalid or expired SSO requests
Direct-link behavior
Cross-claim validation
Error handling
Regression testing
No real credentials, authentication tokens, cookies, certificates, endpoints, or security configuration are included.

2. SSO Business Flow
The fictional SSO workflow is:

ClaimCore ↓ Open Claim ↓ Select Referral ↓ Initiate SSO ↓ Authentication / Authorization ↓ ReferralHub ↓ Validate User Context ↓ Validate Claim Context ↓ Validate Referral Context ↓ ReferralHub Available to User

3. Common Test Data
Authorized User
User: QA-USER-A
Access: ClaimCore + ReferralHub

Unauthorized User
Plain Text
User: QA-USER-B
Access: ClaimCore only
Primary Claim
Plain Text
Claim: CLM-50001
Referral: REF-70001
Secondary Claim
Plain Text
Claim: CLM-50002
Referral: REF-70003
All users, claims, and referrals are fictional.
4. Authorized SSO Testing
RC-SSO-TC-001 – Authorized User SSO
Requirements:
RC-REQ-032
RC-REQ-033
RC-REQ-034
Related Scenario: RC-TS-013
 Test Type: Positive / SSO
Preconditions
QA-USER-A is authenticated in ClaimCore.
QA-USER-A has ReferralHub access.
CLM-50001 exists.
REF-70001 exists.
SSO integration is available.
Steps
Log into ClaimCore as QA-USER-A.
Search for CLM-50001.
Open the claim.
Navigate to the referral area.
Select REF-70001.
Initiate ReferralHub SSO.
Observe the application transition.
Verify ReferralHub opens.
Verify the user is not unnecessarily asked to perform another interactive login.
Verify the appropriate ReferralHub functionality is available.
Expected Result
SSO completes successfully.
ReferralHub opens for QA-USER-A.
An unnecessary second interactive login is not required.
The user receives only authorized functionality.
5. User Context Testing
RC-SSO-TC-002 – Validate User Context
Requirement: RC-REQ-035
 Related Scenario: RC-TS-013
 Test Type: Positive / Identity Context
Preconditions
RC-SSO-TC-001 prerequisites are satisfied.
Steps
Log into ClaimCore as QA-USER-A.
Initiate SSO to ReferralHub.
Review the user context available in ReferralHub.
Validate the applicable user information.
Verify authorized functions.
Verify restricted functions are not unintentionally accessible.
Expected Result
ReferralHub recognizes the appropriate authorized user context.
Permissions correspond to the fictional user's assigned access.
The user does not receive unintended elevated access.
6. Claim Context Testing
RC-SSO-TC-003 – Validate Claim Context
Requirement: RC-REQ-036
 Related Scenario: RC-TS-013
 Test Type: Positive / Integration
Test Data
Plain Text
Claim:
CLM-50001
Steps
Open CLM-50001 in ClaimCore.
Initiate SSO to ReferralHub.
Review the business context loaded in ReferralHub.
Verify the applicable claim reference.
Compare the source ClaimCore claim with the ReferralHub context.
Expected Result
Plain Text
ClaimCore Claim = CLM-50001
ReferralHub Context = CLM-50001
ReferralHub does not display an unrelated claim context.
7. Referral Context Testing
RC-SSO-TC-004 – Validate Referral Context
Requirement: RC-REQ-037
 Related Scenario: RC-TS-013
 Test Type: Positive / Integration
Test Data
Plain Text
Claim:
CLM-50001
 
Referral:
REF-70001
Steps
Open CLM-50001.
Navigate to the referral section.
Select REF-70001.
Initiate SSO.
Allow ReferralHub to load.
Verify the applicable referral.
Compare ClaimCore and ReferralHub referral identifiers.
Expected Result
Plain Text
ClaimCore Referral = REF-70001
ReferralHub Referral = REF-70001
The user does not land on another referral.
8. Cross-Claim Context Testing
RC-SSO-TC-005 – Validate Context Between Two Claims
Requirements:
RC-REQ-036
RC-REQ-037
Test Type: Integration / Data Integrity
Test Data
Plain Text
CLM-50001 → REF-70001
CLM-50002 → REF-70003
Steps
Open CLM-50001.
SSO to REF-70001.
Verify the ClaimCore and ReferralHub context.
Return to ClaimCore.
Open CLM-50002.
Select REF-70003.
Initiate a new SSO transition.
Verify the ReferralHub context.
Expected Result
First SSO:
Plain Text
CLM-50001
REF-70001
Second SSO:
Plain Text
CLM-50002
REF-70003
ReferralHub does not retain stale context from the previous claim.
9. Unauthorized Access Testing
RC-SSO-TC-006 – Unauthorized User SSO
Requirement: RC-REQ-038
 Related Scenario: RC-TS-014
 Test Type: Negative / Security
Preconditions
QA-USER-B can access ClaimCore but does not have the required ReferralHub permission.
Steps
Log into ClaimCore as QA-USER-B.
Open an eligible claim.
Attempt to initiate ReferralHub SSO.
Review the processing result.
Verify whether ReferralHub data is exposed.
Expected Result
Unauthorized access is prevented.
Restricted referral information is not exposed.
Appropriate access-denied behavior occurs.
Existing claim and referral data remain unchanged.
10. Invalid SSO Request Testing
RC-SSO-TC-007 – Invalid SSO Request
Requirement: RC-REQ-039
 Related Scenario: RC-TS-015
 Test Type: Negative / Security
Preconditions
An approved QA method exists to simulate an invalid SSO request.
Steps
Initiate the controlled invalid SSO condition.
Attempt access to ReferralHub.
Review the processing result.
Verify protected ReferralHub information.
Return to ClaimCore.
Verify the original claim/referral data.
Expected Result
Invalid SSO request is rejected.
Protected ReferralHub information is not displayed.
Claim/referral data remains unchanged.
Appropriate error behavior occurs.
11. Expired SSO Context Testing
RC-SSO-TC-008 – Expired SSO Request
Requirement: RC-REQ-039
 Test Type: Negative / Security
Preconditions
An approved QA mechanism exists to test expiration behavior.
Steps
Create or obtain the applicable QA SSO context.
Allow the context to become invalid according to the configured QA expiration rule.
Attempt to use the expired request.
Observe ReferralHub behavior.
Expected Result
Expired request is not accepted as valid.
Protected referral information is not exposed.
User receives the applicable expiration/error behavior.
12. Missing Claim Context Testing
RC-SSO-TC-009 – Missing Claim Context
Related Requirements:
RC-REQ-036
RC-REQ-039
RC-REQ-074
RC-REQ-079
Test Type: Negative / Context Validation
Steps
Use the approved QA method to initiate SSO without the required claim context.
Review the processing result.
Verify ReferralHub does not open an unrelated claim.
Verify no business record is modified.
Expected Result
Missing required context is detected.
An unrelated claim is not displayed.
Referral data is not associated incorrectly.
No unintended business update occurs.
13. Invalid Claim Context Testing
RC-SSO-TC-010 – Invalid Claim Context
Related Requirements:
RC-REQ-036
RC-REQ-091
Test Type: Negative / Data Integrity
Test Data
Plain Text
Claim:
CLM-99999
Steps
Initiate the approved QA SSO test using an invalid claim reference.
Observe ReferralHub processing.
Verify whether unrelated claim information is displayed.
Verify ClaimCore data remains unchanged.
Expected Result
The invalid claim does not resolve to another valid claim.
Unrelated referral information is not displayed.
Existing valid data remains intact.
14. Invalid Referral Context Testing
RC-SSO-TC-011 – Invalid Referral Context
Related Requirements:
RC-REQ-037
RC-REQ-092
Test Type: Negative / Data Integrity
Test Data
Plain Text
Referral:
REF-99999
Steps
Open a valid claim in ClaimCore.
Use the approved QA method to test an invalid referral reference.
Initiate SSO.
Review ReferralHub behavior.
Verify existing valid referral data.
Expected Result
Invalid referral reference does not resolve to an unrelated referral.
Protected referral data is not incorrectly displayed.
Existing referral information remains unchanged.
15. Claim/Referral Mismatch Testing
RC-SSO-TC-012 – Referral Does Not Belong to Claim
Requirements:
RC-REQ-036
RC-REQ-037
RC-REQ-075
Test Type: Negative / Data Integrity / Security
Test Data
Correct relationships:
Plain Text
CLM-50001 → REF-70001
CLM-50002 → REF-70003
Test mismatch:
Plain Text
CLM-50001 → REF-70003
Steps
Open CLM-50001.
Use the approved QA method to test REF-70003 as the referral context.
Initiate SSO.
Review ReferralHub behavior.
Verify no unrelated referral information is exposed.
Expected Result
The mismatched claim/referral combination does not incorrectly provide access to REF-70003 under CLM-50001.
16. Session Testing
RC-SSO-TC-013 – Active Session Behavior
Test Type: Positive / Session
Steps
Log into ClaimCore as an authorized QA user.
SSO to ReferralHub.
Verify ReferralHub opens.
Return to ClaimCore through the supported navigation.
Reinitiate SSO for the same eligible referral.
Review the resulting session behavior.
Expected Result
SSO continues to follow the approved session behavior without incorrect user or claim/referral context.
RC-SSO-TC-014 – Session Expiration
Test Type: Negative / Session / Security
Preconditions
QA environment supports session-expiration testing.
Steps
Establish a valid ClaimCore-to-ReferralHub session.
Allow the applicable QA session to expire.
Attempt an action requiring an active session.
Review system behavior.
Expected Result
Expired session is not treated as active.
User follows the applicable reauthentication or access flow.
Protected referral data remains protected.
17. Direct-Link Testing
RC-SSO-TC-015 – Direct ReferralHub Access Without Valid SSO Context
Test Type: Negative / Security
Steps
Use an approved QA ReferralHub link without creating the required ClaimCore SSO context.
Attempt to access restricted referral functionality.
Review behavior.
Expected Result
The direct path does not bypass required authentication, authorization, or business-context validation.
18. User-Switch Testing
RC-SSO-TC-016 – Different User After Previous SSO Session
Test Type: Security / Context Validation
Preconditions
QA-USER-A has completed a ReferralHub SSO session.
Steps
Complete authorized SSO as QA-USER-A.
End the applicable QA session using the supported workflow.
Sign into ClaimCore as another authorized QA user.
Open the applicable claim.
Initiate SSO.
Verify the ReferralHub user context.
Expected Result
ReferralHub reflects the current authorized user.
Previous-user context is not incorrectly reused.
19. SSO and Referral Update Separation
RC-SSO-TC-017 – SSO Does Not Create Duplicate Referral
Related Requirements:
RC-REQ-053
RC-REQ-055
RC-REQ-056
Test Type: Data Integrity
Preconditions
REF-70001 exists.
Steps
Record the referral count.
SSO from ClaimCore to REF-70001.
Review the referral.
Return to ClaimCore.
SSO to the same referral again.
Verify referral count.
Expected Result
Plain Text
Referral Count Before = 1
Referral Count After = 1
Navigation through SSO does not itself create another referral.
20. SSO Context Persistence Boundary
RC-SSO-TC-018 – New Claim Replaces Previous Context
Test Type: Integration / Context Testing
Steps
SSO from CLM-50001 to REF-70001.
Return to ClaimCore.
Open CLM-50002.
Select REF-70003.
Initiate SSO.
Verify the active ReferralHub context.
Expected Result
The second SSO uses:
Plain Text
CLM-50002
REF-70003
The previous:
Plain Text
CLM-50001
REF-70001
context is not incorrectly retained.
21. SSO Error Handling
RC-SSO-TC-019 – ReferralHub Unavailable During SSO
Related Requirement: RC-REQ-095
 Test Type: Negative / Availability
Preconditions
A controlled ReferralHub-unavailable QA condition exists.
Steps
Open an eligible claim in ClaimCore.
Initiate SSO while ReferralHub is unavailable.
Review ClaimCore behavior.
Review the processing result.
Reopen the claim.
Verify existing referral information.
Expected Result
Failure is handled appropriately.
ClaimCore data remains intact.
Existing referral association remains unchanged.
No duplicate business record is created.
Failure information supports troubleshooting.
22. SSO Regression
RC-SSO-TC-020 – SSO Regression Validation
Requirement: RC-REQ-103
 Scenario: RC-TS-056
 Test Type: Regression
Steps
Select an existing valid claim/referral combination.
Initiate SSO.
Verify authorized access.
Verify user context.
Verify claim context.
Verify referral context.
Return to ClaimCore.
Expected Result
Existing eligible SSO processing continues to operate without unintended regression.
23. SSO Test Coverage Summary
Test Case	CoverageRC-SSO-TC-001	Authorized SSO
RC-SSO-TC-002	User context
RC-SSO-TC-003	Claim context
RC-SSO-TC-004	Referral context
RC-SSO-TC-005	Cross-claim context
RC-SSO-TC-006	Unauthorized access
RC-SSO-TC-007	Invalid request
RC-SSO-TC-008	Expired request
RC-SSO-TC-009	Missing claim context
RC-SSO-TC-010	Invalid claim
RC-SSO-TC-011	Invalid referral
RC-SSO-TC-012	Claim/referral mismatch
RC-SSO-TC-013	Active session
RC-SSO-TC-014	Session expiration
RC-SSO-TC-015	Direct-link access
RC-SSO-TC-016	User-switch behavior
RC-SSO-TC-017	Duplicate prevention
RC-SSO-TC-018	Context replacement
RC-SSO-TC-019	Target unavailable
RC-SSO-TC-020	Regression
24. SSO Execution Evidence
For each test, sanitized evidence may contain:
Plain Text
Test Case ID
Synthetic User
Synthetic Claim ID
Synthetic Referral ID
Source Application
Target Application
Expected Context
Actual Context
Access Result
Session Result
Pass / Fail / Blocked / Not Executed
Defect ID
Never include:
Passwords
Authentication tokens
Cookies
Session identifiers
API keys
Client secrets
Certificates or private keys
Internal URLs
Real user identifiers
Real claim or referral identifiers
25. Traceability Example
Plain Text
RC-REQ-037
Referral Context
↓
RC-TS-013
Authorized SSO
↓
RC-SSO-TC-004
Validate Referral Context
↓
Execution Result
↓
Defect, if applicable
Security example:
Plain Text
RC-REQ-038
Unauthorized SSO Access
↓
RC-TS-014
Unauthorized SSO
↓
RC-SSO-TC-006
Unauthorized User SSO
↓
Execution Result
26. Portfolio Disclaimer
ReferralConnect, ClaimCore, ReferralHub, users, claims, referrals, SSO behavior, authorization rules, sessions, identifiers, error conditions, test data, and expected results presented here are fictional or generalized.
These cases demonstrate SSO testing techniques involving authentication, authorization, user context, business context, session behavior, negative testing, data integrity, error handling, and regression validation.
No confidential employer, client, vendor, production URL, credential, authentication token, certificate, session identifier, claimant information, or proprietary system information is included.
