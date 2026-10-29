ClaimPay Requirements Traceability Matrix
Special Payment Processing Enhancement
Application: ClaimPay
Artifact: Requirements Traceability Matrix (RTM)
Environment: QA Environment
Project Type: Fictional Portfolio Project

1. Purpose
The Requirements Traceability Matrix (RTM) maps ClaimPay business requirements to test scenarios, detailed test cases, execution results, and defects.

The RTM is used to demonstrate:

Requirement coverage
Test coverage
Execution status
Defect traceability
Identification of coverage gaps
End-to-end traceability
Traceability follows the structure:

Requirement ↓ Test Scenario ↓ Detailed Test Case ↓ Execution Result ↓ Defect, if applicable

2. Coverage Status Definitions
Status	Meaning
Pass	Requirement was validated successfully by the mapped curated test
Fail	Mapped test identified behavior that did not fully meet the expected result
Blocked	Test could not be completed because a dependency or required test condition was unavailable
Not Executed	Test was designed but was not executed
Planned	Requirement has scenario-level coverage but no detailed curated portfolio test case
Covered by Related Test	Requirement is exercised as part of another detailed business-flow test
3. Configuration Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-001	Global feature configuration	CP-TS-001	CP-TC-001	CP-ER-001	Pass	None
CP-REQ-002	Business-unit configuration	CP-TS-002	—	—	Planned	—
CP-REQ-003	Customer configuration	CP-TS-003 / CP-TS-004	CP-TC-007	CP-ER-007	Pass	None
CP-REQ-004	Configuration precedence	CP-TS-004	CP-TC-007	CP-ER-007	Pass	None
CP-REQ-005	Authorized configuration access	CP-TS-005	CP-TC-001	CP-ER-001	Pass	None
CP-REQ-006	Restricted configuration access	CP-TS-006	CP-TC-002	CP-ER-002	Pass	None
CP-REQ-007	Configuration persistence	CP-TS-005	CP-TC-001	CP-ER-001	Pass	None
4. Special-Payment Identification Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-008	Qualifying transaction identification	CP-TS-007 / CP-TS-008	CP-TC-003 / CP-TC-004	CP-ER-003 / CP-ER-004	Pass	None
CP-REQ-009	Nonqualifying transaction identification	CP-TS-009	CP-TC-005	CP-ER-005	Pass	None
CP-REQ-010	Supported claim categories	—	—	—	Planned	—
CP-REQ-011	Standard and provisional claims	CP-TS-010 / CP-TS-011	CP-TC-003 / CP-TC-022	CP-ER-003 / CP-ER-022	Pass	None
5. Funding Account Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-012	Initial funding account display	CP-TS-012	—	—	Planned	—
CP-REQ-013	Corporate Funding Account assignment	CP-TS-012	CP-TC-003 / CP-TC-006	CP-ER-003 / CP-ER-006	Pass	None
CP-REQ-014	Configured Corporate Funding Account	—	CP-TC-006	CP-ER-006	Covered by Related Test	None
CP-REQ-015	Nonqualifying payment account	CP-TS-013	CP-TC-005	CP-ER-005	Pass	None
CP-REQ-016	Feature-disabled behavior	CP-TS-014	CP-TC-007	CP-ER-007	Pass	None
CP-REQ-017	Payment History funding display	CP-TS-015	CP-TC-006	CP-ER-006	Pass	None
6. Offset Recovery Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-018	Automatic Offset Recovery	CP-TS-016	CP-TC-008	CP-ER-008	Pass	None
CP-REQ-019	Recovery-code mapping	CP-TS-017	CP-TC-009	CP-ER-009	Pass	None
CP-REQ-020	Separate recovery transaction	CP-TS-018	CP-TC-008	CP-ER-008	Pass	None
CP-REQ-021	Recovery payment type	—	—	—	Planned	—
CP-REQ-022	Recovery funding source	—	CP-TC-023	CP-ER-023	Covered by Related Test	None
CP-REQ-023	Recovery organization	—	—	—	Planned	—
CP-REQ-024	Payment/recovery relationship	CP-TS-019	CP-TC-010	CP-ER-010	Pass	None
CP-REQ-025	Financial offset	CP-TS-019	CP-TC-010	CP-ER-010	Pass	None
CP-REQ-026	Transaction reference	CP-TS-020	CP-TC-010	CP-ER-010	Pass	None
CP-REQ-027	No recovery for Standard Payment	CP-TS-021	CP-TC-011	CP-ER-011	Pass	None
7. Payment Processing and Hold Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-028	Special-Payment review	CP-TS-022	CP-TC-014	CP-ER-014	Covered by Related Test	None
CP-REQ-029	Required Special-Payment information	CP-TS-023	CP-TC-003	CP-ER-003	Covered by Related Test	None
CP-REQ-030	Missing required information	CP-TS-024	CP-TC-012	CP-ER-012	Pass	None
CP-REQ-031	Payment authority	CP-TS-022	CP-TC-013 / CP-TC-014	CP-ER-013 / CP-ER-014	Covered by Related Test	None
CP-REQ-032	Hold release security	CP-TS-025	CP-TC-013	CP-ER-013	Pass	None
CP-REQ-033	Authorized hold release	CP-TS-026	CP-TC-014	CP-ER-014	Pass	None
8. Allocation Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-034	Single Special-Payment allocation	CP-TS-027	CP-TC-015	CP-ER-015	Fail	CP-DEF-001
CP-REQ-035	Additional allocation restriction	CP-TS-027	CP-TC-015	CP-ER-015	Fail	CP-DEF-001
CP-REQ-036	Special allocation on existing payment	CP-TS-028	CP-TC-016	CP-ER-016	Pass	None
CP-REQ-037	Allocation validation message	CP-TS-029	CP-TC-015	CP-ER-015	Fail	CP-DEF-001
CP-REQ-038	Standard multiple allocations	CP-TS-030	CP-TC-017	CP-ER-017	Pass	None
9. Transaction-Code Control Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-039	Special-to-standard change restriction	CP-TS-031	CP-TC-018	CP-ER-018	Pass	None
CP-REQ-040	Standard-to-special change restriction	CP-TS-032	—	—	Planned	—
CP-REQ-041	Special-to-special change restriction	CP-TS-033	—	—	Planned	—
CP-REQ-042	Recovery-code change restriction	CP-TS-034	—	—	Planned	—
CP-REQ-043	Code-change validation	CP-TS-035	CP-TC-018	CP-ER-018	Pass	None
CP-REQ-044	Processed-payment change request	CP-TS-036	CP-TC-019	CP-ER-019	Blocked	None
CP-REQ-045	Change-request review	CP-TS-036	CP-TC-019	CP-ER-019	Blocked	None
CP-REQ-046	Linked-recovery warning	CP-TS-037	—	—	Planned	—
CP-REQ-047	Direct-edit restriction	—	—	—	Planned	—
10. Void Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-048	Void Special Payment	CP-TS-038	CP-TC-020	CP-ER-020	Pass	None
CP-REQ-049	Void related recovery	CP-TS-039	CP-TC-020	CP-ER-020	Pass	None
CP-REQ-050	Void activity note	CP-TS-040	CP-TC-021	CP-ER-021	Fail	CP-DEF-002
CP-REQ-051	Void note details	CP-TS-040	CP-TC-021	CP-ER-021	Fail	CP-DEF-002
CP-REQ-052	Standard-process void	CP-TS-041	—	—	Planned	—
CP-REQ-053	Void notification	—	—	—	Planned	—
11. Refund Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-054	Special-Payment refund	CP-TS-042	—	—	Planned	—
CP-REQ-055	Refund Corporate Funding assignment	CP-TS-043	—	—	Planned	—
CP-REQ-056	Feature-disabled refund behavior	CP-TS-044	—	—	Planned	—
CP-REQ-057	Refund history	CP-TS-045	—	—	Planned	—
CP-REQ-058	Refund validation	—	—	—	Planned	—
12. Payment Upload Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-059	Uploaded Special Payments	CP-TS-046	—	—	Planned	—
CP-REQ-060	Uploaded allocation separation	CP-TS-047	—	—	Planned	—
CP-REQ-061	Uploaded Standard allocations	CP-TS-048	—	—	Planned	—
CP-REQ-062	Uploaded funding assignment	CP-TS-049	—	—	Planned	—
CP-REQ-063	Upload result	CP-TS-050	—	—	Planned	—
13. Provisional Claim Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-064	Provisional Special Payment	CP-TS-051	CP-TC-022	CP-ER-022	Pass	None
CP-REQ-065	Provisional Offset Recovery	CP-TS-052	CP-TC-022	CP-ER-022	Pass	None
CP-REQ-066	Provisional recovery restrictions	CP-TS-053	—	—	Planned	—
CP-REQ-067	Claim conversion	CP-TS-054	CP-TC-022	CP-ER-022	Pass	None
CP-REQ-068	Recovery status preservation	CP-TS-054	CP-TC-022	CP-ER-022	Pass	None
CP-REQ-069	Transaction relationship preservation	CP-TS-055	CP-TC-022	CP-ER-022	Pass	None
14. Payment-Move Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-070	Payment-move request	CP-TS-056	CP-TC-023	CP-ER-023	Pass	None
CP-REQ-071	Payment-move review	CP-TS-057	CP-TC-023	CP-ER-023	Pass	None
CP-REQ-072	Related recovery move	CP-TS-058	CP-TC-023	CP-ER-023	Pass	None
CP-REQ-073	Payment funding preservation	CP-TS-059	CP-TC-023	CP-ER-023	Pass	None
CP-REQ-074	Recovery funding preservation	CP-TS-059	CP-TC-023	CP-ER-023	Pass	None
CP-REQ-075	Transaction-reference preservation	CP-TS-060	CP-TC-023	CP-ER-023	Pass	None
CP-REQ-076	Direct move restriction	CP-TS-061	—	—	Planned	—
15. Exception-Handling Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-077	Processing failure	CP-TS-062	CP-TC-024	CP-ER-024	Not Executed	None
CP-REQ-078	Payment status after failure	CP-TS-063	CP-TC-024	CP-ER-024	Not Executed	None
CP-REQ-079	Failure message	CP-TS-063	CP-TC-024	CP-ER-024	Not Executed	None
CP-REQ-080	No orphan recovery	CP-TS-064	CP-TC-024	CP-ER-024	Not Executed	None
CP-REQ-081	No partial financial update	CP-TS-065	CP-TC-024	CP-ER-024	Not Executed	None
CP-REQ-082	Retry after correction	CP-TS-066	—	—	Planned	—
16. Audit and Activity Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-083	Payment Activity History	CP-TS-067	CP-TC-021	CP-ER-021	Covered by Related Test	CP-DEF-002
CP-REQ-084	Recovery Activity History	CP-TS-068	CP-TC-008 / CP-TC-020	CP-ER-008 / CP-ER-020	Pass	None
CP-REQ-085	Configuration audit	CP-TS-069	—	—	Planned	—
CP-REQ-086	Change-request audit	CP-TS-070	CP-TC-019	CP-ER-019	Blocked	None
CP-REQ-087	Payment-move audit	CP-TS-071	CP-TC-023	CP-ER-023	Covered by Related Test	None
17. Security Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-088	Payment Specialist access	CP-TS-072	CP-TC-002 / CP-TC-003	CP-ER-002 / CP-ER-003	Pass	None
CP-REQ-089	Configuration Administrator access	CP-TS-073	CP-TC-001	CP-ER-001	Pass	None
CP-REQ-090	Operations Analyst access	CP-TS-074	CP-TC-023	CP-ER-023	Pass	None
CP-REQ-091	Reviewer access	CP-TS-075	CP-TC-014	CP-ER-014	Pass	None
CP-REQ-092	Unauthorized action prevention	CP-TS-076	CP-TC-002 / CP-TC-013	CP-ER-002 / CP-ER-013	Pass	None
18. Regression Requirements
Requirement	Requirement Summary	Scenario	Test Case	Execution Result	Status	Defect
CP-REQ-093	Standard Payment processing	CP-TS-077	CP-TC-025	CP-ER-025	Pass	None
CP-REQ-094	Standard Funding Account	CP-TS-078	CP-TC-025	CP-ER-025	Pass	None
CP-REQ-095	No Special Recovery for Standard Payment	CP-TS-079	CP-TC-025	CP-ER-025	Pass	None
CP-REQ-096	Standard allocation editing	CP-TS-080	CP-TC-025	CP-ER-025	Pass	None
CP-REQ-097	Standard multiple allocations	CP-TS-081	CP-TC-017 / CP-TC-025	CP-ER-017 / CP-ER-025	Pass	None
CP-REQ-098	Standard payment move	CP-TS-082	—	—	Planned	—
CP-REQ-099	Existing approval process	CP-TS-083	CP-TC-013 / CP-TC-014	CP-ER-013 / CP-ER-014	Pass	None
CP-REQ-100	Existing Claim Activity	CP-TS-084	—	—	Planned	—
19. End-to-End Requirements
Requirement	Requirement Summary	Scenario	Detailed Coverage	Execution Evidence	Status
CP-REQ-101	Complete Special Payment workflow	CP-TS-E2E-001	CP-TC-003 / 006 / 008 / 010 / 014	CP-ER-003 / 006 / 008 / 010 / 014	Pass
CP-REQ-102	Complete void workflow	CP-TS-E2E-002	CP-TC-020 / CP-TC-021	CP-ER-020 / CP-ER-021	Fail
CP-REQ-103	Complete payment-move workflow	CP-TS-E2E-003	CP-TC-023	CP-ER-023	Pass
20. Defect Traceability
CP-DEF-001
Title: Validation message not displayed when additional allocation is attempted on Special Payment

