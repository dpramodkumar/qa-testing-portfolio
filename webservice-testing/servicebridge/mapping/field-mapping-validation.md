ServiceBridge Field Mapping Validation
Request, Response, Transformation, and Target Application Mapping
Project: ServiceBridge
Testing Type: Web Service / Field Mapping / Data Validation / Integration
Environment: QA Environment

1. Purpose
This document demonstrates the field-level mapping validation approach used for the fictional ServiceBridge web service integration project.

The objective is to verify data throughout the complete integration path:

Claims Application ↓ Service Request ↓ Policy Search Service ↓ Service Response ↓ Mapping / Transformation ↓ Claims Application ↓ UI / Business Record

Testing validates both direct mappings and fields requiring transformation, lookup, defaulting, or conditional processing.

2. Mapping Validation Strategy
Mapping validation is performed at multiple checkpoints.

Request Mapping
Claims Application Source Field
        ↓
Mapping Rule
        ↓
Service Request Field

QA verifies that the correct source information is sent to the service.
Response Mapping
Plain Text
Service Response Field
↓
Mapping Rule
↓
Claims Application Target Field
QA verifies that returned information is stored in the correct target field.
End-to-End Mapping
Plain Text
Business Input
↓
Request
↓
Response
↓
Transformation
↓
Target Application
↓
UI
The final business result must also be validated.
3. Mapping Types
ServiceBridge demonstrates five common mapping patterns.
Direct Mapping
The business value should remain unchanged.
Example:
Plain Text
Response:
PolicyNumber = PH-200245
 
Target:
Policy Number = PH-200245
Transformation Mapping
The source representation changes according to an approved rule.
Example:
Plain Text
Service:
EffectiveDate = 20260101
 
Transformation:
YYYYMMDD → Application Date
 
Target:
Effective Date = 2026-01-01
The format changes while the actual business date remains the same.
Lookup Mapping
An external value is translated into an internal value.
Example:
Plain Text
Service Coverage:
EXT-COV-A
 
↓
 
Coverage Lookup
 
↓
 
Claims Coverage:
LIABILITY-A
Default Mapping
The target receives a default value when the required business condition is satisfied.
Example:
Plain Text
Response Status:
NULL
 
↓
 
Default Rule
 
↓
 
Target Status:
ACTIVE
Defaults should not be applied when the business condition does not allow them.
Conditional Mapping
The target value depends on another business condition.
Example:
Plain Text
Participant Type = PRIMARY
↓
Map to Primary Insured
 
Participant Type = ADDITIONAL
↓
Map to Additional Participant
4. Summary Request Mapping
The Summary Request is generated from information available in the Claims Application.
Representative fictional mappings:
Claims Application Field	Summary Request Field	Mapping Rule	QA ValidationPolicy Number	PolicyIdentifier	Direct	Exact match
Loss Date	LossDate	Direct / format rule	Date comparison
Insured First Name	FirstName	Direct	Exact match
Insured Last Name	LastName	Direct	Exact match
Address Line 1	Address1	Direct	Text comparison
City	City	Direct	Exact match
State	StateCode	Lookup/standardization	Expected code
Postal Code	PostalCode	Format validation	Expected format
5. Summary Request Example
Claims Application
Plain Text
Policy Number:
PH-200245
 
Loss Date:
2026-06-15
 
First Name:
Alex
 
Last Name:
Sample
 
State:
TN
 
Postal Code:
38103
Conceptual Summary Request
Plain Text
PolicyIdentifier = PH-200245
LossDate = 2026-06-15
FirstName = Alex
LastName = Sample
StateCode = TN
PostalCode = 38103
Expected Validation
Plain Text
Claims Policy Number
=
Summary Request PolicyIdentifier
 
Claims Loss Date
=
Summary Request LossDate
 
Claims State
=
Summary Request StateCode
 
6. Summary Request Negative Mapping
Missing Required Policy Number
Example:
Plain Text
Claims Application:
Policy Number = NULL
 
