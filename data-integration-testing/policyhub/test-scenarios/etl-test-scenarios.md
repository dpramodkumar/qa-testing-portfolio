PolicyHub ETL Test Scenarios
Phase 1 – Policy Data Integration
Application: PolicyHub
ETL Tool: Informatica PowerCenter
Database: Oracle Database
Database Validation Tool: Toad for Oracle
Environment: QA Environment
Testing Type: ETL / Database / Data Integration Testing

1. Purpose
This document defines the Phase 1 ETL test scenarios for the fictional PolicyHub project.

The objective is to validate policy data from the incoming source file through Informatica PowerCenter and Oracle processing into the final PolicyHub Repository.

The primary ETL flow is:

Policy Feed ↓ Secure File Gateway ↓ Informatica PowerCenter ↓ Oracle Landing Layer ↓ Transformation ↓ Staging Layer ↓ Validation ↓ Integration Layer ↓ Insert / Update Processing ↓ PolicyHub Repository

Testing includes positive, negative, reconciliation, transformation, database, error-handling, and regression scenarios.

2. Source File Validation Scenarios
PH-TS-001 – Valid Policy File Receipt
Requirements: PH-REQ-001, PH-REQ-006
Test Type: Positive / File Integration

Verify a valid policy file delivered through the approved file-delivery process is available for Informatica processing.

Expected Result
File is received successfully.
File is available for ETL processing.
File is not altered unexpectedly during delivery.
PH-TS-002 – Validate Policy File Format
Requirement: PH-REQ-002
Test Type: Positive / File Validation

Verify the policy file matches the expected file format and structural requirements.

Validate
File type
Delimiter
Column count
Record structure
Required columns
Expected encoding, when applicable
Expected Result
The valid file satisfies the configured structural requirements.

PH-TS-003 – Invalid File Structure
Requirement: PH-REQ-002
Test Type: Negative / File Validation

Verify a policy file with an invalid structure does not progress as a valid policy feed.

Examples:

Missing column
Extra unexpected column
Invalid delimiter
Malformed record
Expected Result
The invalid input is identified and handled according to the ETL error-processing rules.

PH-TS-004 – Validate Filename
Requirement: PH-REQ-003
Test Type: Positive / Negative

Verify the incoming filename follows the expected naming convention.

Also verify that an invalid filename is appropriately handled.

PH-TS-005 – Empty Policy File
Requirement: PH-REQ-004
Test Type: Negative / Boundary

Verify processing behavior when the input file contains no policy data records.

Expected Result
Empty file is identified.
No unintended policy records are created.
Appropriate processing status or information is available.
PH-TS-006 – Duplicate File
Requirement: PH-REQ-005
Test Type: Negative / Data Integrity

Process the same fictional policy file more than once.

Expected Result
Duplicate processing does not create unintended duplicate policy data in the target repository.

3. Informatica Workflow Scenarios
PH-TS-007 – Successful Workflow Execution
Requirements: PH-REQ-007, PH-REQ-012
Test Type: Positive / Informatica

Verify Informatica PowerCenter successfully executes the configured policy-load workflow for a valid source file.

Expected Result
Workflow starts successfully.
Required sessions complete successfully.
Workflow finishes successfully.
Processing status is available for review.
PH-TS-008 – Source Records Read by Informatica
Requirements: PH-REQ-008, PH-REQ-010
Test Type: ETL / Reconciliation

Verify Informatica reads all expected source policy records.

Validation
Compare:

Source File Record Count

to:

Informatica Source Records Read

Expected Result
Counts match unless a documented processing rule explains the difference.

PH-TS-009 – Landing Data Load
Requirements: PH-REQ-009, PH-REQ-011
Test Type: ETL / Database

Verify source policy records are loaded into the appropriate Oracle landing layer.

