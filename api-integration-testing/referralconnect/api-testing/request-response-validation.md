ReferralConnect Request and Response Validation
API and Integration Message Validation Framework
Project: ReferralConnect
Applications: IntakeConnect, ClaimCore, ReferralHub
Environment: QA Environment
Validation Tools: Postman / Bruno, when applicable
Testing Type: API / Integration / Field Mapping / UI Reconciliation

1. Purpose
This document defines how ReferralConnect requests, messages, responses, acknowledgements, and target-system results are validated.

The portfolio remains technology-neutral and does not claim an unconfirmed:

REST or SOAP implementation
HTTP method
JSON or XML payload
Authentication mechanism
Endpoint
Status-code contract
Queue or middleware technology
The same validation framework can be applied after the actual interface contract is known.

2. Complete Validation Chain
A successful technical response does not by itself prove that business processing succeeded.

Validation follows:

Business Event
      ↓
Integration Trigger
      ↓
Request / Message
      ↓
Transport Result
      ↓
Response / Acknowledgement
      ↓
Target-System Processing
      ↓
Target Data
      ↓
Target UI

All applicable checkpoints must be reviewed.
3. Business Trigger Validation
Before reviewing the integration message, verify the correct business event occurred.
Integration	TriggerExisting referral retrieval	Incident referral search
Intake integration	Completed incident submission
New referral integration	Eligible referral creation
Examiner integration	Examiner reassignment
Status integration	Eligible referral-status change
Validate:
Trigger occurred once.
Trigger occurred for the correct business record.
Trigger occurred after the business transaction was saved.
An unrelated update did not trigger the integration.
Repeated saves do not create unintended duplicate transactions.
4. Integration Metadata
Where available, validate:
Plain Text
Transaction identifier
Correlation identifier
Source application
Target application
Event type
Processing timestamp
Message version
Processing status
Retry count
Expected:
Transaction can be traced.
Source and target are correct.
Event type matches the business trigger.
Correlation data links the request and response.
Retry processing does not create duplicate business records.
5. Authentication and Authorization Validation
Without exposing implementation details, verify:
Authentication information is present when required.
Invalid authentication is rejected.
Missing authentication is rejected.
Authorized sender can use the integration.
Unauthorized sender cannot access restricted data.
Sensitive authentication values are not written into public portfolio evidence.
Never commit:
Plain Text
Passwords
Access tokens
Refresh tokens
API keys
Client secrets
Certificates
Session cookies
Internal authentication headers
6. Request Structure Validation
Validate the request or message against the applicable interface contract.
Check:
Required sections
Required fields
Optional fields
Field order when contractually relevant
Data types
Field lengths
Enumerated values
Date formats
Identifier formats
Null handling
Character encoding
Nested structure
Repeating elements
7. Representative Referral Message
The following is a conceptual, fictional representation. It is not an actual internal payload.
Plain Text
Transaction ID: TXN-90001
Event Type: REFERRAL_CREATED
Source: ReferralHub
Target: ClaimCore
 
Claim ID: CLM-50001
Referral ID: REF-70002
Referral Type: SERVICE_REVIEW
Referral Status: OPEN
Assigned Examiner: Examiner-A
Referral Date: 2026-10-15
Vendor Reference: VREF-80001
Validate:
Transaction ID is available for traceability.
Event Type matches referral creation.
Claim ID identifies the correct claim.
Referral ID identifies the new referral.
Status is supported.
Examiner value follows the applicable rule.
Date value uses the required format.
8. Existing Referral Search Validation
Conceptual Request
Plain Text
Transaction ID: TXN-90002
Event Type: REFERRAL_SEARCH
Source: IntakeConnect
Target: ReferralHub
 
Incident ID: INC-40001
Search Reference: SEARCH-10001
Applicable Search Criteria: Synthetic Values
Conceptual Response
Plain Text
Transaction ID: TXN-90002
Processing Result: SUCCESS
Result Count: 1
 
