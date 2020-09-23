# Hyperledger Matriculation Api

## Transactions

In the following, the chaincode api is described. It is identical to the hyperledger-lagom api, except for th following points:
- If the chaincode api returns "", meaning Done, to lagom Unit is returned
- Each of the specified error json objects is sent to lagom in an exception, with a string parameter containing the json string
- Lagom can receive an error of the form:
  ```json
  {
    "type": "HLUnknownTransactionId",
    "title": "The transaction is not defined"
  }
  ```
  - Description: This error is returned, if the transactionId does not exist.
  ```json
  {
    "type": "HLInvalidTransactionCall",
    "title": "The transaction was invoked with the wrong amount of parameters. Expected: ${expected} Actual: ${actual}",
    "transactionId": "addMatricuationData"
  }
  ```
  - Description: This error is returned, if the transactionId does not exist.

### Add Matriculation Data
- ID = addMatriculationData
- Send
    - newMatriculationData :: MatriculationData
- Receive
    - ""
      -  Description: Done
    - ```json
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

  - ```json
      {
        "type": "HLConflict",
        "title": "There is already a MatriculationData for the given enrollment.id",
        "invalidParams": []
      }
      ```
    - Description: This error is returned, if a matriculation data with the given enrollment.id is already present on the ledger.

### Update Matriculation Data
- ID = updateMatriculationData
- Send
    - newMatriculationData :: MatriculationData
- Receive
    - ""
      - Description: Done.

    - ```json
      {
        "type": "HLNotFound",
        "title": "There is no MatriculationData for the given enrollment.id"
      }
      ```
      - Description: This error is returned, if the enrollment.id specified in the MatriculationData is not present on the ledger.

    - ```json
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
    - enrollment.id :: String
- Receive
    - MatriculationData

    - ```json
      {
        "type": "HLNotFound",
        "title": "There is no MatriculationData for the given enrollment.id"
      }
      ```
      - Description: This error is returned, if the enrollment.id is not present on the ledger.
    - ```json
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
    - enrollment.id :: String
    - matriculation :: List\<SubjectMatriculation\>
- Receive
    - ""
      - Description: Done

    - ```json
      {
        "type": "HLNotFound",
        "title": "There is no student for the given enrollment.id"
      }
      ```
      - Description: This error is returned, if the enrollment.id is not present on the ledger.
    - ```json
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
    - ```json
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
  "enrollment.id": "string",
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

## <a id="Errors" />Errors

### <a id="GenericError" />GenericError
```json
{
  "type": "string",
  "title": "string"
}
```

### <a id="DetailedError" />DetailedError
```json
{
  "type": "string",
  "title": "string",
  "invalidParams": [
    {
      "name": "string",
      "reason": "string"
    }
  ]
}
```

### <a id="InvalidParameter" />InvalidParameter
```json
{
  "name": "string",
  "reason": "string"
}
```
name:
- name of the invalid parameter in simple dot- and array notation, i.e. if the second semester entry in the ```semesters``` field of the first ```SubjectMatriculation``` object in the ```matriculationStatus``` field of a ```MatriculationData``` object passed as a parameter named ```data``` is invalid, the ```name``` field of the ```InvalidParameter``` object would be ```"data.matriculationStatus[0].semesters[1]"```

reason:
- a textual representation of why the parameter is invalid
