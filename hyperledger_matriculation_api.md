# Hyperledger Matriculation Api

## Transactions

In the following, the chaincode api is described. It is identical to the hyperledger-lagom api, except for th following points:
- If the chaincode api returns "", meaning Done, to lagom Unit is returned
- Each of the specified error json objects is sent to lagom in an exception, with a string parameter containing the json string
- Lagom can receive an error of the form:
  ```json
  {
    "type": "hl: unknown transactionId",
    "title": "The transaction is not defined."
  }
  ```
  - Description: This error is returned, if the transactionId does not exist.
  ```json
  {
    "type": "hl: invalid transaction call",
    "title": "The transaction was invoked with the wrong amount of parameters. Expected: ${expected} Actual: ${actual}",
    "transactionId": "addMatricuationData"
  }
  ```
  - Description: This error is returned, if the transactionId does not exist.

### Add Matriculation Data
- ID = addMatriculationData
- Send
    - MatriculationData

    - ```json
      {
        "type": "hl: unprocessable entity",
        "title": "The given parameters do not conform to the specified format.",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
    
       - Description: This error is returned, if the given parameters could not be parsed. If the string could not be parsed as json, the list of invalidParams is empty. If some attributes within the json are not well formatted, they are listed in invalidParams.

  - ```json
      {
        "type": "hl: conflict",
        "title": "There is already a student for the given matriculationId.",
        "invalidParams": []
      }
      ```
    - Description: This error is returned, if a student with the given matriculationId is already present on the ledger.

### Update Matriculation Data
- ID = updateMatriculationData
- Send
    - MatriculationData
- Receive
    - ""
      - Description: Done.

    - ```json
      {
        "type": "hl: not found",
        "title": "There is no student for the given matriculationId."
      }
      ```
      - Description: This error is returned, if the matriculationID specified in the MatriculationData is not present on the ledger.

    - ```json
      {
        "type": "hl: unprocessable entity",
        "title": "The given parameters do not conform to the specified format.",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
      - Description: This error is returned, if the given parameters could not be parsed. If the string could not be parsed as json, the list of invalidParams is empty. If some attributes within the json are not well formatted, they are listed in invalidParams.

### Get Matriculation Data
- ID = getMatriculationData
- Send
    - matriculationId
- Receive
    - MatriculationData

    - ```json
      {
        "type": "hl: not found",
        "title": "There is no student for the given matriculationId."
      }
      ```
      - Description: This error is returned, if the matriculationID is not present on the ledger.

### Add Semester to Matriculation Data
  This method adds a single entry to the list of semesters in the MatriculationData, to provide secure updates.
- ID = addEntryToMatriculationData
- Send
    - matriculationId
    - fieldOfStudy
    - semester
- Receive
    - MatriculationData

    - ```json
      {
        "type": "hl: not found",
        "title": "There is no student for the given matriculationId."
      }
      ```
      - Description: This error is returned, if the matriculationID is not present on the ledger.
    - ```json
      {
        "type": "hl: unprocessable entity",
        "title": "The given parameters do not conform to the specified format.",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
      - Description: This error is returned, if the given parameters could not be parsed. If the string could not be parsed as json, the list of invalidParams is empty. If some attributes within the json are not well formatted, they are listed in invalidParams.


## Models

### MatriculationData
```json
{
  "matriculationId": "string",
  "firstName": "string",
  "lastName": "string",
  "birthDate": "2020-07-21",
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

### SubjectMatriculation
```json
{
  "fieldOfStudy": "Computer Science",
  "semesters": ["WS2019/20", "SS2020"]
}

```

### GenericError
```json
{
  "type": "string",
  "title": "string"
}
```

### DetailedError
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
