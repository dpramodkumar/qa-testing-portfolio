ReferralConnect API & Integration Test Scenarios
Bidirectional Referral Integration Testing
Project: ReferralConnect
Applications: IntakeConnect, ClaimCore, ReferralHub
Environment: QA Environment
Testing: API / Integration / SSO / UI Reconciliation / End-to-End

1. Purpose
This document defines the high-level API and integration test scenarios for ReferralConnect.

Testing validates four primary data-integration directions:

ReferralHub → IntakeConnect
IntakeConnect → ClaimCore
ReferralHub → ClaimCore
ClaimCore → ReferralHub
Additional coverage includes:

Single Sign-On
Existing-record updates
Duplicate prevention
Negative testing
Error handling
Cross-system synchronization
API-to-UI reconciliation
End-to-end referral lifecycle testing
2. Test Data
All test data is fictional.

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

3. Existing Referral Retrieval Scenarios
RC-TS-001 – Retrieve Existing Referral During Intake
Requirements: RC-REQ-005 through RC-REQ-010
Direction: ReferralHub → IntakeConnect
Test Type: Positive / API / Integration
Scenario
Verify IntakeConnect retrieves an applicable existing referral from ReferralHub during incident processing.
Expected Result
Referral search is triggered.
Correct referral is returned.
Referral identifier is correct.
Applicable referral details are returned.
Referral is associated with the correct incident.
RC-TS-002 – Multiple Existing Referrals
Requirement: RC-REQ-011
 Test Type: Positive / Data Validation
Verify processing when ReferralHub returns multiple applicable referral records.
Expected Result
The returned referrals are handled according to the defined business rules and are not incorrectly associated with another incident.
RC-TS-003 – No Existing Referral
Requirement: RC-REQ-012
 Test Type: Negative
Verify behavior when no referral exists for the search criteria.
Expected Result
Appropriate no-referral result is returned.
No unrelated referral is associated with the incident.
Intake processing follows the applicable no-referral path.
RC-TS-004 – Invalid Referral Search
Requirement: RC-REQ-013
 Test Type: Negative / Validation
Verify an incomplete or invalid referral search follows the applicable validation/error process.
4. Intake Referral Association Scenarios
RC-TS-005 – Associate Referral to Correct Incident
Requirements: RC-REQ-014 through RC-REQ-016
Test Type: Integration / Data Integrity
Verify the referral returned by ReferralHub is associated with the correct IntakeConnect incident.
Expected Result
Plain Text
INC-40001
↓
REF-70001
REF-70001 is not associated with an unrelated incident.
RC-TS-006 – Referral Persists Through Intake
Requirement: RC-REQ-015
 Test Type: Functional / Integration
Verify associated referral information remains available throughout the applicable intake workflow.
5. IntakeConnect to ClaimCore Scenarios
RC-TS-007 – Send Incident and Referral to ClaimCore
Requirements: RC-REQ-017 through RC-REQ-023
Direction: IntakeConnect → ClaimCore
Test Type: Positive / Integration
Scenario
Verify a completed incident containing existing referral information is successfully transmitted to ClaimCore.
Validate
Incident identifier
Referral identifier
Incident information
Referral information
Field mapping
Target-system association
Expected Result
ClaimCore receives the expected incident and referral information.
RC-TS-008 – Validate Incident Field Mapping
Requirements: RC-REQ-020, RC-REQ-021
Test Type: Field Mapping
Compare applicable IntakeConnect values against ClaimCore.
Expected Result
Mapped fields contain the expected values.
RC-TS-009 – Validate Referral Identifier Mapping
Requirement: RC-REQ-023
 Test Type: Data Integrity
Verify REF-70001 received by ClaimCore is the same referral returned earlier by ReferralHub.
RC-TS-010 – Intake Integration Failure
Requirement: RC-REQ-024
 Test Type: Negative / Error Handling
