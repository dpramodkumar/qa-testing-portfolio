PolicyHub – Multi-System Policy Data Integration Testing

📌 Project Overview
PolicyHub is a fictional multi-system insurance data integration project created to demonstrate my experience with ETL, database, integration, functional, and end-to-end software testing.

The project demonstrates how policy information is received from an external source, processed through Informatica PowerCenter, validated and transformed in Oracle, stored in a centralized policy repository, and consumed by downstream applications through policy-search services.

Testing also covers the complete business lifecycle from incident creation through claim creation, financial processing, and claim closure.

🎯 Testing Objective

The primary objective of this project is to demonstrate validation of data and business processes across multiple integrated systems.

The testing approach verifies that:

Source policy data is received correctly.
Informatica workflows process the source data successfully.
Data transformations are accurate.
Valid policy information reaches the target repository.
Invalid records are handled appropriately.
Policy Search Service requests and responses are accurate.
Incident information flows correctly between applications.
Policy information is revalidated during claim processing.
An incident can be converted successfully into a claim.
Financial transactions can be completed.
The complete claim lifecycle operates successfully.

🏗️ Fictional System Architecture

Phase 1 – Policy Data Integration
External Policy Provider ↓ Policy Feed ↓ Secure File Gateway ↓ Informatica PowerCenter ↓ Oracle Landing Tables ↓ Data Transformation ↓ Staging Tables ↓ Validation ↙ ↘ Invalid Valid ↓ ↓ Error Log Integration Tables ↓ Insert / Update ↓ Policy Repository

Phase 2 – Policy Service Integration
IncidentHub ↓ Policy Search Request ↓ Policy Search Service ↓ PolicyHub Repository ↓ Policy Search Response ↓ Policy / Risk Unit Selection ↓ Incident Processing

Phase 3 – Claims Integration
IncidentHub ↓ ClaimCore ↓ Policy Revalidation ↓ Policy Search Service ↓ PolicyHub Repository ↓ Policy Details Returned ↓ Incident Converted to Claim

Phase 4 – Complete Business Lifecycle
Policy Feed ↓ Informatica Processing ↓ Oracle Validation ↓ PolicyHub Repository ↓ Incident Creation ↓ Policy Search ↓ Policy Selection ↓ Incident Sent to ClaimCore ↓ Policy Revalidation ↓ Incident Converted to Claim ↓ Reserve Creation ↓ Payment Processing ↓ Claim Closure

🧪 Testing Areas

ETL Testing
Source file validation
Informatica workflow validation
Record-count reconciliation
Source-to-target validation
Transformation-rule validation
Required-field validation
Duplicate-record validation
Insert and update processing
Reject and error processing
Database Testing
Oracle database validation
SQL query validation
Landing-table validation
Staging-table validation
Target-table validation
Source-to-target reconciliation
Data integrity validation
Integration Testing
Application-to-service integration
Policy Search Service validation
Request and response validation
Cross-application data validation
Policy and risk-unit validation
Error and exception handling
Functional Testing
Incident creation
Policy search
Policy selection
Policy revalidation
Incident-to-claim conversion
Reserve creation
Payment processing
Claim closure
End-to-End Testing
The end-to-end test validates the complete lifecycle:

Policy Feed ↓ ETL ↓ Policy Repository ↓ Incident ↓ Policy Search ↓ Claims Application ↓ Claim ↓ Reserve ↓ Payment ↓ Claim Closure

The objective is to verify that the same fictional policy information remains accurate and available throughout the complete business workflow.

🛠️ Tools & Technologies

Informatica PowerCenter
Oracle Database
Toad for Oracle
SQL
XML
File-Based Data Integration
Service / API Testing
Database Validation
ETL Testing
Manual Functional Testing
Integration Testing
End-to-End Testing

📂 Project Artifacts

This portfolio project will include:

Sanitized Business Requirements
System Architecture
End-to-End Data Flow
Source-to-Target Mapping
QA Test Plan
ETL Test Scenarios
Detailed Test Cases
SQL Validation Examples
Service Integration Test Cases
End-to-End Lifecycle Testing
Test Results
Sample Defects
Requirements Traceability Matrix

🔒 Portfolio Disclaimer

PolicyHub is a fictional project created solely for portfolio and learning purposes.

Application names, interfaces, databases, schemas, tables, fields, endpoints, file layouts, policy information, claims, users, financial information, business rules, workflows, test data, and system configurations presented in this repository are fictional or generalized.

No confidential employer, client, claimant, production, financial, or proprietary information is included.
