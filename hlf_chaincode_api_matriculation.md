# Hyperledger Matriculation Api

In the following, the chaincode api for the "Matriculation"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](hlf_chaincode_api_errors.md#Errors).

## Transactions

### Add Matriculation Data
- ID = addMatriculationData
- Send
    - newMatriculationData :: [MatriculationData](#MatriculationData)
- Receive
    - [MatriculationData](#MatriculationData)
      -  Description: Done, returns the submitted data.

    - [DetailedError](hlf_chaincode_api_errors.md#DetailedError) 
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


    - [GenericError](hlf_chaincode_api_errors.md#GenericError) 
      ```json
      {
        "type": "HLConflict",
        "title": "There is already a MatriculationData for the given enrollmentId",
      }
      ```
    - Description: This error is returned, if a matriculation data with the given enrollmentId is already present on the ledger.

### Update Matriculation Data
- ID = updateMatriculationData
- Send
    - updatedMatriculationData :: [MatriculationData](#MatriculationData)
- Receive
    - [MatriculationData](#MatriculationData)
      -  Description: Done, returns the submitted data.

    - [GenericError](hlf_chaincode_api_errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no MatriculationData for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId specified in the MatriculationData is not present on the ledger.

    - [DetailedError](hlf_chaincode_api_errors.md#DetailedError) 
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

### Get Matriculation Data
- ID = getMatriculationData
- Send
    - enrollmentId :: String
- Receive
    - [MatriculationData](#MatriculationData)

    - [GenericError](hlf_chaincode_api_errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no MatriculationData for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId is not present on the ledger.

    - [GenericError](hlf_chaincode_api_errors.md#GenericError) 
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

    - [GenericError](hlf_chaincode_api_errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no student for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId is not present on the ledger.
      
    - [DetailedError](hlf_chaincode_api_errors.md#DetailedError) 
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
    
    - [GenericError](hlf_chaincode_api_errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.

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