Summary Request:
PolicyIdentifier = NULL
If PolicyIdentifier is required for the applicable scenario:
Expected Result
The request follows the applicable validation process.
The integration should not silently substitute an unrelated policy identifier.
7. Summary Response Mapping
The Summary Response provides candidate policy and risk-unit information.
Representative fictional mappings:
Summary Response Field	Claims Application Field	Mapping Rule	QA ValidationPolicyIdentifier	Policy Number	Direct	Exact match
EffectiveDate	Effective Date	Date transformation	Business-date match
ExpirationDate	Expiration Date	Date transformation	Business-date match
PolicyStatus	Policy Status	Lookup	Expected internal status
PolicySource	Policy Source	Lookup	Expected source
InsuredName	Insured Name	Direct / formatting	Expected text
RiskUnitIdentifier	Risk Unit	Direct	Exact match
RiskAddress	Risk Unit Address	Address transformation	Field comparison
8. Summary Response Validation Example
Fictional Response
Plain Text
PolicyIdentifier:
PH-200245
 
EffectiveDate:
2026-01-01
 
ExpirationDate:
2026-12-31
 
PolicyStatus:
ACTIVE
 
RiskUnitIdentifier:
RU-101
Expected Claims Search Result
Plain Text
Policy Number:
PH-200245
 
Effective Date:
2026-01-01
 
Expiration Date:
2026-12-31
 
Status:
ACTIVE
 
Risk Unit:
RU-101
Expected Result
Applicable search-result values correspond to the Summary Response.
9. Summary-to-Detail Mapping
One of the most important mapping validations occurs between the selected Summary result and the Detail Request.
Plain Text
Summary Response
↓
Selected Policy
↓
Selected Risk Unit
↓
Detail Request
Representative mapping:
Selected Summary Value	Detail Request Field	ExpectedPolicy Number	PolicyIdentifier	Same policy
Risk Unit	RiskUnitIdentifier	Same risk unit
Policy Source	PolicySource	Same source
Loss Date	LossDate	Same business date
Entity Reference	EntityIdentifier	Correct selected entity
10. Summary-to-Detail Consistency
Example:
Plain Text
Summary Selected Policy:
PH-200245
 
Summary Selected Risk Unit:
RU-101
Expected Detail Request:
Plain Text
PolicyIdentifier:
PH-200245
 
RiskUnitIdentifier:
RU-101
A Detail Request containing another risk unit would fail mapping validation.
11. Detail Response Mapping
The Detail Response contains the deeper information required for downstream claim processing.
Representative fictional mappings:
Detail Response Field	Claims Application Field	MappingPolicyIdentifier	Policy Number	Direct
EffectiveDate	Effective Date	Date transformation
ExpirationDate	Expiration Date	Date transformation
PolicyStatus	Policy Status	Lookup
InsuredName	Primary Insured	Conditional
InsuredAddress	Insured Address	Address mapping
RiskUnitIdentifier	Risk Unit	Direct
RiskAddress	Risk Unit Address	Address transformation
CoverageCode	Coverage Code	Lookup
CoverageLimit	Coverage Limit	Numeric
DeductibleAmount	Deductible	Numeric
ParticipantType	Participant Type	Lookup/conditional
ParticipantName	Participant Name	Direct
12. Policy Mapping Validation
Example:
Detail Response
Plain Text
PolicyIdentifier:
PH-200245
 
EffectiveDate:
2026-01-01
 
ExpirationDate:
2026-12-31
 
PolicyStatus:
ACTIVE
 
Claims Application
Plain Text
Policy Number:
PH-200245
 
Effective Date:
2026-01-01
 
Expiration Date:
2026-12-31
 
Status:
ACTIVE
Expected Result
Policy information maps accurately.
13. Insured Mapping Validation
Representative service response:
Plain Text
ParticipantType:
PRIMARY
 
ParticipantName:
Sample Policyholder
Business rule:
Plain Text
If ParticipantType = PRIMARY
then map ParticipantName
to Claims Application Primary Insured
Expected target:
Plain Text
Primary Insured:
Sample Policyholder
14. Additional Participant Mapping
Example:
Plain Text
ParticipantType:
ADDITIONAL
 
ParticipantName:
Sample Additional Participant
Expected:
The participant follows the applicable additional-participant mapping rule rather than replacing the Primary Insured.
15. Address Mapping Validation
A service address may contain:
Plain Text
Address1
Address2
City
State
PostalCode
Target fields may include:
Plain Text
Address Line 1
Address Line 2
City
State
ZIP
Representative mapping:
Response	Claims ApplicationAddress1	Address Line 1
Address2	Address Line 2
City	City
State	State
PostalCode	ZIP
Validation confirms each component reaches the correct target field.
16. Multiple Address Validation
When multiple addresses are returned:
Plain Text
Address A
Address B
Address C
QA verifies that the correct address is selected according to the approved fictional business rule.
Testing should validate:
Address type
Address priority
Selected address
Target mapping
Persistence
17. Risk Unit Mapping
Example Detail Response:
Plain Text
Policy:
PH-200245
 
