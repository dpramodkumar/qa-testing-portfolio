ClaimPay Detailed Test Cases
Special Payment Processing Enhancement
Application: ClaimPay
Testing Type: Manual Functional / Integration Testing
Environment: QA Environment
Project Type: Fictional Portfolio Project

1. Purpose
This document contains a curated set of detailed test cases for the ClaimPay Special Payment Processing Enhancement.

The test cases demonstrate:

Positive testing
Negative testing
Configuration testing
Security testing
Business-rule validation
Integration testing
Data validation
Exception handling
End-to-end testing
Regression testing
All test data used in these test cases is fictional.

2. Common Test Data
Claims
CLM-10001 - Standard eligible claim
CLM-10002 - Standard eligible claim
CLM-10003 - Standard payment claim
TMP-20001 - Provisional claim
Users
QA-CONFIG-01 - Configuration Administrator
QA-PAY-01 - Payment Specialist
QA-OPS-01 - Payment Operations Analyst
QA-REVIEW-01 - Authorized Reviewer
Special Payment Codes
SP-110
SP-220
SP-330
SP-440
SP-550
Standard Payment Code
ST-600
Recovery Codes
RC-710
RC-720
RC-730
3. Configuration Test Cases
CP-TC-001 - Enable Special Payment Processing
Requirement: CP-REQ-001
Scenario: CP-TS-001
Test Type: Positive / Configuration

Preconditions
User has Configuration Administrator access.
Special Payment Processing is currently disabled.
Steps
Log in as Configuration Administrator.
Navigate to Special Payment configuration.
Locate the Special Payment Processing setting.
Enable the setting.
Save the configuration.
Exit the configuration area.
Reopen the configuration.
Expected Result
Configuration is saved successfully.
Special Payment Processing is displayed as enabled after reopening the configuration.
CP-TC-002 - Unauthorized Configuration Change
Requirement: CP-REQ-006
Scenario: CP-TS-006
Test Type: Negative / Security

Preconditions
Special Payment configuration exists.
User has Payment Specialist access only.
Steps
Log in as Payment Specialist.
Attempt to access Special Payment configuration.
Attempt to modify the configuration if the screen is accessible.
Expected Result
The Payment Specialist cannot modify the Special Payment configuration.
Unauthorized configuration changes are not saved.
4. Special Payment Identification
CP-TC-003 - Create Qualifying Special Payment
Requirements: CP-REQ-008, CP-REQ-013
Scenarios: CP-TS-007, CP-TS-012
Test Type: Positive / Functional

Preconditions
Special Payment Processing is enabled.
CLM-10001 is eligible for payment processing.
User has Payment Specialist access.
Test Data
Claim: CLM-10001
Transaction Code: SP-110
Amount: $100.00
Steps
Log in as Payment Specialist.
Search for CLM-10001.
Open the claim.
Navigate to payment entry.
Create a new payment.
Enter the required payment information.
Select transaction code SP-110.
Enter payment amount of $100.00.
Enter all required Special Payment information.
Save the payment.
Expected Result
Payment is successfully saved.
Payment is identified as a Special Payment.
Corporate Funding Account is assigned to the payment.
Saved payment information is displayed correctly.
CP-TC-004 - Validate All Qualifying Transaction Codes
Requirement: CP-REQ-008
Scenario: CP-TS-008
Test Type: Positive / Data-Driven

Test Data
Execute the test using:

SP-110
SP-220
SP-330
SP-440
SP-550
Steps
For each transaction code:

Open an eligible claim.
Create a new payment.
Select the qualifying transaction code.
Enter required information.
Save the payment.
Review the saved payment.
Expected Result
For each configured transaction code:

Payment is identified as a Special Payment.
Applicable Special Payment processing rules are applied.
CP-TC-005 - Nonqualifying Payment
Requirements: CP-REQ-009, CP-REQ-015
Scenario: CP-TS-009
Test Type: Negative / Regression

Test Data
Claim: CLM-10003
Transaction Code: ST-600
Amount: $100.00
Steps
Open CLM-10003.
Create a new payment.
Select ST-600.
Enter required information.
Save the payment.
Review funding information.
Expected Result
Payment follows Standard Payment processing.
Customer Funding Account remains applicable.
Special Payment processing is not applied.
5. Funding Account Validation
CP-TC-006 - Corporate Funding Account Assignment
Requirements: CP-REQ-013, CP-REQ-017
Scenarios: CP-TS-012, CP-TS-015
Test Type: Positive / Data Validation

