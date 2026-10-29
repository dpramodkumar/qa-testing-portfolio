PolicyHub QA Test Plan
Multi-System Policy Data Integration and Claim Lifecycle
Project: PolicyHub
Application Type: Multi-System Data Integration
Testing Type: ETL / Database / Integration / Functional / End-to-End
Environment: QA Environment
Document Version: 1.0
Project Type: Fictional Portfolio Project

1. Purpose
The purpose of this test plan is to define the testing strategy for the fictional PolicyHub multi-system policy data integration project.

The project validates policy information from initial source-file ingestion through Informatica processing, Oracle database transformation, PolicyHub repository loading, policy service integration, incident processing, claim creation, reserve creation, payment processing, and final claim closure.

The testing approach validates both individual system components and the complete end-to-end business lifecycle.

2. Project Overview
PolicyHub is a centralized fictional policy repository containing policy, coverage, participant, and risk-unit information.

Policy information enters the environment through a structured external policy feed.

The incoming data is processed through Informatica PowerCenter and Oracle Database before becoming available in PolicyHub.

Downstream applications consume the policy information through the Policy Search Service.

The complete business workflow includes:

Policy Feed ↓ Informatica PowerCenter ↓ Oracle Database ↓ Transformation and Validation ↓ PolicyHub Repository ↓ IncidentHub ↓ Policy Search Service ↓ Policy Selection ↓ ClaimCore ↓ Policy Revalidation ↓ Incident-to-Claim Conversion ↓ Reserve Creation ↓ Payment Processing ↓ Claim Closure

3. Testing Objectives
The primary testing objectives are to verify:

Policy files are received in the expected format.
Informatica workflows process eligible files correctly.
Source records are loaded into Oracle successfully.
Source and target record counts reconcile.
Transformation rules produce expected values.
Required-field validations operate correctly.
Invalid records follow the appropriate reject process.
Valid records progress through each ETL processing stage.
New policies are inserted correctly.
Existing policies are updated correctly.
Duplicate processing does not create duplicate records.
PolicyHub contains accurate policy information.
Policy Search Service requests are generated correctly.
Policy Search Service responses contain expected data.
Policy and risk-unit information reaches IncidentHub.
Incident information transfers correctly to ClaimCore.
ClaimCore revalidates the selected policy.
Incidents convert successfully into claims.
Reserve information is processed correctly.
Payment information is processed correctly.
Claims satisfying closure requirements can be closed.
Policy information remains traceable throughout the complete lifecycle.
4. Test Strategy
Testing will be divided into multiple layers.

4.1 Source File Testing
Testing will validate:

Filename
File availability
File format
File structure
Header information
Required fields
Record counts
Duplicate files
Empty files
Invalid records
Example validation flow:

Source File ↓ File Structure Validation ↓ Record Count ↓ Field-Level Validation ↓ ETL Processing

4.2 Informatica ETL Testing
Informatica PowerCenter testing will validate:

Workflow execution
Session execution
Successful workflow completion
Failed workflow processing
Source record processing
Landing-table load
Transformation processing
Reject processing
Target load
Processing statistics
Workflow validation will include comparison of:

Source Records ↓ Records Read ↓ Records Transformed ↓ Records Loaded ↓ Records Rejected

Any unexplained difference will be investigated.

4.3 Database Testing
Oracle database testing will be performed using SQL and Toad for Oracle.

Testing will validate:

Landing data
Staging data
Integration data
Repository data
Reject/error records
Record counts
Field values
Data types
Null values
Duplicate records
Parent/child relationships
Insert behavior
Update behavior
Example:

Source File ↓ Landing ↓ Staging ↓ Integration ↓ Policy Repository

Data will be compared at applicable checkpoints.

5. Source-to-Target Validation
Source-to-target testing will verify that incoming source data reaches the expected target fields.

Example fictional mapping:

Source:

POLICY_ID = PH-100245

Target:

POLICY_NUMBER = PH-100245

Testing will validate:

Direct mappings
Derived values
Transformation rules
Default values
Required values
Null handling
Date formats
Field lengths
Data types
Lookup values
Relationships between policy entities
A separate Source-to-Target Mapping artifact will document representative mappings.

6. Transformation Testing
Transformation testing will verify business rules applied between the source and target.

Testing will include:

Policy identifier mapping
Date conversion
Address transformation
Coverage mapping
Risk-unit transformation
Participant transformation
Default-value assignment
Null handling
Field-length handling
Data-type validation
For each transformation:

Source Value ↓ Transformation Rule ↓ Expected Target Value ↓ Actual Target Value

will be compared.

7. Record Count Reconciliation
Record counts will be validated at key ETL stages.

Example:

Source File             1,000 records
        ↓
Landing Layer           1,000 records
        ↓
Validation
      /     \
   Valid   Invalid
    980      20
     ↓        ↓
Integration  Reject
    980       20
     ↓
Repository
    980


    Expected reconciliation:
