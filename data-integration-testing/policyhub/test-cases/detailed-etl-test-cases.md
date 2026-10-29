PolicyHub Detailed ETL Test Cases
Phase 1 – Policy Data Integration
ETL Tool: Informatica PowerCenter
Database: Oracle Database
Database Validation: Toad for Oracle / SQL
Environment: QA Environment

1. Purpose
This document contains a curated set of detailed ETL test cases for the fictional PolicyHub project.

The test cases validate:

Source file processing
Informatica workflow execution
Oracle landing data
Data transformations
Required-field validation
Reject processing
Record-count reconciliation
Staging and integration processing
New policy inserts
Existing policy updates
Duplicate prevention
Source-to-target accuracy
All data, filenames, database objects, SQL examples, and expected results are fictional.

2. Common Test Data
Valid Policy
Policy Number:

PH-100245

Effective Date:

2026-01-01

Expiration Date:

2026-12-31

Risk Unit:

RU-001

Coverage:

COV-A

Coverage Limit:

100000

Existing Policy
Policy Number:

PH-100246

This policy already exists in PolicyHub and will be used for update testing.

Invalid Policy
Policy Number:

PH-100999

This record will contain invalid or missing information for reject testing.

3. File Processing Test Cases
PH-TC-001 – Process Valid Policy Feed
Requirements: PH-REQ-001, PH-REQ-002, PH-REQ-006
Scenarios: PH-TS-001, PH-TS-002
Test Type: Positive / File Integration

Preconditions
Secure File Gateway is available.
Informatica workflow is available.
Valid synthetic policy file exists.
Steps
Prepare a valid policy feed.
Verify the filename follows the expected convention.
Verify required columns are present.
Place the file in the fictional inbound location.
Verify the file is available for Informatica processing.
Expected Result
File is successfully received.
File structure is valid.
File is available for ETL processing.
PH-TC-002 – Process Empty Policy File
Requirement: PH-REQ-004
Scenario: PH-TS-005
Test Type: Negative / Boundary

Preconditions
An input file exists containing the expected structure but no policy records.

Steps
Place the empty file in the inbound location.
Initiate the ETL workflow.
Review Informatica processing status.
Query applicable Oracle target tables.
Expected Result
Empty file is identified appropriately.
No unintended policy records are created.
Processing status is available for review.
PH-TC-003 – Prevent Duplicate File Processing
Requirement: PH-REQ-005
Scenario: PH-TS-006
Test Type: Negative / Data Integrity

Steps
Process an eligible policy file successfully.
Record the resulting repository data.
Submit the same file again.
Execute the applicable processing workflow.
Query PolicyHub for the policy.
Check for duplicate records.
Expected Result
Repeated file processing does not create unintended duplicate policy records.

4. Informatica Test Cases
PH-TC-004 – Validate Informatica Workflow Execution
Requirements: PH-REQ-007, PH-REQ-008, PH-REQ-012
Scenarios: PH-TS-007, PH-TS-008
Test Type: Positive / ETL

Steps
Submit a valid policy feed.
Start or monitor the Informatica workflow.
Verify the workflow starts.
Verify required sessions execute.
Review source records read.
Review target records processed.
Verify workflow completion status.
Expected Result
Informatica reads the expected source records.
Required sessions complete.
Workflow completes successfully.
Processing statistics are available.
PH-TC-005 – Validate Failed Informatica Workflow
Requirements: PH-REQ-013, PH-REQ-014
Scenarios: PH-TS-010, PH-TS-042
Test Type: Negative / Error Handling

Preconditions
A controlled fictional failure condition is available.

Steps
Submit test data for the failure condition.
Execute the ETL workflow.
Review workflow status.
Review applicable error information.
Query repository data.
Check for incomplete processed records.
Expected Result
Workflow indicates failure.
Failure information is available.
Incomplete records are not exposed as successfully processed policies.
5. Landing-Layer Test Cases
PH-TC-006 – Validate Source-to-Landing Data
Requirements: PH-REQ-009, PH-REQ-010, PH-REQ-011
Scenarios: PH-TS-009, PH-TS-011, PH-TS-012
Test Type: Database / Source-to-Target