Preconditions
Special Payment Processing is enabled.
CLM-10001 is eligible.
Steps
Create a payment using SP-220.
Complete required information.
Save the payment.
Open Payment History.
Review the funding information.
Expected Result
Saved Special Payment uses the Corporate Funding Account.
Payment History displays the correct funding assignment.
CP-TC-007 - Feature Disabled
Requirements: CP-REQ-004, CP-REQ-016
Scenario: CP-TS-014
Test Type: Negative / Configuration

Preconditions
Special Payment Processing is disabled for the test customer.
Steps
Open an eligible claim.
Create a payment.
Select SP-110.
Enter required information.
Save the payment.
Review funding information.
Expected Result
Special funding behavior is not applied.
Payment follows the applicable Standard Payment funding rules.
6. Offset Recovery Testing
CP-TC-008 - Create Offset Recovery
Requirements: CP-REQ-018, CP-REQ-020
Scenarios: CP-TS-016, CP-TS-018
Test Type: Positive / Integration

Preconditions
Special Payment Processing is enabled.
Eligible Special Payment exists.
Steps
Create a qualifying Special Payment.
Complete required review actions.
Process the payment successfully.
Open transaction history.
Locate the processed Special Payment.
Review related transactions.
Expected Result
Original Special Payment is processed successfully.
Separate Offset Recovery is created.
Recovery is not created as an additional allocation on the original payment.
CP-TC-009 - Validate Recovery Code Mapping
Requirement: CP-REQ-019
Scenario: CP-TS-017
Test Type: Positive / Data Validation

Test Data
SP-110 → RC-710
SP-220 → RC-720
SP-330 → RC-730
SP-440 → RC-730
SP-550 → RC-730
Steps
For each Special Payment code:

Create and process the Special Payment.
Locate the generated Offset Recovery.
Review the recovery transaction code.
Expected Result
The corresponding configured Offset Recovery code is created.
CP-TC-010 - Validate Payment and Recovery Relationship
Requirements: CP-REQ-024, CP-REQ-025, CP-REQ-026
Scenarios: CP-TS-019, CP-TS-020
Test Type: Integration / Data Validation

Steps
Process an eligible Special Payment.
Locate the generated Offset Recovery.
Compare original and recovery transaction information.
Review payment and recovery references.
Review the applicable financial values.
Expected Result
Special Payment and Offset Recovery remain related.
Applicable transaction references are retained.
Recovery applies the expected financial offset.
CP-TC-011 - No Recovery for Standard Payment
Requirement: CP-REQ-027
Scenario: CP-TS-021
Test Type: Negative / Regression

Steps
Create a payment using ST-600.
Process the payment successfully.
Review transaction history.
Expected Result
Standard Payment is processed successfully.
No Special Payment Offset Recovery is automatically created.
7. Required Information and Hold Testing
CP-TC-012 - Required Special Payment Information Missing
Requirement: CP-REQ-030
Scenario: CP-TS-024
Test Type: Negative / Validation

Steps
Open an eligible claim.
Create a Special Payment.
Select qualifying transaction code SP-110.
Leave required Special Payment information blank.
Attempt to complete the payment.
Expected Result
Payment cannot be completed.
Appropriate validation information is displayed.
Incomplete Special Payment is not processed.
CP-TC-013 - Unauthorized Hold Release
Requirement: CP-REQ-032
Scenario: CP-TS-025
Test Type: Negative / Security

Preconditions
Special Payment is on a restricted hold.
Payment Specialist does not have required release authority.
Steps
Log in as Payment Specialist.
Open the held payment.
Attempt to release the hold.
Expected Result
User cannot release the restricted hold.
Payment remains in the appropriate hold status.
CP-TC-014 - Authorized Hold Release
Requirement: CP-REQ-033
Scenario: CP-TS-026
Test Type: Positive / Security

