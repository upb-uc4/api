# Hyperledger Matriculation Api

In the following, the chaincode api for the "Matriculation"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## Transactions

### Add Matriculation Data
- ID = addMatriculationData
- Send
    - matriculationData :: [MatriculationData](#MatriculationData)
- Receive
    - [MatriculationData](#MatriculationData)
      -  Description: Done, returns the submitted data.

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
       For detailed informations see [matriculationData checks](#matriculationDataChecks).

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLConflict",
        "title": "There is already a MatriculationData for the given enrollmentId",
      }
      ```
      - Description: This error is returned, if a matriculation data with the given enrollmentId is already present on the ledger.

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLInsufficientApprovals",
        "title": "The approvals present on the ledger do not suffice to execute this transaction"
      }
      ```
      - Description: This error is returned, if the required approvals are not present.




### Update Matriculation Data
- ID = updateMatriculationData
- Send
    - matriculationData :: [MatriculationData](#MatriculationData)
- Receive
    - [MatriculationData](#MatriculationData)
      -  Description: Done, returns the submitted data.

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no MatriculationData for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId specified in the MatriculationData is not present on the ledger.

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
      For detailed informations see [matriculationData checks](#matriculationDataChecks).


### Get Matriculation Data
- ID = getMatriculationData
- Send
    - enrollmentId :: String
- Receive
    - [MatriculationData](#MatriculationData)

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no MatriculationData for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId is not present on the ledger.

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.

### Add Semester to Matriculation Data
This method adds a single entry to the list of semesters in the MatriculationData, to provide secure updates.

- ID = addEntriesToMatriculationData
- Send
    - enrollmentId :: String
    - matriculation :: List\<[SubjectMatriculation](#SubjectMatriculation)\>
- Receive
    - [MatriculationData](#MatriculationData)
      -  Description: Done, returns the submitted data.

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no student for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId is not present on the ledger.
      
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
      For detailed informations see [subjectMatriculation checks](#subjectMatriculationChecks).

    
    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.
    
    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLInsufficientApprovals",
        "title": "The approvals present on the ledger do not suffice to execute this transaction"
      }
      ```
      - Description: This error is returned, if the required approvals are not present.


## <a id="Models" />Models

### <a id="MatriculationData" />MatriculationData
```json
{
  "enrollmentId": "string",
  "matriculationStatus": [
    {
      "fieldOfStudy": "Computer Science",
      "semersters": ["WS2019/20", "SS2020"]
    },
    {
      "fieldOfStudy": "Philosophy",
      "semersters": ["WS2016/17", "SS2017", "WS2017/18"]
    }
  ]
}
```

### <a id="SubjectMatriculation" />SubjectMatriculation
```json
{
  "fieldOfStudy": "Computer Science",
  "semesters": ["WS2019/20", "SS2020"]
}
```

## <a id="Checks" />Input Checks
### <a id="matriculationDataChecks" />matriculationData
- Checks, if parseable Json.
- **enrollmentId**
  - Check, if not null or an empty String.
- **matriculationStatus**
  - see [subjectMatriculation](#subjectMatriculationChecks)


### <a id="subjectMatriculationChecks" />subjectMatriculation
- Check, if not null or an empty list.
- **fieldOfStudy**
  - Check, if not null or an empty String.
  - Check for duplicate entries.
- **semesters**
  - Check, if not null or an empty list.
  - Check, if containing strings are in the format (WS\d{4}/\d{2}|SS\d{4}), e.g. WS2020/21 or SS2020. Also, in case of winter semester, check, if the two included years are two consecutive years with the full displayed year being the chronologically earlier one.
  - Check for duplicate entries.