Test Data
Policy:

PH-100245

Steps
Locate PH-100245 in the source file.
Record key source values.
Process the policy feed.
Open Toad for Oracle.
Query the fictional landing table:
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

Compare the Oracle results with source values.
Expected Result
PH-100245 exists in landing.
Direct source values match.
No unexpected data loss occurs.
Landing record count is consistent with processing rules.
6. Transformation Test Cases
PH-TC-007 – Validate Date Transformation
Requirement: PH-REQ-017
 Scenario: PH-TS-014
 Test Type: Transformation
Test Data
Source:
Plain Text
EFF_DATE = 20260101
EXP_DATE = 20261231
Steps
Process PH-100245.
Record source date values.
Query the staging policy record.
SQL
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE
FROM STG_POLICY
WHERE POLICY_NUMBER = 'PH-100245';
Compare source and staging dates.
Expected Result
Plain Text
20260101 → 01-JAN-2026
20261231 → 31-DEC-2026
The representation changes, but the business dates remain accurate.
PH-TC-008 – Validate Coverage Transformation
Requirement: PH-REQ-019
 Scenario: PH-TS-016
 Test Type: Transformation / Database
Steps
Process PH-100245 using source coverage COV-A.
Query staging coverage.
SQL
SELECT
POLICY_NUMBER,
RISK_UNIT_NUMBER,
COVERAGE_CODE,
COVERAGE_LIMIT
FROM STG_COVERAGE
WHERE POLICY_NUMBER = 'PH-100245';
 
Compare actual coverage against expected mapping.
Verify coverage limit.
Expected Result
Coverage is transformed according to the fictional mapping.
Coverage amount remains accurate.
Coverage is associated with the proper risk unit.
PH-TC-009 – Validate Policy / Risk Unit Relationship
Requirements: PH-REQ-020, PH-REQ-043
 Scenarios: PH-TS-017, PH-TS-039
 Test Type: Integration / Database
Steps
Process PH-100245.
Query PolicyHub:
SQL
SELECT
p.POLICY_NUMBER,
r.RISK_UNIT_NUMBER
FROM POLICY p
JOIN POLICY_RISK_UNIT r
ON p.POLICY_ID = r.POLICY_ID
WHERE p.POLICY_NUMBER = 'PH-100245';
Verify RU-001 is returned.
Verify the relationship belongs to PH-100245.
Expected Result
Risk unit RU-001 is associated with the correct policy.
7. Reject Processing Test Cases
PH-TC-010 – Reject Missing Policy Number
Requirements: PH-REQ-026, PH-REQ-028, PH-REQ-029, PH-REQ-031
Scenarios: PH-TS-023, PH-TS-026, PH-TS-028
Test Type: Negative / ETL
Test Data
Create a source record where:
Plain Text
POLICY_ID = NULL
Steps
Include the invalid record in the source feed.
Process the file.
Review Informatica status.
Query ETL reject data.
SQL
SELECT
SOURCE_RECORD_ID,
ERROR_CODE,
ERROR_MESSAGE
FROM ETL_REJECT
WHERE BATCH_ID = 'BATCH-1001';
Attempt to locate the invalid record in the final repository.
Expected Result
Invalid record is rejected.
Error information identifies the failure.
Invalid record does not appear as a valid policy.
Valid records in the file continue when permitted.
PH-TC-011 – Mixed Valid and Invalid Records
Requirement: PH-REQ-030
 Scenario: PH-TS-027
 Test Type: Integration / Negative
Test Data
Plain Text
Total records = 100
Valid records = 95
Invalid records = 5
Steps
Process the mixed source file.
Record Informatica source count.
Query valid integration records.
Query rejected records.
Reconcile counts.
Verify valid records reached the repository.
Verify rejected records did not.
Expected Result
Plain Text
100 = 95 valid + 5 rejected
All records are accounted for.
8. Reconciliation Test Cases
PH-TC-012 – Reconcile ETL Record Counts
Requirement: PH-REQ-037
 Scenario: PH-TS-033
 Test Type: ETL / Reconciliation