Preconditions
Eligible payment is on hold.
Authorized Reviewer has appropriate access.
Steps
Log in as Authorized Reviewer.
Locate the held payment.
Review the payment.
Release the hold.
Expected Result
Hold is successfully released.
Payment can continue through the applicable workflow.
8. Allocation Testing
CP-TC-015 - Add Allocation to Existing Special Payment
Requirements: CP-REQ-034, CP-REQ-035, CP-REQ-037
Scenarios: CP-TS-027, CP-TS-029
Test Type: Negative / Business Rule

Steps
Create a payment containing SP-110.
Attempt to add another allocation.
Attempt to save the payment.
Expected Result
Additional allocation is prevented.
Appropriate validation information is displayed.
Special Payment remains a single-allocation payment.
CP-TC-016 - Add Special Allocation to Existing Standard Payment
Requirement: CP-REQ-036
Scenario: CP-TS-028
Test Type: Negative

Steps
Create a payment containing ST-600.
Add another allocation.
Select SP-110 for the new allocation.
Attempt to save.
Expected Result
Special Payment allocation is not added to the existing multi-allocation payment.
The invalid combination is prevented.
CP-TC-017 - Standard Multiple Allocations
Requirement: CP-REQ-038
Scenario: CP-TS-030
Test Type: Regression

Steps
Create a Standard Payment.
Add multiple eligible Standard Payment allocations.
Enter required information.
Save the payment.
Expected Result
Eligible nonqualifying allocations are successfully saved.
Special Payment restrictions do not affect valid Standard Payments.
9. Transaction-Code Controls
CP-TC-018 - Restricted Special-to-Standard Change
Requirements: CP-REQ-039, CP-REQ-043
Scenarios: CP-TS-031, CP-TS-035
Test Type: Negative / Business Rule

Preconditions
Scheduled Special Payment exists.
Steps
Open the scheduled Special Payment.
Attempt to change SP-110 to ST-600.
Attempt to save the change.
Expected Result
Direct transaction-code change is prevented.
Appropriate validation information is displayed.
Original transaction code remains unchanged.
CP-TC-019 - Processed Payment Change Request
Requirements: CP-REQ-044, CP-REQ-045
Scenario: CP-TS-036
Test Type: Positive / Workflow

Preconditions
Eligible processed payment exists.
Appropriate users are available.
Steps
Submit an eligible transaction-code change request.
Verify the request is created.
Log in as Payment Operations Analyst.
Locate the request.
Review the requested change.
Complete the eligible request.
Review the resulting transaction information.
Expected Result
Change request follows the approved workflow.
Authorized Payment Operations Analyst can complete the eligible request.
Resulting transaction information reflects the approved action.
10. Void Testing
CP-TC-020 - Void Special Payment and Offset Recovery
Requirements: CP-REQ-048, CP-REQ-049
Scenarios: CP-TS-038, CP-TS-039
Test Type: Positive / Integration

Preconditions
Processed Special Payment exists.
Related Offset Recovery exists.
Steps
Open the processed Special Payment.
Initiate the void process.
Complete required void information.
Submit the void.
Review original payment status.
Review related Offset Recovery status.
Review transaction history.
Expected Result
Original Special Payment is voided.
Related Offset Recovery is also voided.
Transaction history reflects the updated statuses.
CP-TC-021 - Validate Void Activity
Requirements: CP-REQ-050, CP-REQ-051
Scenario: CP-TS-040
Test Type: Positive / Audit

Preconditions
CP-TC-020 completed successfully.
Steps
Open claim activity history.
Locate activity related to the payment void.
Review the recorded information.
Expected Result
Activity includes applicable:

Original transaction date
Payee/recipient information
Original payment amount
Void date
Confirmation of related Offset Recovery handling
11. Provisional Claim / Conversion Testing
CP-TC-022 - Provisional Claim Conversion
Requirements: CP-REQ-064, CP-REQ-065, CP-REQ-067, CP-REQ-068, CP-REQ-069

Scenarios: CP-TS-051, CP-TS-052, CP-TS-054, CP-TS-055

Test Type: Integration / End-to-End

