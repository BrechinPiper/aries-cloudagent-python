## This page contains the code used in April 2024 for Brechin Pipers SSI experiments

The code snippets below may be directly copied and used in the openAPI interface accessbile via browser, with several small changes. The DID field in schema, credential, and issuer fields must be updated to match the DIDs assigned to your agents, ex: RKGBoS1ukjFAfmkG5R1jVd:2:Medical License Schema:1.0 must have the RKGBoS1ukjFAfmkG5R1jVd replaced to match your environment. 

## Step 1: 
Start the local VON testnet, or use the public VON testnat. Start the 4 agents, Alison, DrB, DB, and HR. If using a local VON instance, no url is required at startup. Confirm that the agents are all running by visiting localhost:8021,8031,8041,8051 for HR, DrB, HR, and DB agents. If using local VON instance as per the guide, the web server is accessible at localhost:9000 

## Step 2:
Establish connections between the DrB agent and Alison, DB, and HR. Using the DID connection method is faster, but not required. 

## Step 3:
Using the HR agent at localhost:8041, define several credential schemas 

One for a medical license:
```bash
{
  "attributes": [
"date issued",
"expiration date",
"license holder",
"license type"
  ],
  "schema_name": "Medical License Schema",
  "schema_version": "1.0"
}
```
and one for a consent schema
```bash
{
  "attributes": [
"date issued",
"expiration date",
"conditions"
  ],
  "schema_name": "Patient Consent Schema",
  "schema_version": "1.0"
}
```
Both schemas will be visible on the VON testnet if successful. 

## Step 4:
To issue a credential, a credential definiton must first be creating. Still using the HR agent, issue a cred def:
```bash
{
  "schema_id": "RKGBoS1ukjFAfmkG5R1jVd:2:Medical License Schema:1.0",
  "tag": "Medical License Definition"
}
```
## Step 5: 
Now issue two credentials from the HR agent to the DrB agent. 

The first for family medicine: 
```bash
{
  "auto_remove": false,
  "comment": "Family Medicine License",
  "connection_id": "858f0887-6b63-4a08-9349-45e8dc0120bb",
  "credential_preview": {
    "@type": "issue-credential/2.0/credential-preview",
    "attributes": [
      {
        "name": "date issued",
        "value": "20240318"
      },
      {
        "name": "expiration date",
        "value": "20340318"
      },
{
        "name": "license holder",
        "value": "Bob Smith"
      },
{
        "name": "license type",
        "value": "family medicine"
      }
    ]
  },
  "filter": {
    "indy": {
      "cred_def_id": "RKGBoS1ukjFAfmkG5R1jVd:3:CL:56:Medical License Definition",
      "issuer_did": "RKGBoS1ukjFAfmkG5R1jVd",
      "schema_id": "RKGBoS1ukjFAfmkG5R1jVd:2:Medical License Schema:1.0",
      "schema_issuer_did": "RKGBoS1ukjFAfmkG5R1jVd",
      "schema_name": "Medical License Schema",
      "schema_version": "1.0"
    }
},
  "trace": false,
  "verification_method": "string"
}
```
The second for emergency medicine:
```bash
{
  "auto_remove": false,
  "comment": "Emergency Medicine License",
  "connection_id": "858f0887-6b63-4a08-9349-45e8dc0120bb",
  "credential_preview": {
    "@type": "issue-credential/2.0/credential-preview",
    "attributes": [
      {
        "name": "date issued",
        "value": "20240318"
      },
      {
        "name": "expiration date",
        "value": "20250318"
      },
{
        "name": "license holder",
        "value": "Bob Smith"
      },
{
        "name": "license type",
        "value": "emergency medicine"
      }
    ]
  },
  "filter": {
    "indy": {
      "cred_def_id": "RKGBoS1ukjFAfmkG5R1jVd:3:CL:56:Medical License Definition",
      "issuer_did": "RKGBoS1ukjFAfmkG5R1jVd",
      "schema_id": "RKGBoS1ukjFAfmkG5R1jVd:2:Medical License Schema:1.0",
      "schema_issuer_did": "RKGBoS1ukjFAfmkG5R1jVd",
      "schema_name": "Medical License Schema",
      "schema_version": "1.0"
    }
},
  "trace": false,
  "verification_method": "string"
}
```