Steps
Capture source-file count.
Capture landing count.
Capture valid integration count.
Capture reject count.
Capture target repository count.
Compare results.
Expected Result
Example:
Plain Text
Source = 1,000
Landing = 1,000
Valid = 980
Rejected = 20
Repository = 980
 
1,000 = 980 + 20
There are no unexplained missing or duplicate records.
9. Repository Processing Test Cases
PH-TC-013 – Insert New Policy
Requirements: PH-REQ-038, PH-REQ-039
 Scenario: PH-TS-035
 Test Type: Positive / Database
Steps
Verify PH-100245 does not currently exist.
SQL
SELECT COUNT(*)
FROM POLICY
WHERE POLICY_NUMBER = 'PH-100245';
Process the new policy feed.
Execute the query again.
Validate repository values.
Expected Result
Before processing:
Plain Text
0 records
After processing:
Plain Text
1 record
The new policy is created successfully.
PH-TC-014 – Update Existing Policy
Requirements: PH-REQ-038, PH-REQ-040
 Scenario: PH-TS-036
 Test Type: Positive / Database
Test Data
Existing policy:
PH-100246
Steps
Query existing PH-100246 values.
Record the existing expiration date and status.
Prepare an approved source update.
Process the update through Informatica.
Query PH-100246 again.
Compare before and after values.
Check for duplicates.
Expected Result
Approved values are updated.
Unaffected values remain correct.
Only one applicable policy record exists.
No unintended duplicate is created.
PH-TC-015 – Prevent Duplicate Policy
Requirement: PH-REQ-041
 Scenario: PH-TS-037
 Test Type: Negative / Database
Steps
Process an existing policy update.
Execute:
SQL
SELECT
POLICY_NUMBER,
COUNT(*) AS RECORD_COUNT
FROM POLICY
WHERE POLICY_NUMBER = 'PH-100246'
GROUP BY POLICY_NUMBER
HAVING COUNT(*) > 1;
Expected Result
No unintended duplicate policy is returned.
10. Complete Policy Traceability Test
PH-TC-016 – Trace Policy from Source to Repository
Requirements: PH-REQ-044, PH-REQ-101, PH-REQ-106, PH-REQ-116
Scenarios: PH-TS-034, PH-TS-040, PH-TS-E2E-001
Test Type: End-to-End / ETL / Database
Test Data
Plain Text
Policy Number: PH-100245
Risk Unit: RU-001
Coverage: COV-A
Coverage Limit: 100000
Steps
Verify PH-100245 in the source file.
Process the source file.
Verify Informatica workflow completion.
Query LND_POLICY.
Validate source-to-landing values.
Query STG_POLICY.
Validate transformed policy fields.
Query STG_RISK_UNIT.
Validate risk-unit data.
Query STG_COVERAGE.
Validate coverage data.
Query INT_POLICY.
Confirm policy successfully reached integration.
Query POLICY.
Verify the final repository policy.
Query POLICY_RISK_UNIT.
Verify RU-001.
Query POLICY_COVERAGE.
Verify expected coverage.
Reconcile applicable record counts.
Expected Result
PH-100245 can be traced through:
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
All applicable values match according to documented source-to-target transformation rules.
No unexplained data loss, duplication, or relationship errors exist.
11. Traceability Summary
Test Case	Primary ValidationPH-TC-001	Valid file processing
PH-TC-002	Empty file
PH-TC-003	Duplicate file
PH-TC-004	Informatica workflow
PH-TC-005	ETL failure
PH-TC-006	Source-to-landing
PH-TC-007	Date transformation
PH-TC-008	Coverage transformation
PH-TC-009	Policy/risk-unit relationship
PH-TC-010	Reject processing
PH-TC-011	Mixed valid/invalid records
PH-TC-012	Record reconciliation
PH-TC-013	New policy insert
PH-TC-014	Existing policy update
PH-TC-015	Duplicate prevention
PH-TC-016	Complete source-to-target traceability
12. Portfolio Disclaimer
PolicyHub and all database objects, source files, policy identifiers, transformation rules, SQL queries, record counts, expected results, and test data shown here are fictional or generalized.
The test cases demonstrate ETL testing techniques using Informatica PowerCenter, Oracle Database, Toad for Oracle, and SQL.
No confidential employer, client, production, policyholder, claimant, or proprietary system data is included.