RiskUnit:
RU-101
Expected:
Plain Text
Claims Policy:
PH-200245
 
Claims Risk Unit:
RU-101
 
QA verifies:
Risk unit belongs to the correct policy.
Selected Summary risk unit matches Detail response.
Detail risk unit maps to the correct Claims Application field.
Another policy's risk unit is not incorrectly associated.
18. Coverage Mapping
Example:
Plain Text
External Coverage Code:
EXT-COV-A
Lookup:
Plain Text
EXT-COV-A
↓
LIABILITY-A
Expected target:
Plain Text
Coverage:
LIABILITY-A
QA validates both the lookup and the policy/risk-unit relationship.
19. Unsupported Coverage
Example fictional value:
Plain Text
CoverageCode:
UNKNOWN-COV
Expected:
Unsupported value follows the applicable validation or exception process.
Unsupported value is not silently treated as a valid internal coverage.
Existing valid coverage data is not corrupted.
20. Coverage Limit Mapping
Example response:
Plain Text
CoverageLimit:
100000
Expected target:
Plain Text
Coverage Limit:
100000
QA validates:
Numeric value
Precision where applicable
Correct coverage association
Correct target field
21. Deductible Mapping
Example response:
Plain Text
DeductibleAmount:
1000
Expected target:
Plain Text
Deductible:
1000
QA validates that the deductible is associated with the correct coverage and risk unit.
22. Null Mapping
Null testing is performed according to field classification.
Optional Field
Response:
Plain Text
Address2 = NULL
Expected:
Processing may continue when Address2 is optional.
Mandatory Field
Response:
Plain Text
PolicyIdentifier = NULL
Expected:
The record follows the applicable business validation or exception process.
A mandatory null must not silently produce valid policy data.
23. Default Mapping
Example:
Plain Text
Response PolicyStatus:
NULL
If the fictional business rule permits:
Plain Text
NULL
↓
Default Rule
↓
ACTIVE
QA validates:
The default condition applies.
ACTIVE is populated.
The default is not used when a different rule applies.
24. Data Type Validation
Representative examples:
Field	Expected TypePolicy Number	Text
Effective Date	Date
Coverage Limit	Numeric
Deductible	Numeric
Risk Unit Identifier	Text
Testing verifies invalid types do not silently create incorrect target data.
25. Field Length Validation
Example:
Plain Text
Target Field Maximum:
30 characters
Test:
Plain Text
Response Value:
35 characters
Expected behavior must follow the approved interface and application rules.
QA validates that the value is not silently corrupted.
26. Policy Date Mapping
Example:
Plain Text
Service Effective Date:
2026-01-01
 
Service Expiration Date:
2026-12-31
Expected:
Plain Text
Claims Effective Date:
2026-01-01
 
Claims Expiration Date:
2026-12-31
Business validation also verifies:
Plain Text
Expiration Date >= Effective Date
27. Loss Date Validation
Example:
Plain Text
Policy Effective:
2026-01-01
 
Policy Expiration:
2026-12-31
 
Loss Date:
2026-06-15
Expected:
Loss date falls within the fictional policy period.
Boundary testing should include:
Plain Text
Loss Date = Effective Date
Loss Date = Expiration Date
Loss Date < Effective Date
Loss Date > Expiration Date
28. Mapping Error Isolation
Mapping testing helps identify where a defect occurred.
Example A
Plain Text
Service Response:
Correct
 
Claims Application:
Incorrect
Likely investigation area:
Plain Text
Mapping / Transformation
Example B
Plain Text
Service Response:
Incorrect
 
Claims Application:
Same incorrect value
Likely investigation area:
Plain Text
Service / Source Data
Example C
Plain Text
Service Response:
Correct
 
Mapped Application Data:
Correct
 