Expected Result
Expected policy records exist in landing.
Expected record count is present.
Key source values remain accurate.
PH-TS-010 – Informatica Workflow Failure
Requirements: PH-REQ-013, PH-REQ-014
Test Type: Negative / Error Handling

Verify behavior when the Informatica workflow cannot complete.

Expected Result
Workflow indicates failure.
Useful failure information is available.
Incomplete records are not exposed as successfully processed policies.
Downstream data integrity is maintained.
4. Landing-Layer Validation Scenarios
PH-TS-011 – Source-to-Landing Record Count
Requirements: PH-REQ-010, PH-REQ-011
Test Type: Reconciliation

Compare:

Source File Record Count
           =
Oracle Landing Record Count

Expected Result
Counts reconcile according to documented processing rules.
PH-TS-012 – Source-to-Landing Field Validation
Requirement: PH-REQ-009
 Test Type: Database / Data Validation
Validate representative source fields against corresponding landing values.
Example fictional values:
Plain Text
Policy Number: PH-100245
Effective Date: 2026-01-01
Expiration Date: 2026-12-31
Expected Result
Landing data accurately reflects the incoming source record.
5. Transformation Scenarios
PH-TS-013 – Policy Identifier Transformation
Requirement: PH-REQ-016
 Test Type: Transformation
Verify the policy identifier is preserved or derived according to the applicable transformation rule.
PH-TS-014 – Policy Date Transformation
Requirement: PH-REQ-017
 Test Type: Transformation / Data Type
Verify source policy dates are converted into the required target date format without changing the actual business date.
PH-TS-015 – Address Transformation
Requirement: PH-REQ-018
 Test Type: Transformation
Verify source address information is transformed into the applicable target address fields.
Validate
Address line
City
State
Postal code
PH-TS-016 – Coverage Transformation
Requirement: PH-REQ-019
 Test Type: Transformation
Verify source coverage information maps to the expected PolicyHub coverage information.
PH-TS-017 – Risk Unit Transformation
Requirement: PH-REQ-020
 Test Type: Transformation / Integration
Verify applicable risk-unit information is transformed correctly and remains related to the correct policy.
PH-TS-018 – Participant Transformation
Requirement: PH-REQ-021
 Test Type: Transformation
Verify applicable participant information is transformed into the expected target structure.
PH-TS-019 – Default Value Assignment
Requirement: PH-REQ-022
 Test Type: Transformation
Verify configured default target values are assigned when the applicable source condition requires a default.
PH-TS-020 – Null Value Handling
Requirement: PH-REQ-023
 Test Type: Negative / Transformation
Verify nullable source fields are handled according to the applicable transformation rule.
Also verify that null values are not incorrectly inserted into mandatory target fields.
PH-TS-021 – Target Data Type Validation
Requirement: PH-REQ-024
 Test Type: Database / Transformation
Verify transformed data conforms to target database data types.
PH-TS-022 – Target Field Length Validation
Requirement: PH-REQ-025
 Test Type: Boundary / Negative
Verify values exceeding the applicable target field length are handled appropriately and do not cause unintended data corruption.
6. Required Field and Reject Scenarios
PH-TS-023 – Missing Policy Number
Requirements: PH-REQ-026, PH-REQ-028
 Test Type: Negative / Reject Processing
Submit a source record without the required policy number.
Expected Result
Record fails applicable validation.
Record follows the reject/error process.
Record does not appear as a valid repository policy.
PH-TS-024 – Invalid Policy Date
Requirements: PH-REQ-027, PH-REQ-028
 Test Type: Negative / Data Validation
Submit a fictional record containing an invalid policy date.
Expected Result
Invalid record is identified and handled according to the configured rejection rules.
PH-TS-025 – Invalid Coverage Value
Requirements: PH-REQ-027, PH-REQ-028
 Test Type: Negative / Transformation
Verify an unsupported coverage value is identified.
Expected Result
The record or affected data follows the applicable error rule and invalid coverage is not loaded as valid target data.
PH-TS-026 – Reject Error Information
Requirement: PH-REQ-029
 Test Type: Negative / Error Validation
