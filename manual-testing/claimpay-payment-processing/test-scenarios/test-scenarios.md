ClaimPay Test Scenarios

Special Payment Processing Enhancement

Application: ClaimPay
Test Type: Manual Functional / Integration Testing
Environment: QA Environment
Project Type: Fictional Portfolio Project

1. Purpose
This document defines the high-level test scenarios for the ClaimPay Special Payment Processing Enhancement.

The scenarios are derived from the ClaimPay business requirements and are designed to validate:

Positive business flows
Negative scenarios
Business rules
Security and access
Data integrity
End-to-end workflows
Error handling
Regression behavior
Detailed test cases will be created from these scenarios.

2. Test Scenario Status
The following statuses will be used during test execution:

Not Executed
Pass
Fail
Blocked
Not Applicable
3. Configuration Scenarios
CP-TS-001
Requirement: CP-REQ-001

Verify Special Payment processing can be enabled through the global configuration.

Test Type: Positive / Configuration

CP-TS-002
Requirement: CP-REQ-002

Verify Special Payment processing can be enabled for an eligible business unit.

Test Type: Positive / Configuration

CP-TS-003
Requirement: CP-REQ-003

Verify Special Payment processing can be enabled for an eligible customer.

Test Type: Positive / Configuration

CP-TS-004
Requirements: CP-REQ-003, CP-REQ-004

Verify Special Payment processing does not occur for a customer when the feature is disabled at customer level.

Test Type: Negative / Configuration

CP-TS-005
Requirements: CP-REQ-005, CP-REQ-007

Verify a Configuration Administrator can modify and save Special Payment configuration.

Test Type: Positive / Security

CP-TS-006
Requirement: CP-REQ-006

Verify a Payment Specialist cannot change Special Payment configuration.

Test Type: Negative / Security

4. Special-Payment Identification
CP-TS-007
Requirement: CP-REQ-008

Verify a payment created with qualifying transaction code SP-110 is identified as a Special Payment.

Test Type: Positive

CP-TS-008
Requirement: CP-REQ-008

Verify each configured qualifying Special Payment transaction code triggers the Special Payment process.

Test Data:

SP-110
SP-220
SP-330
SP-440
SP-550
Test Type: Positive / Data-Driven

CP-TS-009
Requirement: CP-REQ-009

Verify nonqualifying transaction code ST-600 follows the Standard Payment process.

Test Type: Negative / Regression

CP-TS-010
Requirement: CP-REQ-011

Verify qualifying Special Payment logic applies to a standard claim.

Test Type: Positive

CP-TS-011
Requirement: CP-REQ-011

Verify qualifying Special Payment logic applies to a provisional claim.

Test Type: Positive

5. Funding-Account Assignment
CP-TS-012
Requirements: CP-REQ-012, CP-REQ-013

Verify the applicable funding account changes to the Corporate Funding Account after an eligible Special Payment is successfully saved.

Test Type: Positive / Integration

CP-TS-013
Requirement: CP-REQ-015

Verify a Standard Payment continues to use the applicable Customer Funding Account.

Test Type: Regression

CP-TS-014
Requirement: CP-REQ-016

Verify a qualifying transaction remains under Standard Payment funding rules when Special Payment processing is disabled.

Test Type: Negative / Configuration

CP-TS-015
Requirement: CP-REQ-017

Verify Payment History displays the funding account associated with the saved Special Payment.

Test Type: Positive / Data Validation

6. Offset Recovery Scenarios
CP-TS-016
Requirement: CP-REQ-018

Verify an Offset Recovery is automatically created after a qualifying Special Payment is successfully processed.

Test Type: Positive / Integration

CP-TS-017
Requirement: CP-REQ-019

Verify the correct recovery transaction code is created for each Special Payment code.

Expected Mapping:

SP-110 → RC-710
SP-220 → RC-720
SP-330 → RC-730
SP-440 → RC-730
SP-550 → RC-730
Test Type: Positive / Data-Driven

CP-TS-018
Requirement: CP-REQ-020

Verify the Offset Recovery is created as a separate transaction rather than as an additional allocation on the original payment.

Test Type: Positive

CP-TS-019
Requirements: CP-REQ-024, CP-REQ-025

Verify the Offset Recovery remains associated with the original Special Payment and applies the appropriate financial offset.

Test Type: Integration / Data Validation

CP-TS-020
Requirement: CP-REQ-026

Verify the appropriate transaction reference is retained between the Special Payment and the Offset Recovery.

Test Type: Data Validation

CP-TS-021
Requirement: CP-REQ-027

Verify an Offset Recovery is not automatically created for a Standard Payment.

Test Type: Negative / Regression

