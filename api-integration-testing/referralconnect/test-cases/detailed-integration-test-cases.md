ReferralConnect Detailed Integration Test Cases
Bidirectional API, SSO, and Referral Synchronization
Project: ReferralConnect
Applications: IntakeConnect, ClaimCore, ReferralHub
Environment: QA Environment
Testing Type: API / Integration / SSO / Functional / End-to-End

1. Purpose
This document contains curated, execution-ready test cases for the ReferralConnect project.

The cases validate:

Existing referral retrieval
Intake-to-claims integration
Incident-to-claim conversion
Single Sign-On
New referral integration
Examiner synchronization
Referral-status synchronization
Request and response validation
API-to-UI reconciliation
Duplicate prevention
Error handling
End-to-end referral processing
The cases remain technology-neutral because the portfolio does not assume an unconfirmed API protocol, transport, authentication mechanism, or payload format.

2. Common Test Data
Incident:
INC-40001

Claim:
CLM-50001

Existing Referral:
REF-70001

New Referral:
REF-70002

Original Examiner:
Examiner-A

Updated Examiner:
Examiner-B

Initial Referral Status:
OPEN

Updated Referral Status:
IN_PROGRESS

All identifiers and values are fictional.
3. Existing Referral Retrieval
RC-TC-001 – Retrieve Existing Referral During Intake
Requirements: RC-REQ-005 through RC-REQ-010
 Scenarios: RC-TS-001, RC-TS-005
 Direction: ReferralHub → IntakeConnect
 Test Type: Positive / Integration
Preconditions
INC-40001 is available in IntakeConnect.
REF-70001 exists in ReferralHub.
Referral search integration is available.
Steps
Open INC-40001 in IntakeConnect.
Enter the required referral-search information.
Trigger the referral search.
Capture or review the outbound search message.
Verify the incident/search identifiers.
Verify required search information is present.
Confirm ReferralHub receives the request.
Review the returned referral information.
Verify REF-70001 is returned.
Verify applicable referral details.
Save the incident.
Reopen INC-40001.
Verify REF-70001 remains associated.
Expected Result
Search is successfully processed.
REF-70001 is returned.
Referral details correspond to ReferralHub.
Referral is associated with INC-40001.
Referral information persists after the incident is reopened.
RC-TC-002 – No Existing Referral Found
Requirement: RC-REQ-012
 Scenario: RC-TS-003
 Test Type: Negative
Preconditions
Search criteria exists with no associated referral.
Steps
Create or open an eligible incident.
enter valid search criteria with no referral match.
Trigger the referral search.
Review the request.
Review the processing response.
Review the incident after processing.
Expected Result
The search completes without a matching referral.
An appropriate no-referral result is provided.
No unrelated referral is associated with the incident.
Incident data remains intact.
RC-TC-003 – Invalid Referral Search Request
Requirements: RC-REQ-013, RC-REQ-079
 Scenarios: RC-TS-004, RC-TS-029
 Test Type: Negative / Validation
Steps
Create a referral search request.
Omit one mandatory search value.
Submit the request.
Review the processing result.
Verify ReferralHub data.
Verify IntakeConnect incident data.
Expected Result
Request is not processed as a valid referral match.
Validation information identifies the processing problem.
No referral is incorrectly associated.
Existing incident information remains unchanged.
4. IntakeConnect-to-ClaimCore Integration
RC-TC-004 – Send Incident and Referral to ClaimCore
Requirements: RC-REQ-017 through RC-REQ-023
 Scenarios: RC-TS-007 through RC-TS-009
 Direction: IntakeConnect → ClaimCore
 Test Type: Positive / Integration
Preconditions
INC-40001 exists.
REF-70001 is associated with INC-40001.
Intake-to-ClaimCore integration is available.
Steps
Open INC-40001.
Verify REF-70001 is associated.
Complete all required intake information.
Submit the incident to ClaimCore.
Capture or review the integration message.
Verify incident identifier.
Verify referral identifier.
Verify applicable incident information.
Verify applicable referral information.
Confirm ClaimCore receives the incident.
Locate the incident in ClaimCore.
Compare source and target values.
Expected Result
ClaimCore receives INC-40001.
ClaimCore receives REF-70001.
Fields map accurately.
Referral information remains associated with the correct incident.
No unrelated claim or referral is updated.
RC-TC-005 – Intake Integration Failure
Requirement: RC-REQ-024
 Scenario: RC-TS-010
 Test Type: Negative / Error Handling