Preconditions
TMP-20001 is a provisional claim.
Special Payment Processing is enabled.
Steps
Open TMP-20001.
Create a qualifying Special Payment.
Process the payment.
Verify Offset Recovery is created.
Record transaction references and statuses.
Convert the provisional claim to a standard claim.
Open the converted claim.
Review the Special Payment.
Review the Offset Recovery.
Compare transaction references and statuses.
Expected Result
Special Payment transfers to the converted claim.
Offset Recovery transfers to the converted claim.
Applicable recovery status is maintained.
Payment/recovery relationship is preserved.
Applicable references remain intact.
12. Payment Move Testing
CP-TC-023 - Move Special Payment and Related Recovery
Requirements: CP-REQ-070, CP-REQ-071, CP-REQ-072, CP-REQ-073, CP-REQ-074, CP-REQ-075

Scenarios: CP-TS-056, CP-TS-057, CP-TS-058, CP-TS-059, CP-TS-060

Test Type: Integration / End-to-End

Preconditions
Processed Special Payment exists on CLM-10001.
Related Offset Recovery exists.
CLM-10002 is eligible to receive the payment.
Steps
Submit a payment-move request from CLM-10001 to CLM-10002.
Verify the request is created.
Log in as Payment Operations Analyst.
Review the move request.
Complete the eligible move.
Open CLM-10001.
Review original transaction history.
Open CLM-10002.
Locate the moved Special Payment.
Locate the associated Offset Recovery.
Verify funding assignments.
Verify transaction references.
Expected Result
Special Payment moves to CLM-10002.
Related Offset Recovery moves with the payment.
Corporate Funding Account remains unchanged.
Recovery funding assignment remains unchanged.
Applicable transaction references are preserved.
Payment/recovery relationship remains intact.
13. Exception Handling
CP-TC-024 - Required Configuration Missing
Requirements: CP-REQ-077, CP-REQ-078, CP-REQ-079, CP-REQ-080, CP-REQ-081

Scenarios: CP-TS-062, CP-TS-063, CP-TS-064, CP-TS-065

Test Type: Negative / Exception Handling

Preconditions
Test configuration required for Special Payment processing is intentionally unavailable or invalid.
Steps
Open an eligible claim.
Create a qualifying Special Payment.
Enter valid payment information.
Attempt to process the payment.
Review payment status.
Review processing result.
Review transaction history.
Check for Offset Recovery.
Review claim financial information.
Expected Result
Special Payment does not partially complete.
Appropriate processing result is provided.
Payment remains in the appropriate unprocessed status.
Orphan Offset Recovery is not created.
Incomplete or unintended financial information is not saved.
14. Regression Testing
CP-TC-025 - Standard Payment Regression
Requirements: CP-REQ-093, CP-REQ-094, CP-REQ-095, CP-REQ-096, CP-REQ-097

Scenarios: CP-TS-077, CP-TS-078, CP-TS-079, CP-TS-080, CP-TS-081

Test Type: Regression

Preconditions
CLM-10003 is available.
ST-600 is an eligible Standard Payment transaction code.
Steps
Open CLM-10003.
Create a Standard Payment using ST-600.
Add another eligible Standard Payment allocation.
Save the payment.
Edit an eligible Standard Payment allocation.
Process the payment.
Review funding information.
Review transaction history.
Check for Special Payment Offset Recovery.
Expected Result
Standard Payment functionality continues to operate.
Multiple eligible Standard Payment allocations are permitted.
Eligible Standard Payment allocations can be edited.
Appropriate Customer Funding Account is used.
No Special Payment Offset Recovery is automatically created.

15. Test Execution Template
Each executed test case will capture:

Test Case ID:
Execution ID:
Execution Date:
Tester: Portfolio QA Tester
Environment: QA Environment
Actual Result:
Status: Pass / Fail / Blocked / Not Executed
Defect ID: If applicable
Comments:

16. Traceability Example
CP-REQ-018 ↓ CP-TS-016 ↓ CP-TC-008 ↓ CP-ER-008 ↓ CP-DEF-001, if applicable

17. Portfolio Disclaimer
ClaimPay is a fictional application created solely for portfolio and learning purposes.

The detailed test cases demonstrate manual QA techniques including positive, negative, configuration, security, integration, end-to-end, exception, data-validation, and regression testing.

All application names, transaction codes, account names, claims, users, roles, financial values, configurations, business rules, workflows, and test data are fictional or generalized.

No confidential employer, client, claimant, banking, production, or proprietary information is included.

