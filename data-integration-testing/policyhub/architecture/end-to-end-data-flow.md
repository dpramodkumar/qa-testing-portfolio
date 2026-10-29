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