Preconditions
A controlled integration-failure condition is available.
Steps
Prepare an eligible incident and referral.
Trigger the intake integration under the failure condition.
Review the processing status.
Review error information.
Search ClaimCore for the incident.
Check for partially created claim/referral data.
Reopen the IntakeConnect incident.
Expected Result
Failure is identifiable.
Incomplete claim/referral data is not unintentionally created.
Existing incident data remains intact.
Sufficient information is available for troubleshooting.
5. Incident-to-Claim Conversion
RC-TC-006 – Convert Incident with Existing Referral
Requirements: RC-REQ-025 through RC-REQ-030
 Scenario: RC-TS-011
 Test Type: Positive / Integration / UI Validation
Preconditions
INC-40001 and REF-70001 exist in ClaimCore.
Steps
Open INC-40001 in ClaimCore.
Verify REF-70001 is available.
Complete required claim-conversion information.
Convert the incident into a claim.
Record the generated claim number.
Open the new claim.
Navigate to the referral section.
Verify REF-70001.
Verify referral details.
Save and exit the claim.
Reopen the claim.
Verify referral information persists.
Expected Result
A unique claim is created.
REF-70001 remains associated with the claim.
Referral information appears in ClaimCore UI.
Referral information persists after reopening.
RC-TC-007 – Prevent Duplicate Referral During Conversion
Requirement: RC-REQ-031
 Scenario: RC-TS-012
 Test Type: Negative / Data Integrity
Preconditions
INC-40001 contains one REF-70001 referral.
Steps
Record the referral count before conversion.
Convert INC-40001 into a claim.
Open the claim referral section.
Count referrals associated with the claim.
Search specifically for REF-70001.
Expected Result
Plain Text
Referral Count Before = 1
Referral Count After = 1
Only one REF-70001 record exists.
6. Single Sign-On
RC-TC-008 – Authorized SSO to ReferralHub
Requirements: RC-REQ-032 through RC-REQ-037
 Scenario: RC-TS-013
 Test Type: Positive / SSO
Preconditions
Authorized ClaimCore user is signed in.
CLM-50001 contains REF-70001.
SSO integration is available.
Steps
Open CLM-50001.
Select REF-70001.
Initiate SSO to ReferralHub.
Verify ReferralHub opens successfully.
Verify another interactive login is not unnecessarily required.
Verify user context.
Verify claim context.
Verify referral context.
Expected Result
Authorized user enters ReferralHub successfully.
The correct user, claim, and referral context is available.
REF-70001 is displayed rather than an unrelated referral.
RC-TC-009 – Unauthorized SSO Access
Requirement: RC-REQ-038
 Scenario: RC-TS-014
 Test Type: Negative / Security
Preconditions
User lacks the required ReferralHub permission.
Steps
Sign in to ClaimCore as the unauthorized test user.
Open an eligible claim.
Attempt SSO to ReferralHub.
Review the processing result.
Expected Result
Restricted ReferralHub access is prevented.
Appropriate access information is displayed.
No unauthorized referral information is exposed.
RC-TC-010 – Invalid or Expired SSO Request
Requirement: RC-REQ-039
 Scenario: RC-TS-015
 Test Type: Negative / Security
Steps
Use the approved QA method to create an invalid or expired SSO condition.
Initiate SSO.
Review ReferralHub behavior.
Verify existing claim/referral data.
Expected Result
Invalid SSO request is rejected.
Restricted referral data is not displayed.
Existing ClaimCore and ReferralHub data remains unchanged.
7. New Referral Integration
RC-TC-011 – Create New Referral in ReferralHub
Requirements: RC-REQ-040 through RC-REQ-044
 Scenario: RC-TS-016
 Test Type: Positive / Functional
