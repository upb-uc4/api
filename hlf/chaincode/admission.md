# Hyperledger Admission Api

In the following, the chaincode api for the "Admission"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## ContractName "UC4.Admission"

## Transactions

### Add Admission
- ID = addAdmission
- Send
    - jsonAdmission :: [Admission](#Admission)
- Receive
    - [Admission](#Admission)
      -  Description: Done, returns the submitted data, decorated with an admissionId.

    - [DetailedError](errors.md#DetailedError) 
      ```json
      {
        "type": "HLUnprocessableEntity",
        "title": "The following parameters do not conform to the specified format",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
       - Description: This error is returned, if the given parameters could not be parsed. If some attributes within the json are not well formatted, they are listed in invalidParams.  
       For detailed informations see [parameter checks](#parameterChecks).

    - [DetailedError](errors.md#DetailedError) 
      ```json
      {
        "type": "HLInvalidEntity",
        "title": "The following parameters produced some semantic error",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
      - Description: This error is returned, if the given parameters produce some error.  
      For detailed informations see [semantic checks](#semanticChecks).
    
    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLInsufficientApprovals",
        "title": "The approvals present on the ledger do not suffice to execute this transaction"
      }
      ```
      - Description: This error is returned, if the required approvals are not present.


### Drop Admission
- ID = dropAdmission
- Send
    - admissionId :: String
- Receive
    - ""
      -  Description: Done, returns the submitted data.

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no Admission for the given admissionId"
      }
      ```
      - Description: This error is returned, if the admissionId is not present on the ledger.

    - [DetailedError](errors.md#DetailedError) 
      ```json
      {
        "type": "HLUnprocessableEntity",
        "title": "The following parameters do not conform to the specified format",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams.  
      For detailed informations see [parameter checks](#parameterChecks).
      (admissionId)

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLInsufficientApprovals",
        "title": "The approvals present on the ledger do not suffice to execute this transaction"
      }
      ```
      - Description: This error is returned, if the required approvals are not present.


### Get Admissions
- ID = getAdmissions
- Send
    - enrollmentId :: String
    - courseId :: String
    - moduleId :: String
- Receive
    - admissionList :: List\<[Admission](#Admission)\>
      - Description: Exhaustive List of all Admissions, filtered by
      inputs "enrollmentId", "courseId", "moduleId".
      Empty if no match could be found.

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.

    - [DetailedError](errors.md#DetailedError) 
      ```json
      {
        "type": "HLUnprocessableEntity",
        "title": "The following parameters do not conform to the specified format",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams.  
      For detailed informations see [parameter checks](#parameterChecks).

## <a id="Models" />Models

### <a id="Admission" />Admission
```json
{
  "admissionId": "0123456:ExampleCourse",
  "enrollmentId": "0123456",
  "courseId": "ExampleCourse",
  "moduleId": "M.1",
  "timestamp": "2004-06-14T23:34:30" \<DATE ISO 8601 YYYY-MM-DDThh:mm:ss\>
}
```

## <a id="Checks" />Input Checks
### <a id="parameterChecks" />Parameters
- Checks, if parseable Json.
- **admissionId**
  - Check, if not null or an empty String.
- **enrollmentId**
  - Check, if not null or an empty String.
- **courseId**
  - Check, if not null or an empty String.
- **moduleId**
  - Check, if not null or an empty String.
- **timestamp**
  - Check, if valid date-String YYYY-MM-DDThh:mm:ss

### <a id="semanticChecks" />SemanticChecks
- **admissionPossible**
  - Check, if **enrollmentId** is matriculated in an examinationRegulation containing the **moduleId**.