7. Payment Review and Hold Scenarios
CP-TS-022
Requirements: CP-REQ-028, CP-REQ-031

Verify a Special Payment requiring additional approval enters the appropriate review or hold workflow.

Test Type: Positive

CP-TS-023
Requirement: CP-REQ-029

Verify all required Special Payment information can be entered.

Test Type: Positive

CP-TS-024
Requirement: CP-REQ-030

Verify the system prevents Special Payment completion when mandatory information is missing.

Test Type: Negative

CP-TS-025
Requirement: CP-REQ-032

Verify an unauthorized user cannot release a restricted payment hold.

Test Type: Negative / Security

CP-TS-026
Requirement: CP-REQ-033

Verify an Authorized Reviewer can release an eligible payment hold.

Test Type: Positive / Security

8. Allocation Scenarios
CP-TS-027
Requirements: CP-REQ-034, CP-REQ-035

Verify an additional allocation cannot be added to an existing Special Payment.

Test Type: Negative

CP-TS-028
Requirement: CP-REQ-036

Verify a Special Payment allocation cannot be added to a payment that already contains another allocation.

Test Type: Negative

CP-TS-029
Requirement: CP-REQ-037

Verify an appropriate validation message is displayed when an additional allocation is prohibited.

Test Type: Negative / Validation

CP-TS-030
Requirement: CP-REQ-038

Verify multiple eligible Standard Payment allocations can still be created.

Test Type: Regression

9. Transaction-Code Change Scenarios
CP-TS-031
Requirement: CP-REQ-039

Verify a scheduled Special Payment cannot be directly changed to a standard transaction code.

Test Type: Negative

CP-TS-032
Requirement: CP-REQ-040

Verify a scheduled Standard Payment cannot be directly changed to a qualifying Special Payment code.

Test Type: Negative

CP-TS-033
Requirement: CP-REQ-041

Verify one scheduled Special Payment transaction code cannot be directly changed to another qualifying Special Payment code.

Test Type: Negative

CP-TS-034
Requirement: CP-REQ-042

Verify the transaction code of a system-created Offset Recovery cannot be directly changed.

Test Type: Negative

CP-TS-035
Requirement: CP-REQ-043

Verify an appropriate validation message is displayed for a prohibited transaction-code change.

Test Type: Negative / Validation

CP-TS-036
Requirements: CP-REQ-044, CP-REQ-045

Verify an eligible processed payment can be changed through the approved transaction-code change-request workflow.

Test Type: Positive / Workflow

CP-TS-037
Requirement: CP-REQ-046

Verify a warning is displayed when a transaction-code change could affect the Special Payment and Offset Recovery relationship.

Test Type: Validation

10. Void Scenarios
CP-TS-038
Requirement: CP-REQ-048

Verify an eligible processed Special Payment can be voided.

Test Type: Positive

CP-TS-039
Requirement: CP-REQ-049

Verify the related Offset Recovery is voided when the original Special Payment is voided.

Test Type: Positive / Integration

CP-TS-040
Requirements: CP-REQ-050, CP-REQ-051

Verify the appropriate claim activity is created when a Special Payment and related Offset Recovery are voided.

Validate that the activity contains the required transaction information.

Test Type: Positive / Data Validation

CP-TS-041
Requirement: CP-REQ-052

Verify payments created under Standard Payment processing continue to follow the appropriate standard void workflow.

Test Type: Regression

11. Refund Scenarios
CP-TS-042
Requirement: CP-REQ-054

Verify an authorized user can create an eligible refund using a qualifying Special Payment transaction code.

Test Type: Positive

CP-TS-043
Requirement: CP-REQ-055

Verify an eligible refund receives the Corporate Funding Account when Special Payment processing is enabled.

Test Type: Positive

CP-TS-044
Requirement: CP-REQ-056

Verify a refund follows standard funding rules when Special Payment processing is disabled.

Test Type: Negative / Configuration

CP-TS-045
Requirement: CP-REQ-057

Verify Payment History displays the appropriate funding information for the refund.

Test Type: Data Validation

12. Payment Upload Scenarios
CP-TS-046
Requirement: CP-REQ-059

Verify the Payment Upload process identifies an uploaded transaction containing a qualifying Special Payment code.

Test Type: Positive / Integration

CP-TS-047
Requirement: CP-REQ-060

Verify each uploaded qualifying Special Payment allocation is created as a separate payment when the source contains multiple allocations.

Test Type: Positive / Integration

CP-TS-048
Requirement: CP-REQ-061

Verify eligible nonqualifying uploaded allocations continue to follow Standard Payment rules.

Test Type: Regression

CP-TS-049
Requirement: CP-REQ-062