Preconditions
CLM-50001 exists.
Authorized user can access ReferralHub.
Steps
Access ReferralHub using the approved flow.
Create a referral for CLM-50001.
Enter all mandatory referral information.
Save the referral.
Record the generated referral identifier.
Reopen the referral.
Expected Result
Referral is created successfully.
Unique referral identifier is generated.
Required information is saved.
CLM-50001 reference is retained.
Referral persists after reopening.
RC-TC-012 – Send New Referral to ClaimCore
Requirements: RC-REQ-045 through RC-REQ-051
 Scenarios: RC-TS-017, RC-TS-018
 Direction: ReferralHub → ClaimCore
 Test Type: Positive / API / Integration
Preconditions
REF-70002 has been created for CLM-50001.
Steps
Complete the integration-triggering referral action.
Capture or review the outbound integration transaction.
Verify REF-70002.
Verify CLM-50001.
Verify required referral fields.
Confirm ClaimCore receives the transaction.
Open CLM-50001.
Navigate to the referral section.
Locate REF-70002.
Compare ClaimCore values with ReferralHub.
Expected Result
Integration is triggered.
ClaimCore locates CLM-50001.
REF-70002 is associated with the correct claim.
Applicable fields map correctly.
Referral appears in ClaimCore.
RC-TC-013 – API-to-UI Referral Reconciliation
Requirements: RC-REQ-052, RC-REQ-086, RC-REQ-100
 Scenarios: RC-TS-019, RC-TS-039, RC-TS-053
 Test Type: API / UI Reconciliation
Steps
Record REF-70002 information in ReferralHub.
Record values in the integration transaction.
Open CLM-50001 in ClaimCore.
Locate REF-70002.
Compare the applicable fields.
Exit and reopen the claim.
Repeat the UI validation.
Expected Result
Plain Text
ReferralHub Data
=
Integration Data
=
ClaimCore Data
=
ClaimCore UI
The referral remains available after the claim is reopened.
RC-TC-014 – Reprocess Existing Referral Integration
Requirements: RC-REQ-053 through RC-REQ-056
 Scenario: RC-TS-020
 Test Type: Negative / Duplicate Prevention
Preconditions
REF-70002 already exists on CLM-50001.
Steps
Record the current referral count.
Reprocess the same applicable integration.
Review processing status.
Reopen CLM-50001.
Count referrals.
Search for REF-70002.
Expected Result
Plain Text
Referral Count Before = 1
Referral Count After = 1
The existing referral is recognized and no duplicate is created.
8. Examiner Synchronization
RC-TC-015 – Synchronize Updated Examiner
Requirements: RC-REQ-057 through RC-REQ-063
 Scenarios: RC-TS-021 through RC-TS-023
 Direction: ClaimCore → ReferralHub
 Test Type: Positive / Bidirectional Integration
Preconditions
Plain Text
Claim: CLM-50001
Referral: REF-70001
ClaimCore Examiner: Examiner-A
ReferralHub Examiner: Examiner-A
Steps
Open CLM-50001 in ClaimCore.
Verify REF-70001.
Change examiner from Examiner-A to Examiner-B.
Save the claim.
Verify Examiner-B persists in ClaimCore.
Capture or review the outbound examiner transaction.
Verify claim identifier.
Verify referral identifier.
Verify updated examiner information.
Open REF-70001 in ReferralHub.
Verify the displayed examiner.
Expected Result
Plain Text
ClaimCore Examiner = Examiner-B
ReferralHub Examiner = Examiner-B
 
The existing REF-70001 referral is updated.
RC-TC-016 – Examiner Change Does Not Duplicate Referral
Requirement: RC-REQ-064
 Scenario: RC-TS-024
 Test Type: Negative / Data Integrity
Steps
Record ReferralHub referral count before examiner change.
Change the ClaimCore examiner.
Allow the integration to process.
Count ReferralHub referrals after processing.
Verify REF-70001.
Expected Result
Plain Text
Referral Count Before = 1
Referral Count After = 1
Only examiner information changes.
RC-TC-017 – Invalid Examiner Information
Requirement: RC-REQ-093
 Scenario: RC-TS-046
 Test Type: Negative / Validation
