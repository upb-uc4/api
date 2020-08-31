# Hyperledger Matriculation Api

In the following, the hlf-api for the "matriculation data" - handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File

## Methods

Here the possible Actions are presented, that can be invoked on the hyperledger-network.
Any Method can at any point return 2 types of errors. 
  - TransactionError(methodName, JsonError) :: Something went wrong during the action. The Model recognized an invalidity.
  - HyperledgerError(methodName, Exception) :: Something went wrong with the Framework. Hyperledger threw an internal error.

### Add Matriculation Data
- Pass (*transient*)
    - MatriculationData :: Json (refer to: [MatriculationData](#matriculationData))
- Receive
    - "" :: String
        => Success
    - TransactionError :: Json (refer to chapter: "Errors - DetailedError")
        => error is returned
          If the given parameters could not be parsed.
            If the string could not be parsed as json, the list of invalidParams is empty.
            If some attributes within the json are not well formatted, they are listed in invalidParams.
          If a matriculation data with the given matriculationId is already present on the ledger.
    - TransactionError :: Json (refer to: [GenericError](#genericError---example))
        => error is returned
          If the given matriculation data could not be parsed as json.

### Update Matriculation Data
- Pass (*transient*)
    - MatriculationData :: Json (refer to: [MatriculationData](#matriculationData))
- Receive
    - "" :: String
        => Success
    - TransactionError :: Json (refer to chapter: "Errors - GenericError")
        => error is returned, 
          If the matriculationID specified in the MatriculationData is not present on the ledger.
          If the given matriculation data could not be parsed as json.
    - TransactionError :: Json (refer to chapter: "Errors - DetailedError")
        => error is returned
          If the given parameters could not be parsed.
            If the string could not be parsed as json, the list of invalidParams is empty.
            If some attributes within the json are not well formatted, they are listed in invalidParams.

### Get Matriculation Data
- Send
    - matriculationId :: String
- Receive
    - MatriculationData :: Json (refer to: [MatriculationData](#matriculationData))
        => Success
    - TransactionError :: Json (refer to chapter: "Errors - GenericError")
        => error is returned,
          If the matriculationID is not present on the ledger.
          If the state of data on the ledger is not consistent with the curent model.
            This error should only occurr if the model changes while the old ledger state remains without modification.

### AddEntriesToMatriculationData
- Send
    - matriculationId
    - listOf(SubjectMatriculation) :: List<SubjectMatriculation> (refer to: [SubjectMatriculation](#subjectMatriculation))
- Receive
    - "" :: String
      => Success
    - TransactionError :: Json (refer to chapter: "Errors - GenericError")
        => error is returned,
          If the matriculationID is not present on the ledger.
          If the state of data on the ledger is not consistent with the curent model.
            This error should only occurr if the model changes while the old ledger state remains without modification.
     TransactionError :: Json (refer to chapter: "Errors - DetailedError")
        => error is returned
          If the given parameters could not be parsed.
            If the string could not be parsed as json, the list of invalidParams is empty. 
            If some attributes within the json are not well formatted, they are listed in invalidParams.

## Models

### MatriculationData
```json
{
  "matriculationId": "string",
  "firstName": "string",
  "lastName": "string",
  "birthDate": "2020-07-21",
  "matriculationStatus": [
    subjectMatriculationInfo :: SubjectMatriculation
    subjectMatriculationInfo2 :: SubjectMatriculation
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

## Errors

### GenericError - Example
```json
{
  "type": "hl: invalid transaction call",
  "title": "The transaction was invoked with the wrong amount of parameters. Expected: ${expected} Actual: ${actual}",
  "transactionId": "addMatricuationData"
}
```

### DetailedError - Example
```json
{
  "type": "hl: unprocessable field",
  "title": "The following fields in the given parameters do not conform to the specified format.",
  "invalidParams": [
    invalidParameter1 :: Invalid Parameter
    invalidParameter2 :: Invalid Parameter
    invalidParameter3 :: Invalid Parameter
  ]
}
```

### InvalidParameter
```json
{
  "name": "string",
  "reason": "string"
}
```
name:
- name of the invalid parameter in simple dot- and array notation, i.e. if the second semester entry in the ```semesters``` field of the first ```SubjectMatriculation``` object in the ```matriculationStatus``` field of a ```MatriculationData``` object is invalid, the ```name``` field of the ```InvalidParameter``` object would be ```"matriculationStatus[0].semesters[1]"```

reason:
- a textual representation of why the parameter is invalid