Verify an uploaded qualifying Special Payment receives the Corporate Funding Account when the feature is enabled.

Test Type: Positive

CP-TS-050
Requirement: CP-REQ-063

Verify the upload process identifies whether uploaded payment records were processed successfully or failed validation.

Test Type: Positive / Negative

13. Provisional Claim Scenarios
CP-TS-051
Requirement: CP-REQ-064

Verify an eligible Special Payment can be created on a provisional claim.

Test Type: Positive

CP-TS-052
Requirement: CP-REQ-065

Verify an Offset Recovery is created for an eligible Special Payment on a provisional claim.

Test Type: Positive / Integration

CP-TS-053
Requirement: CP-REQ-066

Verify a user cannot directly alter a system-created Offset Recovery on a provisional claim.

Test Type: Negative

CP-TS-054
Requirements: CP-REQ-067, CP-REQ-068

Verify the Special Payment and Offset Recovery transfer correctly when a provisional claim is converted to a standard claim.

Verify the recovery retains its applicable status.

Test Type: Integration

CP-TS-055
Requirement: CP-REQ-069

Verify the relationship between the Special Payment and Offset Recovery is maintained after claim conversion.

Test Type: Integration / Data Validation

14. Payment-Move Scenarios
CP-TS-056
Requirement: CP-REQ-070

Verify an authorized user can submit a payment-move request for an eligible Special Payment.

Test Type: Positive

CP-TS-057
Requirement: CP-REQ-071

Verify a Payment Operations Analyst can review and complete an eligible payment-move request.

Test Type: Positive / Security

CP-TS-058
Requirement: CP-REQ-072

Verify the related Offset Recovery moves with the original Special Payment.

Test Type: Integration

CP-TS-059
Requirements: CP-REQ-073, CP-REQ-074

Verify Special Payment and recovery funding assignments remain unchanged after the payment move.

Test Type: Integration / Data Validation

CP-TS-060
Requirement: CP-REQ-075

Verify applicable transaction references are preserved after a payment move.

Test Type: Data Validation

CP-TS-061
Requirement: CP-REQ-076

Verify restricted Special Payments cannot bypass the approved payment-move workflow.

Test Type: Negative / Security

15. Exception-Handling Scenarios
CP-TS-062
Requirement: CP-REQ-077

Verify Special Payment processing does not partially complete when required configuration is unavailable.

Test Type: Negative / Exception Handling

CP-TS-063
Requirements: CP-REQ-078, CP-REQ-079

Verify a failed Special Payment remains in the appropriate unprocessed status and an appropriate processing result is provided.

Test Type: Negative / Exception Handling

CP-TS-064
Requirement: CP-REQ-080

Verify an Offset Recovery is not created without a successfully processed related Special Payment.

Test Type: Negative / Data Integrity

CP-TS-065
Requirement: CP-REQ-081

Verify a processing failure does not leave incomplete or unintended financial data on the claim.

Test Type: Negative / Data Integrity

CP-TS-066
Requirement: CP-REQ-082

Verify an eligible failed payment can be reprocessed after the cause of the failure is corrected.

Test Type: Positive / Recovery Testing

16. Audit and Activity-History Scenarios
CP-TS-067
Requirement: CP-REQ-083

Verify relevant Special Payment activity is recorded in claim history.

Test Type: Positive / Audit

CP-TS-068
Requirement: CP-REQ-084

Verify the Offset Recovery appears in the appropriate transaction history.

Test Type: Positive / Audit

CP-TS-069
Requirement: CP-REQ-085

Verify configuration changes retain sufficient audit information.

Test Type: Security / Audit

CP-TS-070
Requirement: CP-REQ-086

Verify transaction-code change requests retain appropriate request and review information.

Test Type: Audit

CP-TS-071
Requirement: CP-REQ-087

Verify payment-move history retains appropriate source, destination, status, and review information.

Test Type: Audit

17. Security Scenarios
CP-TS-072
Requirement: CP-REQ-088

Verify a Payment Specialist can perform permitted payment functions but cannot modify configuration.

Test Type: Positive / Negative / Security

CP-TS-073
Requirement: CP-REQ-089

Verify a Configuration Administrator can maintain authorized Special Payment settings.

Test Type: Security

CP-TS-074
Requirement: CP-REQ-090

Verify a Payment Operations Analyst can process authorized payment-move and transaction-code change requests.

Test Type: Security

CP-TS-075
Requirement: CP-REQ-091

Verify an Authorized Reviewer can perform the applicable payment-hold review and release process.

Test Type: Security

CP-TS-076
Requirement: CP-REQ-092

Verify unauthorized users cannot perform restricted Special Payment functions.

Test Type: Negative / Security