Steps
Use the approved QA method to submit invalid examiner information.
Review the outbound integration.
Review ReferralHub processing.
Verify the existing referral.
Verify the original valid examiner data.
Expected Result
Invalid examiner information follows the applicable error process.
An unrelated examiner is not assigned.
Existing valid referral data is not corrupted.
No duplicate referral is created.
9. Referral Status Synchronization
RC-TC-018 – Synchronize Referral Status
Requirements: RC-REQ-065 through RC-REQ-072
 Scenarios: RC-TS-025 through RC-TS-027
 Direction: ReferralHub → ClaimCore
 Test Type: Positive / Integration / UI Reconciliation
Preconditions
Plain Text
Claim: CLM-50001
Referral: REF-70001
Current Status: OPEN
Steps
Open REF-70001 in ReferralHub.
Change status from OPEN to IN_PROGRESS.
Save the referral.
Verify the status persists.
Capture or review the status integration.
Verify referral identifier.
Verify updated status.
Confirm ClaimCore receives the update.
Open CLM-50001.
Locate REF-70001.
Verify the status in ClaimCore UI.
Reopen the claim and verify again.
Expected Result
Plain Text
ReferralHub Status = IN_PROGRESS
Integration Status = IN_PROGRESS
ClaimCore Status = IN_PROGRESS
ClaimCore UI Status = IN_PROGRESS
RC-TC-019 – Status Change Does Not Duplicate Referral
Requirement: RC-REQ-073
 Scenario: RC-TS-028
 Test Type: Negative / Data Integrity
Steps
Record referral count before the status update.
Change the ReferralHub status.
Allow the integration to process.
Record referral count after processing.
Verify the existing referral status.
Expected Result
Plain Text
Referral Count Before = 1
Referral Count After = 1
The existing referral is updated.
RC-TC-020 – Unsupported Referral Status
Requirement: RC-REQ-094
 Scenario: RC-TS-047
 Test Type: Negative / Validation
Steps
Use the approved QA method to submit an unsupported status.
Review integration processing.
Open CLM-50001.
Verify REF-70001.
Review the existing valid status.
Expected Result
Unsupported status is not applied as a valid ClaimCore status.
Existing valid status is not incorrectly overwritten.
Appropriate error information is available.
10. Identifier and Field Validation
RC-TC-021 – Validate Mandatory Fields and Identifiers
Requirements: RC-REQ-074, RC-REQ-075, RC-REQ-079
 Scenarios: RC-TS-029, RC-TS-030
 Test Type: Positive / Negative / Data Validation
Steps
Capture a valid referral integration transaction.
Verify required incident, claim, and referral identifiers.
Verify mandatory business fields.
Submit a controlled test transaction missing one mandatory field.
Review the processing result.
Verify target-system data.
Expected Result
Valid transaction processes successfully.
Transaction missing mandatory information is not incorrectly processed.
Missing value is identifiable.
No unintended target record is created or updated.
RC-TC-022 – Validate Data Type, Length, and Null Handling
Requirements: RC-REQ-076 through RC-REQ-078
 Scenarios: RC-TS-031 through RC-TS-033
 Test Type: Negative / Boundary
Test Variations
Invalid data type
Overlength value
Null optional field
Null mandatory field
Expected Result
Valid optional null follows the applicable rule.
Invalid data type is not incorrectly processed.
Overlength value follows the applicable validation process.
Mandatory null is rejected.
Existing valid target data remains intact.
11. Error and Recovery Testing
RC-TC-023 – Unknown Claim or Referral
Requirements: RC-REQ-090 through RC-REQ-092
 Scenarios: RC-TS-043 through RC-TS-045
 Test Type: Negative / Data Integrity
Test Variations
Referral not found
Claim not found
Invalid referral identifier
Expected Result
Update is not applied to an unrelated claim or referral.
No partial target information is created.
Error information identifies the failed transaction.
RC-TC-024 – Integration Unavailable
Requirement: RC-REQ-095
 Scenario: RC-TS-048
 Test Type: Negative / Integration Failure
