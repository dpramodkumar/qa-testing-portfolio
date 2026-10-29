PolicyHub End-to-End System Architecture
Overview
PolicyHub is a fictional multi-system insurance data integration solution used to demonstrate ETL, database, service integration, functional, and end-to-end testing.

The architecture contains multiple interconnected processing layers.

The complete business flow begins with policy data ingestion and continues through incident creation, policy search, claims processing, financial transactions, and claim closure.

System Components
External Policy Provider
Provides policy information through a structured policy feed.

Secure File Gateway
Receives the policy feed and makes the file available for ETL processing.

Informatica PowerCenter
Processes the incoming policy feed and manages the ETL workflow.

Major responsibilities include:

Reading source policy data
Loading landing data
Applying transformation rules
Processing valid records
Managing rejected records
Loading downstream staging data
Oracle Database
Stores policy information throughout different stages of the data-processing workflow.

The fictional database layers include:

Landing Tables
Transformation / Staging Tables
Integration Tables
Policy Repository Tables
Error / Reject Tables
PolicyHub Repository
Acts as the centralized fictional repository containing validated policy information.

Policy information stored in the repository can be searched and consumed by downstream applications.

Policy Search Service
Provides an integration layer between downstream applications and the PolicyHub Repository.

The service supports:

Policy search requests
Policy search responses
Policy detail requests
Policy detail responses
Risk-unit information
Error responses
IncidentHub
Represents the fictional incident-intake application.

IncidentHub allows a user to:

Create an incident
Enter loss information
Search for policy information
Select the applicable policy
Select the applicable risk unit
Submit the incident for downstream claims processing
ClaimCore
Represents the fictional claims-processing application.

ClaimCore receives incident information and supports:

Policy revalidation
Incident-to-claim conversion
Claim creation
Reserve creation
Payment processing
Claim financial validation
Claim closure

Phase 1 – Policy Data Ingestion
The first phase validates the movement of policy data from the external source into the PolicyHub Repository.

External Policy Provider
          |
          v
     Policy Feed
          |
          v
 Secure File Gateway
          |
          v
 Informatica PowerCenter
          |
          v
 Oracle Landing Tables
          |
          v
 Data Transformation
          |
          v
   Staging Tables
          |
          v
   Data Validation
       /       \
      /         \
 Invalid        Valid
    |             |
    v             v
Error / Reject  Integration Tables
    Tables          |
                    v
             Insert / Update
                    |
                    v
             PolicyHub Repository


          
Phase 1 Validation Points

Testing will validate:
Policy feed arrival
File format and structure
Required source fields
Informatica workflow execution
Source record counts
Landing-table record counts
Data transformations
Staging-table data
Required-field validation
Invalid/rejected records
Integration-table data
Insert processing
Update processing
Duplicate handling
Final repository data
Source-to-target reconciliation

Phase 2 – Incident and Policy Search Integration
After policy information is available in PolicyHub, the policy can be consumed by the fictional incident-intake application.

 IncidentHub
                 |
                 v
          Create Incident
                 |
                 v
       Enter Loss Information
                 |
                 v
       Policy Search Request
                 |
                 v
       Policy Search Service
                 |
                 v
       PolicyHub Repository
                 |
                 v
      Policy Search Response
                 |
                 v
         Policy Selection
                 |
                 v
        Risk Unit Selection
                 |
                 v
       Complete Incident

       Phase 2 Validation Points
Testing will validate:
Incident creation
Search-request generation
Required request fields
Policy Search Service processing
Repository search results
Response structure
Policy information
Effective-date validation
Risk-unit information
Policy selection
Data consistency between PolicyHub and IncidentHub
No-match behavior
Invalid-request behavior
Service-error handling

Phase 3 – Claims Integration
After the incident is completed, the incident information is sent to ClaimCore.
ClaimCore performs another policy validation before the incident is converted into a claim.

IncidentHub
|
v
Completed Incident
|
v
ClaimCore
|
v
Receive Incident
|
v
Revalidate Policy
|
v
Policy Search Request
|
v
Policy Search Service
|
v
PolicyHub Repository
|
v
Policy Details Response
|
v
Validate Policy Data
|
v
Convert Incident to Claim
|
v
Claim Created
Phase 3 Validation Points
Testing will validate:
Successful incident transmission
Incident data received by ClaimCore
Policy number
Loss date
Policy-search request
Policy-search response
Policy details
Risk-unit information
Coverage information
Cross-system field mapping
Incident-to-claim conversion
Claim-number creation
Policy association with the new claim
Data consistency after conversion