Requirement Impact:

CP-REQ-034
CP-REQ-035
CP-REQ-037
Scenario:

CP-TS-027
CP-TS-029
Test Case:

CP-TC-015

Execution Result:

CP-ER-015

Status: Open

CP-DEF-002
Title: Void activity does not identify related Offset Recovery handling

Requirement Impact:

CP-REQ-050
CP-REQ-051
CP-REQ-083
Scenario:

CP-TS-040
CP-TS-067
Test Case:

CP-TC-021

Execution Result:

CP-ER-021

Status: Open

21. Coverage Gap Review
The curated portfolio test suite intentionally does not create a detailed test case for every individual requirement.

Requirements marked Planned have been retained in the RTM to demonstrate that the requirement exists and that additional test coverage would be required in a full production QA cycle.

Examples include:

Business-unit configuration
Recovery organization validation
Additional transaction-code combinations
Refund processing
Payment Upload
Direct payment-move restrictions
Configuration audit validation
Standard payment-move regression
Requirements marked Blocked or Not Executed are not treated as passed.

This prevents incomplete testing from being represented as successful requirement validation.

22. Traceability Example
Example complete traceability:

CP-REQ-018 Automatic Offset Recovery ↓ CP-TS-016 Verify Offset Recovery is automatically created ↓ CP-TC-008 Create Offset Recovery ↓ CP-ER-008 PASS ↓ No Defect

Example failed traceability:

CP-REQ-037 Allocation Validation Message ↓ CP-TS-029 Verify validation message ↓ CP-TC-015 Additional Allocation Restriction ↓ CP-ER-015 FAIL ↓ CP-DEF-001

23. RTM Conclusion
The RTM demonstrates traceability between ClaimPay business requirements, test scenarios, detailed test cases, synthetic execution results, and fictional defects.

The matrix also identifies requirements that would require additional detailed testing in a full QA cycle.

Pass, Fail, Blocked, Not Executed, Planned, and related-test coverage are kept separate so that test coverage is not overstated.

24. Portfolio Disclaimer
ClaimPay is a fictional application created solely for portfolio and learning purposes.

The requirements, scenarios, test cases, execution results, defects, users, transaction codes, account names, claims, financial values, workflows, and statuses used in this RTM are fictional or generalized.

The RTM is intended to demonstrate requirements traceability and manual QA documentation practices.

It does not represent execution against any employer, client, claimant, banking, production, or proprietary system.