Preconditions
A controlled integration-unavailable condition exists.
Steps
Trigger an eligible referral update.
Observe processing during service unavailability.
Verify source data.
Verify target data.
Review processing/error information.
Expected Result
Source business data remains intact.
Target application does not contain an unintended partial update.
Failure information is available.
Transaction is not falsely reported as successfully completed.
RC-TC-025 – Reprocess Corrected Transaction
Requirement: RC-REQ-096
 Scenario: RC-TS-049
 Test Type: Recovery / Integration
Preconditions
A previously failed eligible transaction exists.
Steps
Identify the original failed transaction.
Correct the underlying test-data or configuration problem.
Reprocess using the supported QA workflow.
Review processing status.
Verify target data.
Check for duplicate referrals.
Expected Result
Corrected transaction processes successfully.
Target data is accurate.
Existing business record is updated appropriately.
No unintended duplicate is created.
12. End-to-End Referral Lifecycle
RC-TC-026 – Complete ReferralConnect Lifecycle
Requirements: RC-REQ-107 through RC-REQ-111
 Scenarios: RC-TS-E2E-001 through RC-TS-E2E-005
 Test Type: End-to-End / Integration / SSO / UI
Steps
Create INC-40001 in IntakeConnect.
Trigger existing referral search.
Retrieve REF-70001 from ReferralHub.
Associate REF-70001 with INC-40001.
Send incident and referral information to ClaimCore.
Verify ClaimCore receives INC-40001 and REF-70001.
Convert the incident into CLM-50001.
Verify REF-70001 appears on the claim.
SSO from ClaimCore to ReferralHub.
Verify user, claim, and referral context.
Create or process REF-70002.
Verify REF-70002 reaches ClaimCore.
Verify REF-70002 in ClaimCore UI.
Change examiner from Examiner-A to Examiner-B.
Verify examiner-update integration.
Verify Examiner-B on the existing ReferralHub referral.
Change REF-70001 status to IN_PROGRESS.
Verify status integration to ClaimCore.
Verify IN_PROGRESS in ClaimCore UI.
Verify referral counts.
Reopen the claim.
Verify referrals, examiner, and status persist.
Expected Result
Existing referral remains traceable from intake through claim conversion.
SSO works with the applicable context.
New referral reaches the correct claim.
Examiner synchronization completes.
Status synchronization completes.
UI values match integration values.
No unintended duplicate referrals exist.
13. Traceability Summary
Test Case	Primary CoverageRC-TC-001	Existing referral retrieval
RC-TC-002	No referral found
RC-TC-003	Invalid search
RC-TC-004	Intake-to-ClaimCore integration
RC-TC-005	Intake integration failure
RC-TC-006	Incident-to-claim conversion
RC-TC-007	Conversion duplicate prevention
RC-TC-008	Authorized SSO
RC-TC-009	Unauthorized SSO
RC-TC-010	Invalid/expired SSO
RC-TC-011	Referral creation
RC-TC-012	New referral integration
RC-TC-013	API-to-UI reconciliation
RC-TC-014	Referral reprocessing
RC-TC-015	Examiner synchronization
RC-TC-016	Examiner duplicate prevention
RC-TC-017	Invalid examiner
RC-TC-018	Status synchronization
RC-TC-019	Status duplicate prevention
RC-TC-020	Unsupported status
RC-TC-021	Mandatory fields and identifiers
RC-TC-022	Type, length, and null validation
RC-TC-023	Unknown claim/referral
RC-TC-024	Integration unavailable
RC-TC-025	Corrected-transaction reprocessing
RC-TC-026	Complete referral lifecycle
14. Execution Record Template
For each execution, record:
Plain Text
Execution ID:
Test Case ID:
Execution Date:
Environment:
Tester:
Test Data:
Actual Result:
Status: Pass / Fail / Blocked / Not Executed
Defect ID:
Evidence:
Comments:
15. Portfolio Disclaimer
ReferralConnect, IntakeConnect, ClaimCore, ReferralHub, integration services, incidents, claims, referrals, examiner information, statuses, identifiers, fields, requests, responses, test data, expected results, and error conditions are fictional or generalized.
These cases demonstrate API, integration, SSO, field-mapping, negative, duplicate-prevention, recovery, UI-reconciliation, and end-to-end testing practices.
No confidential employer, client, vendor, claimant, production endpoint, credential, token, API key, certificate, or proprietary system information is included.