## 4. Preview and commit

Click **Preview**, review the formatting, then:

**Commit changes...**

Use:

```text
Add PolicyHub detailed ETL test cases

Commit to main.
Step 7B: Dedicated Oracle/Toad SQL Validation File
Now create the second artifact.
1. Return to the policyhub folder
Click:
Add file → Create new file
Enter:
Plain Text
sql-validation/oracle-validation-queries.sql
The .sql extension is intentional. GitHub will display it as SQL rather than Markdown documentation.
2. Paste this SQL
SQL
/*
==============================================================
PolicyHub Oracle / Toad SQL Validation Examples
==============================================================
 
Purpose:
Demonstrate SQL techniques used to validate ETL processing
across landing, staging, integration, reject, and repository
layers.
 
Tool:
Toad for Oracle
 
Database:
Oracle Database
 
IMPORTANT:
All schemas, tables, columns, policies, batch identifiers,
values, and queries are fictional and created only for
portfolio demonstration.
==============================================================
*/
 
 
/* ============================================================
1. LANDING LAYER VALIDATION
============================================================ */
 
-- Validate individual policy loaded from source.
 
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
 
 
/* ============================================================
2. LANDING RECORD COUNT
============================================================ */
 
SELECT
COUNT(*) AS LANDING_COUNT
FROM LND_POLICY
WHERE BATCH_ID = 'BATCH-1001';
 
 
/* ============================================================
3. STAGING POLICY VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE,
POLICYHOLDER_NAME,
PROCESS_STATUS
FROM STG_POLICY
WHERE POLICY_NUMBER = 'PH-100245';
 
 
/* ============================================================
4. DATE TRANSFORMATION VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
TO_CHAR(EFFECTIVE_DATE, 'YYYYMMDD') AS EFFECTIVE_DATE_CHECK,
TO_CHAR(EXPIRATION_DATE, 'YYYYMMDD') AS EXPIRATION_DATE_CHECK
FROM STG_POLICY
WHERE POLICY_NUMBER = 'PH-100245';
 
/*
Expected fictional result:
 
EFFECTIVE_DATE_CHECK = 20260101
EXPIRATION_DATE_CHECK = 20261231
*/
 
 
/* ============================================================
5. RISK UNIT VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
RISK_UNIT_NUMBER,
RISK_STATE,
RISK_POSTAL_CODE
FROM STG_RISK_UNIT
WHERE POLICY_NUMBER = 'PH-100245';
 
 
/* ============================================================
6. COVERAGE VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
RISK_UNIT_NUMBER,
COVERAGE_CODE,
COVERAGE_LIMIT
FROM STG_COVERAGE
WHERE POLICY_NUMBER = 'PH-100245';
 
 
/* ============================================================
7. PARTICIPANT VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
PARTICIPANT_TYPE,
PARTICIPANT_NAME
FROM STG_PARTICIPANT
WHERE POLICY_NUMBER = 'PH-100245';
 
 
/* ============================================================
8. INTEGRATION LAYER VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE,
PROCESS_STATUS
FROM INT_POLICY
WHERE POLICY_NUMBER = 'PH-100245';
 
 
/* ============================================================
9. VALID INTEGRATION COUNT
============================================================ */
 
SELECT
COUNT(*) AS VALID_COUNT
FROM INT_POLICY
WHERE BATCH_ID = 'BATCH-1001'
AND PROCESS_STATUS = 'VALID';
 
 
/* ============================================================
10. REJECT COUNT
============================================================ */
 
SELECT
COUNT(*) AS REJECT_COUNT
FROM ETL_REJECT
WHERE BATCH_ID = 'BATCH-1001';
 
 
/* ============================================================
11. REJECT DETAILS
============================================================ */
 
SELECT
BATCH_ID,
SOURCE_RECORD_ID,
ERROR_CODE,
ERROR_MESSAGE
FROM ETL_REJECT
WHERE BATCH_ID = 'BATCH-1001'
ORDER BY SOURCE_RECORD_ID;
 
 
/* ============================================================
12. CONFIRM REJECTED POLICY WAS NOT LOADED
============================================================ */
 
SELECT
COUNT(*) AS REJECTED_POLICY_COUNT
FROM POLICY
WHERE POLICY_NUMBER = 'PH-100999';
 
/*
Expected result:
 
REJECTED_POLICY_COUNT = 0
*/
 
 
/* ============================================================
13. FINAL POLICY REPOSITORY VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE,
POLICYHOLDER_NAME,
POLICY_STATUS
FROM POLICY
WHERE POLICY_NUMBER = 'PH-100245';
 
 
/* ============================================================
14. POLICY / RISK UNIT RELATIONSHIP
============================================================ */
 
SELECT
p.POLICY_NUMBER,
r.RISK_UNIT_NUMBER,
r.RISK_STATE,
r.RISK_POSTAL_CODE
FROM POLICY p
JOIN POLICY_RISK_UNIT r
ON p.POLICY_ID = r.POLICY_ID
WHERE p.POLICY_NUMBER = 'PH-100245';
 
 
/* ============================================================
15. POLICY / RISK / COVERAGE RELATIONSHIP
============================================================ */
 
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
 
 
/* ============================================================
16. POLICY / PARTICIPANT RELATIONSHIP
============================================================ */
 
SELECT
p.POLICY_NUMBER,
pp.PARTICIPANT_TYPE,
pp.PARTICIPANT_NAME
FROM POLICY p
JOIN POLICY_PARTICIPANT pp
ON p.POLICY_ID = pp.POLICY_ID
WHERE p.POLICY_NUMBER = 'PH-100245';
 
 
/* ============================================================
17. DUPLICATE POLICY CHECK
============================================================ */
 
SELECT
POLICY_NUMBER,
COUNT(*) AS RECORD_COUNT
FROM POLICY
GROUP BY POLICY_NUMBER
HAVING COUNT(*) > 1;
 
 
/*
Expected result:
 
No unintended duplicate policies returned.
*/
 
 
/* ============================================================
18. NULL REQUIRED FIELD VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE
FROM POLICY
WHERE POLICY_NUMBER IS NULL
OR EFFECTIVE_DATE IS NULL
OR EXPIRATION_DATE IS NULL;
 
 
/*
Expected result:
 
No records when all fields are mandatory according to
the fictional business rules.
*/
 
 
/* ============================================================
19. EFFECTIVE / EXPIRATION DATE LOGIC
============================================================ */
 
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE
FROM POLICY
WHERE EXPIRATION_DATE < EFFECTIVE_DATE;
 
 
/*
Expected result:
 
No invalid date ranges.
*/
 
 
/* ============================================================
20. ORPHAN RISK UNIT CHECK
============================================================ */
 
SELECT
r.RISK_UNIT_ID,
r.RISK_UNIT_NUMBER
FROM POLICY_RISK_UNIT r
LEFT JOIN POLICY p
ON r.POLICY_ID = p.POLICY_ID
WHERE p.POLICY_ID IS NULL;
 
 
/*
Expected result:
 
No orphan risk units.
*/
 
 
/* ============================================================
21. ORPHAN COVERAGE CHECK
============================================================ */
 
SELECT
c.COVERAGE_ID,
c.COVERAGE_CODE
FROM POLICY_COVERAGE c
LEFT JOIN POLICY_RISK_UNIT r
ON c.RISK_UNIT_ID = r.RISK_UNIT_ID
WHERE r.RISK_UNIT_ID IS NULL;
 
 
/*
Expected result:
 
No orphan coverage records.
*/
 
 
/* ============================================================
22. NEW POLICY INSERT VALIDATION
============================================================ */
 
SELECT
COUNT(*) AS POLICY_COUNT
FROM POLICY
WHERE POLICY_NUMBER = 'PH-100245';
 
/*
Before new-policy ETL:
POLICY_COUNT = 0
 
After successful new-policy ETL:
POLICY_COUNT = 1
*/
 
 
/* ============================================================
23. EXISTING POLICY UPDATE VALIDATION
============================================================ */
 
SELECT
POLICY_NUMBER,
EFFECTIVE_DATE,
EXPIRATION_DATE,
POLICY_STATUS
FROM POLICY
WHERE POLICY_NUMBER = 'PH-100246';
 
/*
Execute before and after processing the fictional update
and compare approved changed fields.
*/
 
 
/* ============================================================
24. BATCH-LEVEL PROCESSING SUMMARY
============================================================ */
 
SELECT
BATCH_ID,
PROCESS_STATUS,
COUNT(*) AS RECORD_COUNT
FROM INT_POLICY
WHERE BATCH_ID = 'BATCH-1001'
GROUP BY
BATCH_ID,
PROCESS_STATUS
ORDER BY
PROCESS_STATUS;
 
 
/* ============================================================
25. SOURCE / VALID / REJECT RECONCILIATION
============================================================ */
 
/*
Example expected calculation:
 
SOURCE COUNT = 1000
VALID COUNT = 980
REJECT COUNT = 20
 
SOURCE = VALID + REJECT
 
1000 = 980 + 20
*/
 
 
/* ============================================================
26. FULL POLICY DATA VIEW FOR QA VALIDATION
============================================================ */
 
SELECT
p.POLICY_NUMBER,
p.EFFECTIVE_DATE,
p.EXPIRATION_DATE,
p.POLICY_STATUS,
r.RISK_UNIT_NUMBER,
c.COVERAGE_CODE,
c.COVERAGE_LIMIT
FROM POLICY p
LEFT JOIN POLICY_RISK_UNIT r
ON p.POLICY_ID = r.POLICY_ID
LEFT JOIN POLICY_COVERAGE c
ON r.RISK_UNIT_ID = c.RISK_UNIT_ID
WHERE p.POLICY_NUMBER = 'PH-100245'
ORDER BY
r.RISK_UNIT_NUMBER,
c.COVERAGE_CODE;
 
 
/*
==============================================================
END OF POLICYHUB ORACLE VALIDATION EXAMPLES
 
These statements demonstrate QA validation concepts only.
They are not queries from any employer or production
environment.
==============================================================
*/
3. Commit the SQL separately
Click Commit changes...
Use:
Plain Text
Add PolicyHub Oracle SQL validation examples
Commit to main.
Step 7 completion check
Your PolicyHub structure should now contain:
Plain Text
policyhub/
│
├── README.md
├── architecture/
│ └── end-to-end-data-flow.md
├── requirements/
│ └── business-requirements.md
├── test-plan/
│ └── test-plan.md
├── test-scenarios/
│ └── etl-test-scenarios.md
├── source-to-target/
│ └── source-to-target-mapping.md
├── test-cases/
│ └── detailed-etl-test-cases.md
└── sql-validation/
└── oracle-validation-queries.sql
This is a particularly useful section of your portfolio because it shows three interconnected abilities:
Plain Text
Business Requirement
↓
ETL Test Scenario
↓
Detailed Test Case
↓
Oracle / Toad SQL
↓
Database Evidence
So instead of your profile merely saying “SQL, Oracle Database, Toad for Oracle, Informatica PowerCenter,” the repository now demonstrates how you use them together for QA validation.
Next step
I would not create ETL results yet. The next logical artifact is the service-integration side:
Plain Text
policyhub/service-testing/policy-service-testing.md
That will move the story from:
Policy Feed → Informatica → Oracle → PolicyHub
into:
IncidentHub → Policy Search Service → PolicyHub → response → policy/risk-unit selection
and after that we'll connect the flow to ClaimCore → policy revalidation → claim → reserve → payment → closure.
Design rationale: the ETL cases are curated around meaningful QA behaviors rather than creating one case per requirement, while the dedicated .sql artifact demonstrates reusable Oracle/Toad validation techniques such as reconciliation, duplicate detection, null checks, referential-integrity checks, inserts, updates, rejects, and policy-risk-coverage joins.