Verify rejected records contain sufficient information to identify:
Failed record
Processing stage
Validation problem
Error reason
PH-TS-027 – Valid and Invalid Records in Same File
Requirement: PH-REQ-030
 Test Type: Integration / Negative
Process a fictional file containing both valid and invalid records.
Example:
Plain Text
Total Input Records: 100
Valid Records: 95
Invalid Records: 5
Expected Result
Where permitted by the processing design:
95 valid records continue.
5 invalid records follow reject processing.
Invalid records do not appear as valid policies.
Counts remain reconcilable.
PH-TS-028 – Rejected Record Exclusion
Requirement: PH-REQ-031
 Test Type: Negative / Data Integrity
Verify a rejected policy record cannot be found as a valid policy in the PolicyHub Repository.
7. Staging and Integration Scenarios
PH-TS-029 – Staging Layer Load
Requirement: PH-REQ-033
 Test Type: Database / ETL
Verify transformed valid records are loaded into the appropriate staging layer.
PH-TS-030 – Staging Data Accuracy
Requirement: PH-REQ-034
 Test Type: Database / Transformation
Compare expected transformed values against values stored in the staging layer.
PH-TS-031 – Valid Record Promotion
Requirement: PH-REQ-035
 Test Type: ETL / Integration
Verify a record successfully passing validation progresses from staging to the applicable integration layer.
PH-TS-032 – Invalid Record Does Not Progress
Requirement: PH-REQ-036
 Test Type: Negative / Data Integrity
Verify a record failing mandatory validation does not progress into the valid integration layer.
8. Record-Count Reconciliation Scenarios
PH-TS-033 – Complete ETL Record Reconciliation
Requirement: PH-REQ-037
 Test Type: ETL / Reconciliation
Validate record counts across the complete ETL workflow.
Example:
Plain Text
Source File = 1,000
Landing = 1,000
 
Valid = 980
Rejected = 20
 
980 + 20 = 1,000
 
Integration = 980
Repository Load = 980
Expected Result
All records are accounted for.
There are no unexplained missing or duplicate records.
PH-TS-034 – Single Record Traceability
Requirements: PH-REQ-037, PH-REQ-044
 Test Type: ETL / Data Lineage
Trace a single fictional policy:
Plain Text
PH-100245
across:
Plain Text
Source
↓
Landing
↓
Staging
↓
Integration
↓
PolicyHub Repository
Expected Result
The same policy can be traced through all applicable stages, with only documented transformations applied.
9. Repository Load Scenarios
PH-TS-035 – New Policy Insert
Requirements: PH-REQ-038, PH-REQ-039
 Test Type: Positive / Database
Process a policy that does not currently exist.
Expected Result
A new policy is successfully created in PolicyHub.
PH-TS-036 – Existing Policy Update
Requirements: PH-REQ-038, PH-REQ-040
 Test Type: Positive / Database
Process an updated version of an existing policy.
Expected Result
The applicable existing policy information is updated according to the defined business rules.
PH-TS-037 – Duplicate Policy Prevention
Requirement: PH-REQ-041
 Test Type: Negative / Data Integrity
Process an existing policy and verify that the update process does not create an unintended duplicate policy record.
PH-TS-038 – Policy Persistence
Requirement: PH-REQ-042
 Test Type: Database
Verify successfully loaded policy information remains available after ETL processing completes.
PH-TS-039 – Policy Relationship Validation
Requirement: PH-REQ-043
 Test Type: Database / Integration
Verify applicable relationships are maintained between:
Policy
Participant
Coverage
Risk Unit
Expected Result
Each related record remains associated with the correct policy.
PH-TS-040 – Source-to-Target Accuracy
Requirement: PH-REQ-044
 Test Type: Source-to-Target / Database