18. Regression Scenarios
CP-TS-077
Requirement: CP-REQ-093

Verify Standard Payment creation continues to operate with nonqualifying transaction codes.

Test Type: Regression

CP-TS-078
Requirement: CP-REQ-094

Verify a Standard Payment continues to use the appropriate Customer Funding Account.

Test Type: Regression

CP-TS-079
Requirement: CP-REQ-095

Verify Standard Payments do not automatically generate Offset Recoveries.

Test Type: Regression

CP-TS-080
Requirement: CP-REQ-096

Verify eligible Standard Payment allocations can continue to be edited.

Test Type: Regression

CP-TS-081
Requirement: CP-REQ-097

Verify multiple eligible Standard Payment allocations can continue to be created.

Test Type: Regression

CP-TS-082
Requirement: CP-REQ-098

Verify eligible Standard Payments continue to support standard payment-move processing.

Test Type: Regression

CP-TS-083
Requirement: CP-REQ-099

Verify existing payment authority, hold, review, and release behavior continues to operate.

Test Type: Regression

CP-TS-084
Requirement: CP-REQ-100

Verify existing claim activity/history functionality continues to operate.

Test Type: Regression

19. End-to-End Scenarios
CP-TS-E2E-001
Requirement: CP-REQ-101

Validate complete Special Payment processing
Configuration Enabled ↓ Open Eligible Claim ↓ Create Special Payment ↓ Select Qualifying Transaction Code ↓ Enter Required Information ↓ Save Payment ↓ Corporate Funding Account Assigned ↓ Review / Release Payment if Required ↓ Process Payment ↓ Offset Recovery Created ↓ Validate Payment History ↓ Validate Recovery ↓ Validate Claim Activity

Expected Outcome:

The complete Special Payment workflow completes successfully, and the related payment, recovery, funding, transaction, and activity information remains consistent.

CP-TS-E2E-002
Requirement: CP-REQ-102

Validate Special Payment void workflow
Processed Special Payment ↓ Related Offset Recovery Exists ↓ Void Original Special Payment ↓ Original Payment Voided ↓ Offset Recovery Voided ↓ Claim Activity Created ↓ Validate Transaction History

Expected Outcome:

The original Special Payment and its related Offset Recovery are handled consistently through the complete void process.

CP-TS-E2E-003
Requirement: CP-REQ-103

Validate Special Payment move workflow
Processed Special Payment ↓ Related Offset Recovery ↓ Submit Payment-Move Request ↓ Authorized Review ↓ Move Original Payment ↓ Move Related Recovery ↓ Validate Destination Claim ↓ Validate Funding ↓ Validate References ↓ Validate Audit History

Expected Outcome:

The Special Payment and associated Offset Recovery move through the approved workflow while retaining required relationships and transaction information.

CP-TS-E2E-004
Validate Provisional Claim conversion workflow
Provisional Claim ↓ Create Special Payment ↓ Process Payment ↓ Offset Recovery Created ↓ Convert to Standard Claim ↓ Validate Payment Transfer ↓ Validate Recovery Transfer ↓ Validate Recovery Status ↓ Validate Payment/Recovery Relationship

Test Type: End-to-End / Integration

CP-TS-E2E-005
Validate configuration-off workflow
Disable Special Payment Processing ↓ Open Eligible Claim ↓ Create Payment Using Qualifying Code ↓ Save Payment ↓ Validate Standard Funding Behavior ↓ Validate Special Recovery Is Not Created ↓ Validate Transaction History

Test Type: End-to-End / Negative / Regression

20. Scenario Coverage Summary
The test scenarios provide coverage for:

Configuration
User security
Special Payment identification
Standard Payment differentiation
Funding-account assignment
Offset Recovery creation
Payment/recovery relationships
Payment authority and holds
Allocation restrictions
Transaction-code restrictions
Void processing
Refund processing
Payment Upload
Provisional Claim processing
Claim conversion
Payment moves
Exception handling
Data integrity
Audit history
Regression
End-to-end business workflows
21. Traceability Approach
Detailed test cases will be linked back to these scenarios.

Example:

CP-REQ-018 ↓ CP-TS-016 ↓ CP-TC-016-01 CP-TC-016-02 CP-TC-016-03 ↓ Execution Result ↓ Defect, if applicable

One test scenario may produce multiple detailed test cases when different test data, user roles, conditions, or negative paths must be validated.

22. Portfolio Disclaimer
ClaimPay is a fictional application created solely for portfolio and learning purposes.

All application names, transaction codes, account names, claims, users, roles, financial information, configurations, business rules, workflows, and test data presented in this document are fictional or generalized.

No confidential employer, client, claimant, banking, production, or proprietary information is included.
