PolicyHub Source-to-Target Mapping and Data Validation
Phase 1 – Policy Data Integration
Project: PolicyHub
ETL Tool: Informatica PowerCenter
Database: Oracle Database
Database Validation Tool: Toad for Oracle
Validation Method: SQL / Source-to-Target Reconciliation
Environment: QA Environment

1. Purpose
This document demonstrates the source-to-target validation approach used for the fictional PolicyHub data integration project.

The objective is to verify that policy data received from the source feed is correctly loaded, transformed, validated, and stored in the PolicyHub Repository.

The primary data flow is:

Source Policy Feed ↓ Informatica PowerCenter ↓ Oracle Landing Layer ↓ Transformation ↓ Oracle Staging Layer ↓ Validation ↓ Integration Layer ↓ PolicyHub Repository

SQL queries executed through Toad for Oracle are used to validate data at applicable database checkpoints.

2. Source-to-Target Validation Strategy
Source-to-target testing validates data at multiple levels.

For each representative field:

Source Value ↓ Landing Value ↓ Transformation Rule ↓ Staging Value ↓ Repository Value

The following areas are validated:

Direct field mappings
Transformed fields
Derived values
Default values
Required fields
Null handling
Date conversion
Data types
Field lengths
Lookup values
Parent/child relationships
Record counts
Insert and update behavior
3. Fictional Source File
The following represents a simplified fictional policy feed.

POLICY_ID|EFF_DATE|EXP_DATE|INSURED_NAME|ADDR1|CITY|STATE|ZIP|RISK_ID|COVERAGE_CD|LIMIT_AMT
PH-100245|20260101|20261231|Sample Organization|100 Test Ave|Memphis|TN|38103|RU-001|COV-A|100000

This data is completely synthetic and used only for portfolio demonstration.
4. Fictional Database Layers
The PolicyHub portfolio uses the following fictional database objects.
Landing Layer
Plain Text
LND_POLICY
Purpose:
Stores source policy information after initial ingestion.
Staging Layer
Plain Text
STG_POLICY
STG_RISK_UNIT
STG_COVERAGE
STG_PARTICIPANT
Purpose:
Stores transformed data before final integration.
Integration Layer
Plain Text
INT_POLICY
INT_RISK_UNIT
INT_COVERAGE
Purpose:
Stores validated data ready for repository processing.
PolicyHub Repository
Plain Text
POLICY
POLICY_RISK_UNIT
POLICY_COVERAGE
POLICY_PARTICIPANT
 
Purpose:
Stores successfully processed policy information used by downstream applications.
Error Processing
Plain Text
ETL_REJECT
Purpose:
Stores information about fictional records rejected from valid processing.
5. Policy Source-to-Target Mapping
Source Field	Landing Field	Transformation	Target Field	ValidationPOLICY_ID	POLICY_ID	Direct mapping	POLICY_NUMBER	Exact match
EFF_DATE	EFF_DATE_RAW	Convert YYYYMMDD to Oracle DATE	EFFECTIVE_DATE	Date comparison
EXP_DATE	EXP_DATE_RAW	Convert YYYYMMDD to Oracle DATE	EXPIRATION_DATE	Date comparison
INSURED_NAME	INSURED_NAME	Trim whitespace	POLICYHOLDER_NAME	Text comparison
ADDR1	ADDRESS_RAW	Address transformation	ADDRESS_LINE_1	Rule validation
CITY	CITY	Upper/standardize if required	CITY_NAME	Text comparison
STATE	STATE_CD	Validate state code	STATE_CODE	Lookup validation
ZIP	POSTAL_CD	Format validation	POSTAL_CODE	Text comparison

6. Risk Unit Mapping
Source Field	Landing Field	Transformation	Target Field	ValidationPOLICY_ID	POLICY_ID	Parent policy lookup	POLICY_NUMBER	Relationship
RISK_ID	RISK_ID	Direct / derived mapping	RISK_UNIT_NUMBER	Exact match
STATE	STATE_CD	State validation	RISK_STATE	Lookup
ZIP	POSTAL_CD	Format validation	RISK_POSTAL_CODE	Exact match
Validation must confirm that the risk unit is associated with the correct policy.
7. Coverage Mapping
Source Field	Landing Field	Transformation	Target Field	ValidationPOLICY_ID	POLICY_ID	Policy relationship	POLICY_NUMBER	Relationship
RISK_ID	RISK_ID	Risk-unit relationship	RISK_UNIT_NUMBER	Relationship
COVERAGE_CD	COVERAGE_CD	Coverage lookup	COVERAGE_CODE	Lookup
LIMIT_AMT	LIMIT_AMT	Numeric conversion	COVERAGE_LIMIT	Numeric comparison
Coverage validation includes both field-level accuracy and relationship validation.
The expected relationship is:
Policy ↓ Risk Unit ↓ Coverage
8. Direct-Mapping Validation
For a direct mapping, the source and target values should match unless a documented formatting rule applies.
Example:
Plain Text
Source:
 
POLICY_ID = PH-100245
 