Compare selected source fields with final repository values after applying documented transformations.
Expected Result
Target values match source information according to the approved transformation rules.
10. ETL Failure and Recovery Scenarios
PH-TS-041 – Processing Error Visibility
Requirement: PH-REQ-032
 Test Type: Negative / Supportability
Verify processing failures provide sufficient information to identify where the failure occurred.
PH-TS-042 – Prevent Partial Repository Load
Requirement: PH-REQ-014
 Test Type: Negative / Data Integrity
Simulate or validate a failed ETL condition.
Expected Result
Incomplete processing does not expose unintended partially processed policy data as successfully loaded repository data.
PH-TS-043 – Corrected Record Reprocessing
Related Requirements: PH-REQ-027, PH-REQ-028, PH-REQ-038
 Test Type: Recovery / Regression
Correct a fictional record that previously failed validation and process the corrected record through the approved workflow.
Expected Result
The corrected record successfully proceeds through validation and reaches the PolicyHub Repository.
11. ETL Regression Scenarios
PH-TS-044 – Existing Valid Policy Feed Regression
Requirement: PH-REQ-111
 Test Type: Regression
Verify an existing valid policy feed continues to process successfully.
PH-TS-045 – Existing Policy Update Regression
Requirement: PH-REQ-111
 Test Type: Regression
Verify existing-policy update processing continues to operate.
PH-TS-046 – Existing Policy Search Data Availability
Requirement: PH-REQ-112
 Test Type: ETL / Regression
After successful ETL processing, verify the policy data required for downstream policy-search functionality remains available in PolicyHub.
12. Phase 1 End-to-End ETL Scenario
PH-TS-E2E-001 – Complete Policy Feed to Repository Validation
Requirements:
PH-REQ-001 through PH-REQ-044, where applicable
Test Type: End-to-End / ETL / Database
Test Data
Example fictional policy:
Plain Text
Policy Number: PH-100245
Effective Date: 2026-01-01
Expiration Date: 2026-12-31
Workflow
Plain Text
Create Source Policy Record
↓
Deliver Policy File
↓
Verify File Availability
↓
Execute Informatica Workflow
↓
Validate Source Record Count
↓
Validate Oracle Landing Data
↓
Validate Transformations
↓
Validate Staging Data
↓
Validate Reject Processing
↓
Validate Integration Data
↓
Determine Insert / Update
↓
Validate PolicyHub Repository
↓
Perform Source-to-Target Reconciliation
Expected Result
The valid source policy successfully progresses through the complete ETL workflow and becomes available in PolicyHub.
All expected values are accurate after applicable transformations.
Record counts reconcile.
No unexplained duplicate or missing records exist.
13. ETL Scenario Coverage Summary
The Phase 1 scenarios provide coverage for:
File receipt
File structure
Filename validation
Empty files
Duplicate files
Informatica workflow execution
Workflow failures
Landing-table validation
Transformation testing
Data-type testing
Field-length testing
Required-field validation
Invalid records
Reject processing
Staging validation
Integration validation
Record-count reconciliation
Single-record traceability
Insert processing
Update processing
Duplicate prevention
Repository persistence
Parent/child relationships
Source-to-target accuracy
Failure handling
Reprocessing
Regression testing
14. Traceability
Example:
Plain Text
PH-REQ-038
↓
PH-TS-035
↓
Detailed Test Case
↓
Execution Result
↓
Defect, if applicable
The detailed PolicyHub test-case document will provide step-by-step execution procedures for a curated subset of these scenarios.
15. Portfolio Disclaimer
PolicyHub is a fictional project created solely for portfolio and learning purposes.
All files, policy numbers, tables, schemas, fields, transformations, record counts, database structures, business rules, expected results, and test data shown in this document are fictional or generalized.
The scenarios demonstrate ETL testing concepts using Informatica PowerCenter, Oracle Database, SQL, and Toad for Oracle.
No confidential employer, client, policyholder, production, financial, or proprietary system information is included.