Plain Text
Source Records
=
Valid Records
+
Rejected Records
Example:
Plain Text
1,000 = 980 + 20
Any unexplained count difference will be investigated.
8. Error and Reject Testing
Negative ETL testing will intentionally use invalid fictional records.
Examples include:
Missing policy number
Missing required date
Invalid date format
Invalid coverage
Invalid field length
Incorrect data type
Duplicate record
Unsupported value
Testing will verify:
Invalid record is identified.
Valid records continue when permitted.
Invalid record does not enter the valid repository.
Error information identifies the rejected record.
Error reason is understandable.
Record counts remain reconcilable.
9. Insert and Update Testing
Testing will validate the difference between new and existing policy processing.
New Policy
When the policy does not exist:
Incoming Policy ↓ ETL Validation ↓ Repository Lookup ↓ No Existing Match ↓ INSERT
Existing Policy
When the policy already exists:
Incoming Policy ↓ ETL Validation ↓ Repository Lookup ↓ Existing Match ↓ UPDATE
Testing will verify that:
New policies are inserted.
Existing policies are updated appropriately.
Unchanged information remains accurate.
Duplicate repository records are not unintentionally created.
10. Policy Search Service Testing
Policy Search Service testing will validate requests and responses between downstream applications and PolicyHub.
Testing will include:
Valid policy search
Multiple search results
No-match response
Invalid request
Missing required search criteria
Policy detail request
Policy detail response
Risk-unit response
Coverage information
Service error processing
Request validation will include applicable:
Policy search criteria
Loss date
Policy number
Risk-unit identifier
Response validation will include applicable:
Policy number
Effective dates
Policy status
Risk units
Participants
Coverage information
11. IncidentHub Testing
Testing will validate the fictional incident-intake process.
Coverage will include:
Incident creation
Required incident fields
Loss information
Policy search initiation
Policy search results
Policy selection
Risk-unit selection
Policy association
Incident completion
The selected policy information will be compared with PolicyHub data.
12. IncidentHub-to-ClaimCore Integration Testing
Integration testing will verify that incident information moves correctly from IncidentHub into ClaimCore.
Testing will validate:
Incident transmission
Successful receipt
Incident identifier
Policy identifier
Loss information
Risk-unit information
Field mapping
Data consistency
Integration-error handling
13. ClaimCore Policy Revalidation
Before claim creation is completed, ClaimCore will perform another policy validation.
Testing will validate:
ClaimCore ↓ Policy Search Request ↓ Policy Search Service ↓ PolicyHub ↓ Policy Detail Response ↓ ClaimCore
Validation will include:
Policy number
Loss date
Policy effective dates
Risk unit
Coverage
Applicable policy details
Returned information will be compared with PolicyHub repository data.
14. Incident-to-Claim Conversion Testing
Testing will verify:
Eligible incident can be converted.
Claim number is generated.
Policy remains associated.
Risk unit remains associated.
Coverage remains available.
Loss information remains accurate.
Relevant incident data transfers to the claim.
Claim data persists after reopening.
15. Reserve Testing
Reserve testing will include:
Valid reserve creation
Required fields
Reserve amount
Coverage association
Claim association
Reserve persistence
Invalid reserve data
Financial history
Example:
Claim ↓ Coverage ↓ Reserve = $10,000 ↓ Save ↓ Reopen Claim ↓ Reserve = $10,000
16. Payment Testing
Payment testing will validate:
Payment creation
Required payment information
Payment amount
Claim association
Coverage association
Payment processing
Payment status
Financial history
Invalid payment scenarios
Example:
Claim Reserve $10,000 ↓ Payment $2,000 ↓ Process Payment ↓ Validate Financial History
All financial values used in the portfolio are fictional.
17. Claim Closure Testing
Claim closure testing will verify:
Required closure conditions are satisfied.
Authorized user can close the claim.
Claim status changes appropriately.
Policy information remains available.
Reserve history remains available.
Payment history remains available.
Claim activity remains available.
18. End-to-End Testing
The flagship PolicyHub test will validate the entire fictional lifecycle.
End-to-End Workflow
Create a fictional policy record in the source feed.
Deliver the policy feed.
Execute the Informatica workflow.
Validate source processing.
Validate Oracle landing data.
Validate transformations.
Validate staging data.
Validate repository data.
Create an incident in IncidentHub.
Search for the policy.
Validate the service request.
Validate the service response.
Select the expected policy.
Select the applicable risk unit.
Complete the incident.
Send the incident to ClaimCore.
Validate incident data in ClaimCore.
Trigger policy revalidation.
Validate the second service request.
Validate the policy response.
Convert the incident into a claim.
Validate the claim.
Create a reserve.
Validate the reserve.
Create a payment.
Process the payment.
Validate financial history.
Complete required claim activity.
Close the claim.
Reopen/review the claim.
Validate Closed status.
Validate policy and financial history.
19. End-to-End Data Traceability
One synthetic policy will be followed throughout the complete process.
Example:
Policy Number:
PH-100245
Traceability:
Source File ↓ Oracle Landing ↓ Oracle Staging ↓ PolicyHub Repository ↓ Policy Search Service Response ↓ IncidentHub ↓ ClaimCore ↓ Claim ↓ Closed Claim
Key fields will be compared at applicable checkpoints.
20. Regression Testing
Regression testing will validate that existing functionality continues to operate.
Regression coverage will include:
Valid policy-feed processing
Standard ETL processing
Existing policy updates
Policy searches
Incident creation
Incident transmission
Claim creation
Reserve processing
Payment processing
Claim history
21. Security and Access Testing
Testing will verify fictional role-based access for:
ETL Support User
Policy Search User
Incident User
Claim Examiner
Financial User
QA Database User
Testing will validate that users can access only authorized functions.
Database testing will use read-only QA access where appropriate.
22. Test Data Strategy
All portfolio data will be synthetic.
Example policies:
PH-100245
PH-100246
PH-100247
Example incidents:
INC-70001
INC-70002
Example claims:
CLM-80001
CLM-80002
Test data will include:
New policies
Existing policies
Valid records
Invalid records
Duplicate records
Records with null values
Multiple risk units
Multiple coverage types
Valid incidents
Invalid incident data
Claims eligible for financial processing
No production data will be used.
23. Test Environment
Testing will use a fictional QA environment containing:
Secure File Gateway
Informatica PowerCenter
Oracle Database
PolicyHub Repository
Policy Search Service
IncidentHub
ClaimCore
Database validation will be performed using:
SQL
Toad for Oracle
24. Entry Criteria
Testing may begin when:
Requirements are available.
Required functionality is deployed to QA.
Informatica workflows are available.
Oracle QA database is available.
Required QA applications are accessible.
Policy Search Service is available.
Required user permissions are available.
Synthetic test data is prepared.
Test scenarios are reviewed.
Blocking environmental issues are resolved or documented.
25. Exit Criteria
Testing may be considered complete when:
Critical requirements have been tested.
Planned high-priority tests have been executed.
ETL reconciliation has been completed.
Source-to-target validation has been completed.
Critical service integrations have been tested.
Incident-to-claim integration has been tested.
Reserve and payment workflows have been tested.
End-to-end lifecycle testing has been completed.
Regression testing has been completed.
No unresolved blocking defects remain.
Remaining known issues are documented.
Test results are completed.
Requirements coverage is reviewed.
Blocked or unexecuted tests will not be represented as passed.
26. Defect Management
A defect will be documented when actual behavior differs from the expected result.
Each defect will contain:
Defect ID
Title
Related requirement
Related test case
Processing phase
Environment
Preconditions
Test data
Steps to reproduce
Expected result
Actual result
Severity
Priority
Status
Evidence
Example:
PH-DEF-001
27. Severity Guidelines
Critical
Data corruption, major financial impact, or complete end-to-end processing failure with no workaround.
High
Major functionality fails or policy data cannot progress through a critical processing stage.
Medium
A function behaves incorrectly but the primary business workflow can continue.
Low
Minor formatting, display, or informational issue with limited business impact.
28. Risks and Mitigation
RISK-001 - Test Data Availability
Risk: Required policy or claim conditions may not exist.
Mitigation: Prepare synthetic test data before execution.
RISK-002 - Informatica Dependency
Risk: ETL workflow failure can block downstream testing.
Mitigation: Validate Informatica processing before starting downstream integration tests.
RISK-003 - Database Dependency
Risk: Oracle processing or data availability may delay validation.
Mitigation: Confirm QA database availability before execution.
RISK-004 - Service Dependency
Risk: Policy Search Service failure can block incident and claim testing.
Mitigation: Separate service failures from application defects and document blocked scenarios.
RISK-005 - Multi-System Dependency
Risk: A failure in one upstream application can prevent downstream testing.
Mitigation: Track the originating integration point and avoid reporting downstream symptoms as separate defects without investigation.
RISK-006 - Record Count Mismatch
Risk: Records may be lost or duplicated between ETL stages.
Mitigation: Reconcile source, valid, rejected, staging, and target counts.
29. Test Deliverables
The PolicyHub QA project will contain:
Project README
System Architecture
Business Requirements
QA Test Plan
ETL Test Scenarios
Source-to-Target Mapping
Detailed Test Cases
SQL Validation Examples
Policy Service Testing
End-to-End Lifecycle Testing
Test Execution Results
Sample Defects
Requirements Traceability Matrix
30. Traceability
Testing will maintain traceability across:
Requirement ↓ Test Scenario ↓ Detailed Test Case ↓ Execution Result ↓ Defect, if applicable
Example:
PH-REQ-038 ↓ PH-TS-038 ↓ PH-TC-012 ↓ PH-ER-012 ↓ PH-DEF-001, if applicable
31. Portfolio Disclaimer
PolicyHub, IncidentHub, ClaimCore, and Policy Search Service are fictional names created solely for portfolio demonstration.
All policy records, files, databases, schemas, tables, fields, service requests, endpoints, users, incidents, claims, financial values, expected results, defects, business rules, and test data presented in this project are fictional or generalized.
The project demonstrates testing concepts involving Informatica PowerCenter, Oracle Database, Toad for Oracle, SQL validation, ETL testing, database testing, service integration, functional testing, financial workflow validation, and end-to-end multi-system testing.
No confidential employer, client, claimant, production, financial, or proprietary system information is included.
