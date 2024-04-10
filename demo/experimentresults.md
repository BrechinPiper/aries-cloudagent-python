## This page contains the code used in April 2024 for Brechin Pipers SSI experiments

The code snippets below may be directly copied and used in the openAPI interface accessbile via browser, with several small changes. The DID field in schema, credential, and issuer fields must be updated to match the DIDs assigned to your agents, ex: RKGBoS1ukjFAfmkG5R1jVd:2:Medical License Schema:1.0 must have the RKGBoS1ukjFAfmkG5R1jVd replaced to match your environment. 

Whenever possible, ensure that autoaccept is set to true. For example, when issuing credentials. This will save several steps of issuing and accepting credentials. 

## Step 1: 
Start the local VON testnet, or use the public VON testnet. Start the 4 agents, Alison, DrB, DB, and HR. If using a local VON instance, no url is required at startup. Confirm that the agents are all running by visiting localhost:8021,8031,8041,8051 for HR, DrB, Alison, and DB agents. If using local VON instance as per the guide, the web server is accessible at localhost:9000 

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
To confirm that the DrB agent has received both credentials you can check the CLI, or switch to the DrB agent in a browser and query held credentials. 

## Step 6
Switch to the Alison agent at localhost:8041. 

If Alsion does not have a public DID registered already, register one on the VON testnet. Alison may query the DrB agent for proof of a medical license, which is a natural step when selecting a doctor. Once satisfied with the Drs credentials, we will issue a consent credential. To do this, Alison must first define this credential. Note that the schema ID is still that of the schema creator, the DID must match that of the HR. 

Credential definition for Alison' informed patient consent.
```bash
{
  "schema_id": "RKGBoS1ukjFAfmkG5R1jVd:2:Patient Consent Schema:1.0",
  "tag": "Alice Consent Definition"
}
```
Now that a credential definition has been registered on the VON testnet, a credential can be issued to the DrB agent. 
```bash
{
  "auto_remove": false,
  "comment": "Patient Consent",
  "connection_id": "6345c122-3920-4fb6-a0b2-a8052805e155",
  "credential_preview": {
    "@type": "issue-credential/2.0/credential-preview",
    "attributes": [
      {
        "name": "date issued",
        "value": "20240318"
      },
      {
        "name": "expiration date",
        "value": "20290318"
      },
{
        "name": "conditions",
        "value": "none"
      }
    ]
  },
  "filter": {
    "indy": {
      "cred_def_id": "82kCCW32YHsDs8tCdeec4z:3:CL:57:Alice Patient Consent",
      "issuer_did": "82kCCW32YHsDs8tCdeec4z",
      "schema_id": "RKGBoS1ukjFAfmkG5R1jVd:2:Patient Consent Schema:1.0",
      "schema_issuer_did": "RKGBoS1ukjFAfmkG5R1jVd",
      "schema_name": "Patient Consent Schema",
      "schema_version": "1.0"
    }
},
  "trace": false,
  "verification_method": "string"
}
```

## Step 7
The DrB agent should now hold 3 credentials, 2 issued from the HR for medical licenses, 1 from the patient Alison for a consent form. We will now test several proofs from the database agent (DB) at localhost:8051. 

To ensure that the Drs emergency medicine license is accepted. Note that only a emergency medical license issued by a known health regulator should be accepted, this is enforced using the DID of the issuer. 
```bash
{
  "comment": "Emergency Access Request",
  "connection_id": "127af9c7-dac3-4630-9af5-d3a7bde33048",
  "presentation_request": {
    "indy": {
      "name": "Valid Emergency License Proof",
      "version": "1.0",
      "requested_attributes": {
        "0_name_uuid": {
          "name": "license holder",
          "restrictions": [
            {
              "cred_def_id": "RKGBoS1ukjFAfmkG5R1jVd:3:CL:56:Medical License Definition"
            }
          ]
        }
      },

      "requested_predicates": {
        "0_expiration_uuid": {
          "name": "expiration date",
          "p_type": ">=",
          "p_value": 20240318,
          "restrictions": [
            {
              "cred_def_id": "RKGBoS1ukjFAfmkG5R1jVd:3:CL:56:Medical License Definition"
            }
          ]
        },

        "0_licensetype_uuid": {
          "name": "license type",
          "p_type": "=",
          "p_value": "emergency medicine",
          "restrictions": [
            {
              "cred_def_id": "RKGBoS1ukjFAfmkG5R1jVd:3:CL:56:Medical License Definition"
            }
          ]
        }
      }
    }
  }
}
```
The routine access proof combines a medical license from a known regulator, with that of a known patient. The restrictions field specifies the DID of the HR and Alison. The business logic of a database could be configured multiple ways, in this instance allowing the Dr full read/write to health records associated only with Alison. 
```bash
{
  "comment": "Routine Access Request",
  "connection_id": "127af9c7-dac3-4630-9af5-d3a7bde33048",
  "presentation_request": {
    "indy": {
      "name": "Patient Consent Proof",
      "version": "1.0",
      "requested_attributes": {
        "0_name_uuid": {
          "name": "license holder",
          "restrictions": [
            {
              "cred_def_id": "RKGBoS1ukjFAfmkG5R1jVd:3:CL:56:Medical License Definition"
            }
          ]
        }
      },

      "requested_predicates": {
        "0_license_expiration_uuid": {
          "name": "expiration date",
          "p_type": ">=",
          "p_value": 20240318,
          "restrictions": [
            {
              "cred_def_id": "RKGBoS1ukjFAfmkG5R1jVd:3:CL:56:Medical License Definition"
            }
          ]
        },

        "0_consent_expiration_uuid": {
          "name": "expiration date",
          "p_type": ">=",
          "p_value": "20240318",
          "restrictions": [
            {
              "cred_def_id": "82kCCW32YHsDs8tCdeec4z:3:CL:57:Alice Patient Consent"
            }
          ]
        }
      }
    }
  }
}
```