Verify failed intake integration does not create an unintended partial claim/referral association.
6. Incident-to-Claim Conversion Scenarios
RC-TS-011 – Convert Incident with Existing Referral
Requirements: RC-REQ-025 through RC-REQ-030
Test Type: Positive / Integration
Scenario
Convert INC-40001 into a claim after the incident and existing referral have reached ClaimCore.
Expected Result
Incident converts successfully.
Unique claim number is generated.
REF-70001 remains associated.
Referral is displayed in the ClaimCore UI.
Referral persists after reopening the claim.
RC-TS-012 – Prevent Duplicate Referral During Conversion
Requirement: RC-REQ-031
 Test Type: Negative / Data Integrity
Expected Result
Incident-to-claim conversion does not create a second REF-70001 record.
7. SSO Scenarios
RC-TS-013 – Authorized SSO
Requirements: RC-REQ-032 through RC-REQ-037
Direction: ClaimCore → ReferralHub
Test Type: Positive / SSO
Expected Result
Authorized user can initiate SSO.
ReferralHub opens successfully.
Applicable user context is available.
Applicable claim context is available.
Applicable referral context is available.
Unnecessary second interactive login is not required.
RC-TS-014 – Unauthorized SSO
Requirement: RC-REQ-038
 Test Type: Negative / Security
Verify a user without applicable authorization cannot access restricted ReferralHub functionality through SSO.
RC-TS-015 – Invalid SSO Request
Requirement: RC-REQ-039
 Test Type: Negative / Security
Verify an invalid or expired SSO request follows the appropriate error process.
8. New Referral Creation Scenarios
RC-TS-016 – Create New Referral in ReferralHub
Requirements: RC-REQ-040 through RC-REQ-044
Test Type: Positive / Functional
Expected Result
Referral is created.
Unique referral identifier is assigned.
Required information is saved.
Applicable claim reference is retained.
9. ReferralHub to ClaimCore Integration Scenarios
RC-TS-017 – Send New Referral to ClaimCore
Requirements: RC-REQ-045 through RC-REQ-051
Direction: ReferralHub → ClaimCore
Test Type: Positive / API / Integration
Scenario
Create REF-70002 in ReferralHub and validate downstream integration to ClaimCore.
Expected Result
Referral integration is triggered.
Correct claim reference is transmitted.
ClaimCore locates CLM-50001.
REF-70002 is associated with CLM-50001.
Referral is displayed in ClaimCore.
RC-TS-018 – Validate Referral Field Mapping
Requirement: RC-REQ-050
 Test Type: Field Mapping
Compare representative values:
Plain Text
ReferralHub
↓
Integration
↓
ClaimCore
Expected Result
Applicable referral fields map correctly.
RC-TS-019 – API-to-UI Referral Validation
Requirements: RC-REQ-051, RC-REQ-052
Test Type: API / UI Reconciliation
Verify:
Plain Text
ReferralHub Data
=
Integration Data
=
ClaimCore Data
=
ClaimCore UI
10. Duplicate Prevention Scenarios
RC-TS-020 – Reprocess Same Referral Integration
Requirements: RC-REQ-053 through RC-REQ-056
Test Type: Negative / Data Integrity
Preconditions
REF-70002 already exists on CLM-50001.
Steps
Reprocess the applicable referral integration.
Expected Result
Plain Text
Referral Count Before = 1
Referral Count After = 1
 
