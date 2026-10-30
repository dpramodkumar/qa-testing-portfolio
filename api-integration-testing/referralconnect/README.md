ReferralConnect – Bidirectional API & SSO Integration Testing
📌 Project Overview
ReferralConnect is a fictional multi-system integration project created to demonstrate API, Single Sign-On (SSO), functional, integration, and end-to-end software testing.

The project represents an insurance claims workflow involving three fictional applications:

IntakeConnect – Incident intake application
ClaimCore – Claims management application
ReferralHub – External referral management application
The applications exchange incident, claim, referral, status, and examiner information through system integrations.

🎯 Project Objective
The objective of this project is to demonstrate validation of bidirectional data flows across multiple integrated applications.

Testing focuses on:

Incident integration
Existing referral retrieval
Intake-to-claims referral mapping
Single Sign-On
Referral creation
API request and response validation
Vendor-to-claims synchronization
Claims-to-vendor synchronization
Examiner synchronization
Referral-status synchronization
UI and API data reconciliation
Duplicate prevention
Error handling
End-to-end referral lifecycle testing
🏗️ High-Level Integration Flow
Flow 1 – Existing Referral During Incident Intake
An incident is created in IntakeConnect.

IntakeConnect retrieves applicable existing referral information from ReferralHub.

IntakeConnect
      ↓
Create Incident
      ↓
ReferralHub
      ↓
Retrieve Existing Referral
      ↓
IntakeConnect
      ↓
Incident + Referral Data
      ↓
ClaimCore
      ↓
Convert Incident to Claim
      ↓
Referral Available on Claim

Testing validates that referral information retrieved during intake reaches the correct claim after incident-to-claim conversion.
Flow 2 – Single Sign-On
An authorized ClaimCore user can access ReferralHub through Single Sign-On.
Plain Text
ClaimCore
↓
SSO Launch
↓
Authentication / Authorization
↓
ReferralHub
↓
Correct Claim / Referral Context
Testing validates authorized access and the applicable claim/referral context.
Flow 3 – New Referral from ReferralHub to ClaimCore
A referral created in ReferralHub is sent to ClaimCore through the integration layer.
Plain Text
ReferralHub
↓
Create Referral
↓
Referral Integration
↓
ClaimCore
↓
Existing Claim Updated
↓
Referral Displayed in ClaimCore UI
Testing verifies:
Correct claim association
Referral information
Integration processing
Field mapping
Existing-record behavior
ClaimCore UI display
Flow 4 – Claim Examiner Synchronization
After a referral exists, the claim examiner can be changed in ClaimCore.
The updated examiner information is synchronized to ReferralHub.
Plain Text
ClaimCore
↓
Claim Examiner Changed
↓
Outbound Integration
↓
ReferralHub
↓
Existing Referral Updated
↓
New Examiner Displayed
Testing verifies that the existing referral is updated without creating an unintended duplicate referral.
Flow 5 – Referral Status Synchronization
When referral status changes in ReferralHub, the updated status is sent to ClaimCore.
Plain Text
ReferralHub
↓
Referral Status Changed
↓
Status Integration
↓
ClaimCore
↓
Existing Referral Updated
↓
Updated Status Displayed in ClaimCore UI
Testing verifies that:
Plain Text
ReferralHub Status
=
Integration Data
=
ClaimCore Data
=
ClaimCore UI
🔄 Bidirectional Integration
ReferralConnect demonstrates integrations operating in both directions.
Plain Text
ClaimCore
↕ ↕
Examiner Referral
Update Status
↕ ↕
ReferralHub
ReferralHub → ClaimCore
Examples:
New referral
Referral-status update
ClaimCore → ReferralHub
Example:
Claim examiner update
ReferralHub → IntakeConnect → ClaimCore
Example:
Existing referral retrieved during incident intake
🧪 Testing Areas
API / Integration Testing
Request validation
Response validation
Required-field validation
Field mapping
Positive API scenarios
Negative API scenarios
Error responses
Existing-record updates
Duplicate prevention
Cross-system data consistency
SSO Testing
Authorized SSO access
Unauthorized access
User context
Claim context
Referral context
Session behavior
Error handling
Functional Testing
Incident creation
Referral retrieval
Incident-to-claim conversion
Referral display
Examiner change
Referral-status change
UI / Integration Reconciliation
Testing compares information across integration points.
Example:
Plain Text
ReferralHub
↓
Integration
↓
ClaimCore
↓
ClaimCore UI
The applicable values should remain consistent across the complete flow.
End-to-End Testing
The complete referral lifecycle includes:
Plain Text
Incident Creation
↓
Existing Referral Retrieval
↓
Incident Integration
↓
Claim Creation
↓
Referral Available on Claim
↓
SSO to ReferralHub
↓
Referral Processing
↓
Referral Update to ClaimCore
↓
Examiner Update to ReferralHub
↓
Referral Status Update to ClaimCore
🔍 Key QA Validation Patterns
API-to-UI Validation
Plain Text
Referral Data
↓
Integration Message
↓
ClaimCore
↓
ClaimCore UI
Expected:
The UI accurately represents the referral information received through the integration.
Claims-to-Vendor Validation
Plain Text
ClaimCore Examiner
↓
Examiner Update Integration
↓
ReferralHub Referral
Expected:
The existing ReferralHub referral reflects the new ClaimCore examiner.
Vendor-to-Claims Validation
Plain Text
ReferralHub Status
↓
Status Integration
↓
ClaimCore Referral
↓
ClaimCore UI
Expected:
The referral status displayed in ClaimCore reflects the latest applicable ReferralHub status.
🛠️ Testing Skills Demonstrated
This project demonstrates:
API Testing
Integration Testing
Request / Response Validation
Data Mapping Validation
Single Sign-On Testing
Bidirectional Integration Testing
Cross-Application Validation
UI / API Reconciliation
Positive Testing
Negative Testing
Error Handling
Regression Testing
End-to-End Testing
Requirements Traceability
Defect Validation
📂 Planned Portfolio Artifacts
The ReferralConnect project will include:
Integration Architecture
Sanitized Business Requirements
API Integration Test Plan
API Test Scenarios
Request / Response Validation
SSO Test Cases
Detailed Integration Test Cases
End-to-End Referral Lifecycle
Test Execution Results
Sample Defects
Requirements Traceability Matrix
🔒 Portfolio Disclaimer
ReferralConnect, IntakeConnect, ClaimCore, ReferralHub, and all integrations shown in this project are fictional or generalized for portfolio demonstration.
All application names, users, claims, incidents, referrals, identifiers, statuses, fields, payloads, endpoints, authentication details, business rules, test data, expected results, and workflows used in this repository are fictional or generalized.
No confidential employer, client, vendor, claimant, production, credential, token, API key, endpoint, or proprietary system information is included.