Target:
 
POLICY_NUMBER = PH-100245
Expected:
Plain Text
Source POLICY_ID = Target POLICY_NUMBER
9. Date Transformation Validation
Example source value:
Plain Text
EFF_DATE = 20260101
Transformation:
Plain Text
YYYYMMDD
↓
Oracle DATE
Expected repository value:
Plain Text
01-JAN-2026
Testing verifies that the format changes but the actual business date does not change.
10. Lookup Validation
Example:
Plain Text
Source Coverage Code
COV-A
↓
Coverage Lookup
↓
PolicyHub Coverage Code
LIABILITY-A
Testing validates that the configured lookup translates the source value to the expected fictional target value.
The validation should also include an invalid source value to confirm error handling.
11. Default-Value Validation
Some target fields may require a default value when an optional source field is not supplied.
Example:
Plain Text
Source STATUS = NULL
Business rule:
Plain Text
If STATUS is null
then TARGET_STATUS = 'ACTIVE'
Expected target:
Plain Text
TARGET_STATUS = ACTIVE
Testing validates that defaults are applied only when the defined condition is satisfied.
12. Null-Handling Validation
Null handling should distinguish between:
Required fields
Optional fields
Fields with default rules
Example:
Plain Text
POLICY_ID = NULL
Expected:
Plain Text
Record rejected
Example:
Plain Text
OPTIONAL_ADDRESS_LINE_2 = NULL
Expected:
Plain Text
Record may continue when allowed by the business rule.
13. Record-Count Reconciliation
Record counts are validated between ETL stages.
Example:
Plain Text
Source File = 1,000
Landing = 1,000
 
Valid = 980
Rejected = 20
 