Referral ID: REF-70001
Referral Status: OPEN
Assigned Examiner: Examiner-A
Vendor Reference: VREF-80000
Validation
Request and response transaction IDs correlate.
Result count matches returned records.
REF-70001 exists in ReferralHub.
Referral information belongs to INC-40001.
No unrelated referral is returned.
9. Intake-to-ClaimCore Mapping
Representative mapping:
IntakeConnect Field	ClaimCore Field	RuleIncident ID	Source Incident ID	Direct
Referral ID	Referral ID	Direct
Loss Reference	Loss Reference	Direct or documented transformation
Referral Status	Referral Status	Approved status mapping
Vendor Reference	External Reference	Direct
Examiner	Assigned Examiner	Approved identifier mapping
Validation method:
Plain Text
IntakeConnect Value
↓
Integration Value
↓
ClaimCore Stored Value
↓
ClaimCore UI Value
10. New Referral Request Validation
For referral creation/update processing, validate applicable:
Plain Text
Claim ID
Referral ID
Referral Type
Referral Status
Referral Date
Assigned Examiner
Vendor Reference
Created Date
Updated Date
Expected:
Claim exists.
Referral identifier is unique for a new referral.
Mandatory values are populated.
Values map to the correct ClaimCore fields.
Referral is associated with the correct claim.
11. Examiner Update Validation
Business Event
Plain Text
ClaimCore Examiner:
Examiner-A → Examiner-B
Conceptual Integration Message
Plain Text
Transaction ID: TXN-90003
Event Type: EXAMINER_UPDATED
Source: ClaimCore
Target: ReferralHub
 
Claim ID: CLM-50001
Referral ID: REF-70001
Previous Examiner: Examiner-A
Updated Examiner: Examiner-B
Validation
Event type represents examiner update.
Claim ID is CLM-50001.
Referral ID is REF-70001.
Updated examiner is Examiner-B.
ReferralHub locates the existing referral.
Existing referral is updated.
Referral count remains one.
ReferralHub displays Examiner-B.
Expected reconciliation:
Plain Text
ClaimCore Examiner
=
Integration Examiner
=
ReferralHub Examiner
12. Referral Status Update Validation
Business Event
Plain Text
ReferralHub Status:
OPEN → IN_PROGRESS
Conceptual Integration Message
Plain Text
Transaction ID: TXN-90004
Event Type: REFERRAL_STATUS_UPDATED
Source: ReferralHub
Target: ClaimCore
 
Claim ID: CLM-50001
Referral ID: REF-70001
Previous Status: OPEN
Updated Status: IN_PROGRESS
Validation
Event type represents status update.
Claim ID is correct.
Referral ID is correct.
Updated status is supported.
ClaimCore locates the existing referral.
Existing referral is updated.
Referral count remains one.
ClaimCore UI displays IN_PROGRESS.
Expected reconciliation:
Plain Text
ReferralHub Status
=
Integration Status
=
ClaimCore Stored Status
=
ClaimCore UI Status
13. Response and Acknowledgement Validation
Where the interface provides a response or acknowledgement, validate:
Plain Text
Transaction ID
Correlation ID
Processing result
Target record identifier
Error code, when applicable
Error description, when applicable
Processing timestamp
A success acknowledgement should correspond to successful target-system processing.
If acknowledgement says success but the target data is missing or incorrect, the business test should fail.
14. Technical Result vs. Business Result
Example:
Plain Text
Technical Result:
Message accepted
 
Business Result:
Referral not displayed in ClaimCore
Overall result:
Plain Text
FAIL
Another example:
Plain Text
Technical Result:
Message accepted
 