The existing referral is identified appropriately.
11. Examiner Synchronization Scenarios
RC-TS-021 – Change Claim Examiner
Requirements: RC-REQ-057, RC-REQ-058
Test Type: Positive / Functional
Scenario
Change the examiner for CLM-50001:
Plain Text
Examiner-A
↓
Examiner-B
Expected Result
Examiner-B is successfully saved in ClaimCore.
RC-TS-022 – Trigger Examiner Integration
Requirements: RC-REQ-059 through RC-REQ-061
Direction: ClaimCore → ReferralHub
Test Type: API / Integration
Expected Result
Examiner change triggers integration.
REF-70001 is correctly identified.
Updated examiner information is transmitted.
RC-TS-023 – Update ReferralHub Examiner
Requirements: RC-REQ-062, RC-REQ-063
Test Type: Integration / Data Synchronization
Verify ReferralHub updates the existing referral.
Expected Result
Plain Text
ClaimCore Examiner = Examiner-B
ReferralHub Examiner = Examiner-B
RC-TS-024 – Examiner Update Does Not Duplicate Referral
Requirement: RC-REQ-064
 Test Type: Negative / Data Integrity
Expected Result
Plain Text
Referral Count Before = 1
Referral Count After = 1
Only examiner information is updated.
12. Referral Status Synchronization Scenarios
RC-TS-025 – Change Referral Status in ReferralHub
Requirements: RC-REQ-065, RC-REQ-066
Test Type: Positive / Functional
Verify an eligible referral status can be changed and saved in ReferralHub.
RC-TS-026 – Trigger Referral Status Integration
Requirements: RC-REQ-067 through RC-REQ-069
Direction: ReferralHub → ClaimCore
Test Type: API / Integration
Expected Result
Status change triggers integration.
Correct referral identifier is transmitted.
Correct status is transmitted.
ClaimCore identifies the existing referral.
RC-TS-027 – Update ClaimCore Referral Status
Requirements: RC-REQ-070 through RC-REQ-072
Test Type: Integration / UI Reconciliation
Expected:
Plain Text
ReferralHub Status
=
Integration Status
=
ClaimCore Status
=
ClaimCore UI Status
RC-TS-028 – Status Update Does Not Duplicate Referral
Requirement: RC-REQ-073
 Test Type: Negative / Data Integrity
Verify the status update modifies the existing referral.
Expected Result
Referral count does not increase.
13. Integration Field Validation Scenarios
RC-TS-029 – Validate Mandatory Fields
Requirements: RC-REQ-074, RC-REQ-079
Test Type: Positive / Negative
Verify applicable integration messages include required business fields.
Verify processing behavior when one required field is missing.
RC-TS-030 – Validate Identifiers
Requirement: RC-REQ-075
 Test Type: Data Integrity
Validate applicable:
Incident ID
Claim ID
Referral ID
across source and target systems.
RC-TS-031 – Validate Data Types
Requirement: RC-REQ-076
 Test Type: Negative / Data Validation
Verify values with invalid data types are not incorrectly processed.
RC-TS-032 – Validate Field Length
Requirement: RC-REQ-077
 Test Type: Boundary
Verify processing behavior when a value exceeds the applicable supported field length.
RC-TS-033 – Validate Null Handling
Requirement: RC-REQ-078
 Test Type: Negative / Data Validation
Verify optional null fields and mandatory null fields are handled according to their respective rules.
RC-TS-034 – Validate Field Mapping
Requirement: RC-REQ-080
 Test Type: Data Mapping
Compare source and target values for representative referral fields.
14. Integration Response Scenarios
RC-TS-035 – Successful Integration Result
Requirement: RC-REQ-081
 Test Type: Positive
Verify successful processing returns or records the applicable success result/acknowledgement.
RC-TS-036 – Failed Integration Result
Requirements: RC-REQ-082, RC-REQ-083
Test Type: Negative / Error Handling
Verify failed processing provides sufficient information to identify and troubleshoot the transaction.
RC-TS-037 – Prevent Partial Update
Requirement: RC-REQ-084
 Test Type: Negative / Data Integrity
Verify failed processing does not leave unintended partial referral information in the receiving application.
15. Cross-System Synchronization Scenarios
RC-TS-038 – Referral Identifier Consistency
Requirement: RC-REQ-085
 Test Type: Cross-System Validation