Business Processing:
Incorrect
Likely investigation area:
Plain Text
Business Rule Processing
29. End-to-End Field Traceability
A representative field can be tracked through all applicable layers.
Policy Number
Plain Text
Claims Search:
PH-200245
↓
Summary Request:
PH-200245
↓
Summary Response:
PH-200245
↓
Detail Request:
PH-200245
↓
Detail Response:
PH-200245
↓
Claims Application:
PH-200245
↓
Claim:
PH-200245
Expected:
The same business identifier remains traceable throughout the complete service workflow.
30. Risk Unit Traceability
Plain Text
Summary Response:
RU-101
↓
User Selection:
RU-101
↓
Detail Request:
RU-101
↓
Detail Response:
RU-101
↓
Claims Application:
RU-101
↓
Claim:
RU-101
31. Coverage Traceability
Unlike direct fields, transformed values may differ.
Plain Text
Detail Response:
EXT-COV-A
↓
Lookup Rule
↓
Claims Application:
LIABILITY-A
Expected:
The values differ only because of the documented mapping rule.
32. Refresh Mapping Validation
Policy refresh must apply the same approved mapping rules to the refreshed response.
Plain Text
Existing Claim Policy
↓
Refresh Detail Request
↓
Updated Detail Response
↓
Mapping
↓
Update Existing Policy
QA compares:
Plain Text
Before Refresh
vs.
Service Response
vs.
Expected Mapping
vs.
After Refresh
 
Only approved changed values should be updated.
33. Failed Refresh Mapping Protection
If refresh processing fails:
Plain Text
Existing Valid Data
↓
Refresh Failure
↓
Do Not Apply Invalid Mapping
↓
Preserve Existing Data
Expected:
Incomplete response information does not overwrite existing valid policy information.
34. Mapping Validation Matrix
Representative consolidated mapping:
Business Element	Request	Response	Target	ValidationPolicy Number	PolicyIdentifier	PolicyIdentifier	Policy Number	Direct
Loss Date	LossDate	N/A	Loss Date	Direct/format
Effective Date	N/A	EffectiveDate	Effective Date	Date transform
Expiration Date	N/A	ExpirationDate	Expiration Date	Date transform
Policy Status	N/A	PolicyStatus	Policy Status	Lookup
Insured	Search name	InsuredName	Primary Insured	Conditional
Risk Unit	RiskUnitIdentifier	RiskUnitIdentifier	Risk Unit	Direct
Coverage	N/A	CoverageCode	Coverage	Lookup
Coverage Limit	N/A	CoverageLimit	Coverage Limit	Numeric
Deductible	N/A	DeductibleAmount	Deductible	Numeric
35. Mapping Test Evidence Template
For each mapping validation, capture:
Plain Text
Requirement ID:
Test Case ID:
Business Element:
Source Field:
Source Value:
Request Field:
Request Value:
Response Field:
Response Value:
Mapping Rule:
Expected Target Field:
Expected Target Value:
Actual Target Value:
UI Value:
Status:
Defect ID:
This makes it possible to isolate whether the issue exists in the request, response, mapping, target data, or UI.
36. Requirements Supported
This artifact primarily supports:
Plain Text
SB-REQ-019
Summary Request Mapping
 
SB-REQ-033 through SB-REQ-035
Policy / Risk Unit Selection
 
SB-REQ-043
Detail Request Mapping
 
SB-REQ-054 through SB-REQ-064
Response-to-Application Mapping
 
SB-REQ-065 through SB-REQ-072
Transformation Rules
 
SB-REQ-073 through SB-REQ-081
Address, Participant, Coverage, Limit, and Deductible Rules
 
SB-REQ-127
Refresh Mapping
 
SB-REQ-137 through SB-REQ-142
Cross-Layer Data Consistency
37. Portfolio Confidentiality
The portfolio must not contain actual:
Internal field names
Mapping specification documents
Internal service payloads
Production policy values
Real claim information
Client data
Internal service URLs
Authentication information
Screenshots showing proprietary system data
All mapping examples in this file are intentionally fictional.
38. Portfolio Disclaimer
ServiceBridge, Claims Application, Policy Search Service, fields, policies, risk units, participants, coverage codes, limits, deductibles, mappings, values, transformations, lookups, requests, responses, UI fields, refresh processing, test data, and expected results are fictional or generalized.
This artifact demonstrates field-mapping testing techniques across request creation, service responses, transformations, lookups, default values, null handling, target application validation, refresh processing, and end-to-end data traceability.
No confidential employer, client, claimant, production payload, endpoint, mapping specification, credential, authentication token, certificate, internal hostname, or proprietary system information is included.