Integration = 980
Repository = 980
Reconciliation rule:
Plain Text
Source
=
Valid
+
Rejected
Therefore:
Plain Text
1,000 = 980 + 20
No unexplained records should be missing or duplicated.
14. Oracle / Toad SQL Validation Examples
The following SQL statements demonstrate the type of validation that can be performed through Toad for Oracle.
All table names, columns, and data values are fictional.
14.1 Validate Landing Record
SQL
SELECT
POLICY_ID,
EFF_DATE_RAW,
EXP_DATE_RAW,
INSURED_NAME,
RISK_ID,
COVERAGE_CD,
LIMIT_AMT
FROM LND_POLICY
WHERE POLICY_ID = 'PH-100245';
Validation
Compare the returned landing values against the incoming source record.
14.2 Validate Staging Policy
SQL
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE,
POLICYHOLDER_NAME
FROM STG_POLICY
WHERE POLICY_NUMBER = 'PH-100245';
Validation
Confirm:
Policy number is correct.
Effective date transformation is correct.
Expiration date transformation is correct.
Policyholder name transformation is correct.
14.3 Validate Risk Unit
SQL
SELECT
POLICY_NUMBER,
RISK_UNIT_NUMBER,
RISK_STATE,
RISK_POSTAL_CODE
FROM STG_RISK_UNIT
WHERE POLICY_NUMBER = 'PH-100245';
Validation
Confirm that the expected risk unit exists and remains associated with the correct policy.
14.4 Validate Coverage
SQL
SELECT
POLICY_NUMBER,
RISK_UNIT_NUMBER,
COVERAGE_CODE,
COVERAGE_LIMIT
FROM STG_COVERAGE
WHERE POLICY_NUMBER = 'PH-100245';
Validation
Confirm:
Correct policy relationship
Correct risk-unit relationship
Expected coverage mapping
Expected coverage limit
14.5 Validate Integration Record
SQL
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE,
PROCESS_STATUS
FROM INT_POLICY
WHERE POLICY_NUMBER = 'PH-100245';
Expected Result
The valid fictional policy exists in the integration layer with the expected processing status.
14.6 Validate Final Repository Policy
SQL
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE,
POLICYHOLDER_NAME,
POLICY_STATUS
FROM POLICY
WHERE POLICY_NUMBER = 'PH-100245';
Expected Result
The successfully processed policy exists in the PolicyHub Repository with expected values.
14.7 Validate Policy-to-Risk Relationship
SQL
SELECT
p.POLICY_NUMBER,
r.RISK_UNIT_NUMBER
FROM POLICY p
JOIN POLICY_RISK_UNIT r
ON p.POLICY_ID = r.POLICY_ID
WHERE p.POLICY_NUMBER = 'PH-100245';
Expected Result
The expected risk unit is associated with the correct policy.
14.8 Validate Policy-to-Coverage Relationship
SQL
SELECT
p.POLICY_NUMBER,
r.RISK_UNIT_NUMBER,
c.COVERAGE_CODE,
c.COVERAGE_LIMIT
FROM POLICY p
JOIN POLICY_RISK_UNIT r
ON p.POLICY_ID = r.POLICY_ID
JOIN POLICY_COVERAGE c
ON r.RISK_UNIT_ID = c.RISK_UNIT_ID
WHERE p.POLICY_NUMBER = 'PH-100245';
Expected Result
The correct coverage is associated with the expected policy and risk unit.
15. Record-Count SQL Examples
Landing Count
SQL
SELECT COUNT(*) AS LANDING_COUNT
FROM LND_POLICY
WHERE BATCH_ID = 'BATCH-1001';
Valid Integration Count
SQL
SELECT COUNT(*) AS VALID_COUNT
FROM INT_POLICY
WHERE BATCH_ID = 'BATCH-1001';
Reject Count
SQL
SELECT COUNT(*) AS REJECT_COUNT
FROM ETL_REJECT
WHERE BATCH_ID = 'BATCH-1001';
Reconciliation
Expected relationship:
Plain Text
LANDING_COUNT
=
VALID_COUNT
+
REJECT_COUNT
Example:
Plain Text
1000 = 980 + 20
16. Duplicate Validation
The following query demonstrates how duplicate policies could be identified.
SQL
SELECT
POLICY_NUMBER,
COUNT(*) AS RECORD_COUNT
FROM POLICY
GROUP BY POLICY_NUMBER
HAVING COUNT(*) > 1;
Expected Result
No unintended duplicate policy records are returned.
17. Reject Validation
Example:
SQL
SELECT
BATCH_ID,
SOURCE_RECORD_ID,
ERROR_CODE,
ERROR_MESSAGE
FROM ETL_REJECT
WHERE BATCH_ID = 'BATCH-1001';
Validation
Confirm that:
Rejected record is identifiable.
Error code is available.
Error message identifies the validation problem.
Rejected record does not exist as a valid repository policy.
18. Validate Rejected Policy Is Not Loaded
Example fictional rejected policy:
Plain Text
PH-100999
Query:
SQL
SELECT COUNT(*)
FROM POLICY
WHERE POLICY_NUMBER = 'PH-100999';
Expected Result
Plain Text
0
The rejected record must not appear as a valid repository policy.
19. Insert Validation
For a new policy:
SQL
SELECT COUNT(*)
FROM POLICY
WHERE POLICY_N*MBER = 'PH-100245';
Before ETL:
Plain Text
0
After successful ETL:
Plain Text
1
This demonstrates new-policy insert validation.
20. Update Validation
For an existing policy, capture the original information.
Example:
SQL
SELECT
POLICY_NUMBER,
EXP*RATION_DATE,
POLICY_STATUS
FRO* POLICY
WHERE POLICY_NUMBER = 'PH-*00245';
After processing the updated source feed, execute the query again.
Validate:
Expected fields changed.
Fields not included in the approved update remained correct.
No duplicate policy was created.
21. End-to-End Single-Policy Traceability
A representative policy can be traced through every database stage.
Example:
Plain Text
PH-100245
Validation path:
Plain Text
Source CSV
↓
LND_PO*ICY
↓
STG_POLICY
↓
INT_POL*CY
↓
POLICY
↓
POLICY_RISK_*NIT
↓
POLICY_COVERAGE
SQL validation at each stage provides evidence that the same policy progressed through the ETL process.
22. Source-to-Target Test Evidence
For execution, evidence should capture:
Source record
Informatica workflow result
Landing query result
Staging query result
Integration query result
Repository query result
Expected transformation
Actual transformation
Record counts
Reject information when applicable
Sensitive or real production data should never be included in portfolio evidence.
23. Related Requirements
This artifact primarily supports:
PH-REQ-009 – Landing Data Load
PH-REQ-015 – Data Transformation
PH-REQ-016 through PH-REQ-025 – Transformation Rules
PH-REQ-026 through PH-REQ-032 – Validation and Error Processing
PH-REQ-033 through PH-REQ-037 – Staging and Integration
PH-REQ-038 through PH-REQ-044 – Policy Repository
PH-REQ-101 through PH-REQ-107 – Data Consistency and Traceability
24. Related ETL Test Scenarios
This artifact supports scenarios including:
PH-TS-011 – Source-to-Landing Record Count
PH-TS-012 – Source-to-Landing Field Validation
PH-TS-013 through PH-TS-022 – Transformation Validation
PH-TS-023 through PH-TS-028 – Reject Processing
PH-TS-029 through PH-TS-032 – Staging and Integration
PH-TS-033 – Complete ETL Record Reconciliation
PH-TS-034 – Single Record Traceability
PH-TS-035 – New Policy Insert
PH-TS-036 – Existing Policy Update
PH-TS-037 – Duplicate Policy Prevention
PH-TS-039 – Policy Relationship Validation
PH-TS-040 – Source-to-Target Accuracy
25. Portfolio Disclaimer
PolicyHub and all associated database objects are fictional.
All source fields, table names, column names, policy numbers, batch identifiers, SQL queries, transformation rules, lookup values, record counts, and expected results shown in this document are fictional or generalized for portfolio purposes.
The examples demonstrate source-to-target testing techniques using Informatica PowerCenter, Oracle Database, SQL, and Toad for Oracle.
No confidential employer, client, production database, policyholder, claimant, financial, or proprietary system information is included.