Phase 4 – Claim Financial Lifecycle
After a claim is created, the financial lifecycle is validated.

 Claim Created
                |
                v
         Review Coverage
                |
                v
         Create Reserve
                |
                v
        Validate Reserve
                |
                v
         Create Payment
                |
                v
        Process Payment
                |
                v
    Validate Financial History
                |
                v
        Complete Claim Work
                |
                v
          Close Claim

Phase 4 Validation Points
Testing will validate:
Claim creation
Policy information
Applicable coverage
Reserve creation
Reserve amount
Reserve persistence
Payment creation
Payment processing
Financial history
Claim activity/history
Required closure conditions
Claim closure
Closed-claim status
Preservation of policy and financial history

Complete End-to-End Lifecycle
The primary end-to-end workflow connects all four phases.

External Policy Provider
          |
          v
      Policy Feed
          |
          v
 Informatica PowerCenter
          |
          v
    Oracle Database
          |
          v
  PolicyHub Repository
          |
          v
      IncidentHub
          |
          v
     Create Incident
          |
          v
 Policy Search Service
          |
          v
  PolicyHub Repository
          |
          v
      Policy Match
          |
          v
    Complete Incident
          |
          v
       ClaimCore
          |
          v
   Policy Revalidation
          |
          v
 Policy Search Service
          |
          v
  PolicyHub Repository
          |
          v
 Incident Converted to Claim
          |
          v
    Reserve Created
          |
          v
    Payment Processed
          |
          v
      Claim Closed

      End-to-End Data Validation Strategy
A key testing objective is to verify that the same fictional policy can be traced throughout the complete workflow.

Source Policy Feed
        |
        v
Oracle Landing Layer
        |
        v
Oracle Staging Layer
        |
        v
PolicyHub Repository
        |
        v
Policy Search Response to IncidentHub
        |
        v
Selected Incident Policy
        |
        v
Policy Revalidation in ClaimCore
        |
        v
Claim Policy Information
        |
        v
Closed Claim

Error Processing Architecture
The architecture also includes negative and exception processing.

Policy Feed
     |
     v
Informatica Processing
     |
     +------ Invalid Record ------> Error / Reject Processing
     |
     v
Valid Record
     |
     v
Policy Repository

IncidentHub / ClaimCore
          |
          v
Policy Search Service
          |
     +----+----+
     |         |
   Success    Failure
     |         |
     v         v
Policy Data   Error Response


Testing will verify that invalid data or failed integrations do not create incomplete or inconsistent downstream records.
Testing Across Integration Points
The architecture contains several important QA integration points.
Integration Point 1
Plain Text
Policy Feed → Informatica
Validate source file ingestion.
Integration Point 2
Plain Text
Informatica → Oracle
Validate ETL loading and transformations.
Integration Point 3
Plain Text
Oracle → PolicyHub Repository
Validate final policy persistence.
Integration Point 4
Plain Text
IncidentHub → Policy Search Service
Validate policy-search requests.
Integration Point 5
Plain Text
Policy Search Service → PolicyHub
 
Validate repository lookup.
Integration Point 6
Plain Text
PolicyHub → IncidentHub
Validate search responses and policy selection.
Integration Point 7
Plain Text
IncidentHub → ClaimCore
Validate incident integration.
Integration Point 8
Plain Text
ClaimCore → Policy Search Service → PolicyHub
Validate downstream policy revalidation.
Integration Point 9
Plain Text
ClaimCore → Claim Financial Processing
Validate reserve and payment processing.
Integration Point 10
Plain Text
Claim Financial Processing → Claim Closure
Validate completion of the claim lifecycle.
QA Perspective
The architecture is tested at multiple levels:
File level
ETL workflow level
Database level
Transformation level
Service/API level
Application level
Integration level
Financial transaction level
End-to-end business-process level
This approach provides validation of both individual components and the complete business lifecycle.
Portfolio Disclaimer
PolicyHub, IncidentHub, ClaimCore, and Policy Search Service are fictional names created for portfolio demonstration.
Application names, architecture, database structures, schemas, tables, fields, interfaces, endpoints, policy information, claim information, financial values, business rules, and test data presented in this document are fictional or generalized.
The architecture is intended to demonstrate software testing concepts involving Informatica PowerCenter, Oracle Database, ETL processing, service integration, database validation, functional testing, and end-to-end testing.
No confidential employer, client, claimant, production, financial, or proprietary system information is included.