Verify the referral identifier remains traceable across applicable applications.
RC-TS-039 – Referral Data Consistency
Requirement: RC-REQ-086
 Test Type: Integration / Data Reconciliation
Compare applicable referral information in ReferralHub and ClaimCore.
RC-TS-040 – Examiner Consistency
Requirement: RC-REQ-087
 Test Type: Cross-System Validation
Expected:
Plain Text
ClaimCore Examiner = ReferralHub Examiner
after successful integration.
RC-TS-041 – Referral Status Consistency
Requirement: RC-REQ-088
 Test Type: Cross-System Validation
Expected:
Plain Text
ReferralHub Status = ClaimCore Status
after successful integration.
RC-TS-042 – Intake-to-Claim Referral Traceability
Requirement: RC-REQ-089
 Test Type: End-to-End Data Traceability
Trace REF-70001 from:
Plain Text
ReferralHub
↓
IntakeConnect
↓
ClaimCore Incident
↓
Converted Claim
↓
ClaimCore UI
16. Error and Exception Scenarios
RC-TS-043 – Referral Not Found
Requirement: RC-REQ-090
 Test Type: Negative
Verify appropriate behavior when an expected referral cannot be located.
RC-TS-044 – Claim Not Found
Requirement: RC-REQ-091
 Test Type: Negative / Data Integrity
Verify an incoming referral update referencing an unknown claim is not associated with another claim.
RC-TS-045 – Invalid Referral Identifier
Requirement: RC-REQ-092
 Test Type: Negative
Verify an invalid referral ID cannot update another referral.
RC-TS-046 – Invalid Examiner
Requirement: RC-REQ-093
 Test Type: Negative
Verify invalid examiner information follows the applicable validation/error process.
RC-TS-047 – Unsupported Referral Status
Requirement: RC-REQ-094
 Test Type: Negative
Verify an unsupported status does not incorrectly replace valid ClaimCore referral status.
RC-TS-048 – Integration Unavailable
Requirement: RC-REQ-095
 Test Type: Negative / Integration Failure
Verify unavailable integration does not create incomplete cross-system information.
RC-TS-049 – Reprocess Corrected Transaction
Requirement: RC-REQ-096
 Test Type: Recovery / Integration