Business Result:
Referral updated correctly in ClaimCore and UI
Overall result:
Plain Text
PASS
15. Negative Request Validation
Test variations should include:
Missing Required Field
Example:
Plain Text
Referral ID = NULL
Expected:
Message is not processed as a valid referral update.
Another referral is not updated.
Invalid Claim Identifier
Example:
Plain Text
Claim ID = CLM-99999
Expected:
Integration does not update an unrelated claim.
Appropriate error information is available.
Invalid Referral Identifier
Example:
Plain Text
Referral ID = REF-99999
Expected:
Existing referral is not incorrectly updated.
Unsupported Status
Example:
Plain Text
Status = UNKNOWN_STATUS
Expected:
Valid target status is not overwritten.
Invalid Examiner
Example:
Plain Text
Examiner = INVALID-USER
Expected:
Invalid value follows applicable error processing.
Existing valid examiner remains intact.
Overlength Value
Expected:
Value follows the interface validation rule.
Data is not silently truncated unless explicitly required by the approved interface contract.
16. Null Handling
Classify each field as:
Mandatory
Optional
Conditionally required
Validate:
Condition	Expected BehaviorMandatory field populated	Process normally
Mandatory field null	Reject or fail validation
Optional field null	Process according to contract
Conditionally required field absent	Validate business condition
Never assume every null value should be rejected.
17. Date and Timestamp Validation
Where applicable, validate:
Accepted format
Time zone
Date-only vs. date-time behavior
Source/target consistency
Processing timestamp
Created vs. updated timestamp
Future/past-date business rules
Example fictional date:
Plain Text
2026-10-15
The portfolio should not claim a specific production format unless confirmed by the interface contract.
18. Duplicate and Idempotency Validation
Reprocess the same logical referral transaction.
Before:
Plain Text
REF-70002 Count = 1
After:
Plain Text
REF-70002 Count = 1
Validate:
Existing referral is recognized.
Duplicate referral is not created.
Repeated update does not corrupt data.
Retry result is traceable.
Target business values remain correct.
19. Error Response Validation
For failed processing, verify applicable:
Plain Text
Transaction ID
Error category
Error code
Error description
Failed field
Processing stage
Retry eligibility
Timestamp
Good error information should support troubleshooting without exposing sensitive system details.
20. Target-System Validation
After successful integration, validate the target application.
ReferralHub → ClaimCore
Verify:
Correct claim
Correct referral
Correct status
Correct examiner
UI display
Persistence after reopening
ClaimCore → ReferralHub
Verify:
Correct existing referral
Correct updated examiner
No duplicate
Persistence after reopening
21. UI Reconciliation
A target UI field should be compared with the integration value.
Integration Field	ClaimCore UI FieldReferral ID	Referral ID
Referral Type	Referral Type
Referral Status	Status
Assigned Examiner	Examiner
Vendor Reference	External Reference
Expected:
Plain Text
Integration Value = UI Value
Document approved label/format transformations separately.
22. Postman or Bruno Validation Approach
When a callable QA interface is available, Postman or Bruno may be used to validate:
Authentication configuration
Headers or metadata
Request body
Response body
Technical status/result
Response time
Correlation identifier
Positive requests
Negative requests
Environment variables
Do not store real secrets in exported collections.
Use placeholders such as:
Plain Text
{{base_url}}
{{test_claim_id}}
{{test_referral_id}}
{{auth_placeholder}}
23. Evidence Template
For each transaction, capture only sanitized evidence:
Plain Text
Test Case ID:
Transaction ID:
Source Application:
Target Application:
Business Event:
Synthetic Claim ID:
Synthetic Referral ID:
Expected Result:
Actual Result:
Technical Result:
Business Result:
UI Validation:
Status:
Defect ID:
Before publishing evidence, remove:
Real URLs
Real headers
Tokens
Credentials
Cookies
Certificates
Internal hostnames
Real claim/referral numbers
Personal information
Vendor-specific secrets
24. Traceability
Example referral creation:
Plain Text
RC-REQ-045
Referral Integration Trigger
↓
RC-TS-017
Send New Referral to ClaimCore
↓
RC-TC-012
Send New Referral to ClaimCore
↓
Request / Response Validation
↓
ClaimCore Data Validation
↓
ClaimCore UI Validation
Example examiner update:
Plain Text
RC-REQ-063
Examiner Data Consistency
↓
RC-TS-023
Update ReferralHub Examiner
↓
RC-TC-015
Synchronize Updated Examiner
↓
ClaimCore → Integration → ReferralHub
Example status update:
Plain Text
RC-REQ-072
Status Consistency
↓
RC-TS-027
Update ClaimCore Referral Status
↓
RC-TC-018
Synchronize Referral Status
↓
ReferralHub → Integration → ClaimCore → UI
25. Portfolio Disclaimer
All request and response structures, event names, transaction identifiers, fields, claims, incidents, referrals, examiner values, statuses, application names, mappings, and validation examples are fictional or generalized.
The document demonstrates API and integration-testing techniques without reproducing any internal interface contract, real payload, endpoint, authentication mechanism, credential, token, certificate, vendor secret, or proprietary business data.