Where supported, correct the underlying issue and reprocess the failed transaction.
Expected Result
The corrected transaction processes successfully without creating an unintended duplicate.
17. Traceability and Audit Scenarios
RC-TS-050 – Referral Integration Traceability
Requirement: RC-REQ-097
Verify an applicable referral transaction can be traced across source, integration, and target.
RC-TS-051 – Examiner Update Traceability
Requirement: RC-REQ-098
Trace:
Plain Text
ClaimCore Examiner Change
↓
Integration
↓
ReferralHub Update
RC-TS-052 – Status Update Traceability
Requirement: RC-REQ-099
Trace:
Plain Text
ReferralHub Status Change
↓
Integration
↓
ClaimCore Update
RC-TS-053 – Integration-to-UI Traceability
Requirement: RC-REQ-100
Verify data received through integration can be traced to the applicable ClaimCore UI referral record.
18. Regression Scenarios
RC-TS-054 – Incident Processing Regression
Requirement: RC-REQ-101
Verify existing eligible incident creation and transmission continues to operate.
RC-TS-055 – Claim Conversion Regression
Requirement: RC-REQ-102
Verify existing eligible incident-to-claim conversion continues to operate.
RC-TS-056 – SSO Regression
Requirement: RC-REQ-103
Verify existing eligible SSO behavior continues to operate.
RC-TS-057 – Referral Processing Regression
Requirement: RC-REQ-104
Verify existing referral creation and display behavior continues to operate.
RC-TS-058 – Examiner Regression
Requirement: RC-REQ-105
Verify standard examiner assignment functionality continues to operate.
RC-TS-059 – Referral Status Regression
Requirement: RC-REQ-106
Verify standard referral-status functionality continues to operate.
19. End-to-End Test Scenarios
RC-TS-E2E-001 – Existing Referral Intake Lifecycle
Requirement: RC-REQ-107
Plain Text
Create Incident
↓
Retrieve Existing Referral
↓
Associate Referral to Incident
↓
Send Incident + Referral to ClaimCore
↓
Convert Incident to Claim
↓
Validate Referral on Claim
↓
Validate ClaimCore UI
Expected Result
The same existing referral remains traceable throughout the complete intake-to-claim workflow.
RC-TS-E2E-002 – New Referral Lifecycle
Requirement: RC-REQ-108
Plain Text
ClaimCore
↓
SSO to ReferralHub
↓
Create Referral
↓
Referral Integration
↓
ClaimCore
↓
Referral Displayed in UI
Expected Result
The newly created referral is associated with the correct existing claim and displayed correctly.
RC-TS-E2E-003 – Examiner Synchronization Lifecycle
Requirement: RC-REQ-109
Plain Text
Existing Claim + Referral
↓
Change Examiner in ClaimCore
↓
Integration Trigger
↓
ReferralHub
↓
Existing Referral Updated
↓
Verify Examiner
↓
Verify Referral Count
Expected Result
The existing referral reflects the new examiner and no duplicate referral is created.
RC-TS-E2E-004 – Referral Status Lifecycle
Requirement: RC-REQ-110
Plain Text
Existing Referral
↓
Change Status in ReferralHub
↓
Integration Trigger
↓
ClaimCore
↓
Existing Referral Updated
↓
Updated Status Displayed in UI
Expected Result
ReferralHub and ClaimCore display consistent referral status.
RC-TS-E2E-005 – Complete ReferralConnect Lifecycle
Requirement: RC-REQ-111
Plain Text
Create Incident
↓
Retrieve Existing Referral
↓
Send Intake to ClaimCore
↓
Convert Incident to Claim
↓
Validate Existing Referral
↓
SSO to ReferralHub
↓
Create / Process Referral
↓
Validate Referral in ClaimCore
↓
Change Examiner in ClaimCore
↓
Validate Examiner in ReferralHub
↓
Change Status in ReferralHub
↓
Validate Status in ClaimCore
↓
Verify No Duplicate Referrals
Expected Result
All applicable referral, examiner, status, claim, and integration information remains consistent through the complete lifecycle.
20. Scenario Coverage Summary
The test scenarios cover:
Existing referral retrieval
Incident/referral association
Intake-to-claims integration
Incident-to-claim conversion
SSO
Referral creation
Vendor-to-claims integration
API-to-UI validation
Examiner synchronization
Claims-to-vendor integration
Referral status synchronization
Existing-record updates
Duplicate prevention
Mandatory fields
Data types
Field lengths
Null handling
Field mapping
Successful/failed processing
Error handling
Reprocessing
Audit traceability
Regression
End-to-end lifecycle testing
21. Traceability Example
Plain Text
RC-REQ-070
Existing Referral Status Update
↓
RC-TS-027
Update ClaimCore Referral Status
↓
Detailed Test Case
↓
Execution Result
↓
Defect, if applicable
Another example:
Plain Text
RC-REQ-087
Examiner Consistency
↓
RC-TS-040
Examiner Consistency
↓
Detailed Test Case
↓
Execution Result
22. Portfolio Disclaimer
ReferralConnect, IntakeConnect, ClaimCore, ReferralHub, integration services, incidents, claims, referrals, examiner information, statuses, messages, requests, responses, identifiers, fields, error conditions, test data, and expected results shown in this document are fictional or generalized.
The scenarios demonstrate API and integration testing, Single Sign-On testing, bidirectional synchronization, field mapping, negative testing, duplicate prevention, cross-application reconciliation, and end-to-end QA.
No confidential employer, client, vendor, claimant, production endpoint, credential, token, API key, certificate, or proprietary system information is included.
