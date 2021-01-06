# Hyperledger Operation Api

In the following, the chaincode api for the "Operation"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## Transactions

### ApproveTransaction
- ID = approveTransaction
- Send
    - contractName, transactionName :: String
    - params :: List\<String\>

- Receive
    - operationData :: [OperationData](#OperationData) 
      -  Description: Success, returns the list of approvals.

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
       - Description: This error is returned, if the given parameters could not be parsed. If some attributes are not well formatted, they are listed in invalidParams.  
       For detailed informations see [Input Checks](#Checks).

### RejectTransaction
- ID = rejectTransaction
- Send
    - operationId :: String
    - rejectMessage :: String
- Receive
    - operationData :: [OperationData](#OperationData) 
      -  Description: Success, returns the list of operations.


    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the current model. This error should only occur if the model changes while the old ledger state remains without modification.

- [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no Operation for the given operationId"
      }
      ```
      - Description: This error is returned, if there is no operation for the specified operationId present on the ledger.
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
       - Description: This error is returned, if the given parameters could not be parsed. If some attributes are not well formatted, they are listed in invalidParams.  
       For detailed informations see [Input Checks](#Checks).


### GetOperationData
- ID = getOperationData
- Send
    - operationId :: String
- Receive
    - operationData :: [OperationData](#OperationData)

- [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no Operation for the given operationId"
      }
      ```
      - Description: This error is returned, if there is no operation for the specified operationId present on the ledger.
    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the current model. This error should only occur if the model changes while the old ledger state remains without modification.

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
       - Description: This error is returned, if the given parameters could not be parsed. If some attributes are not well formatted, they are listed in invalidParams.  
       For detailed informations see [Input Checks](#Checks).

### GetOperations
- ID = getOperations
- Send
    - enrollmentId :: String (optional)
    - state :: String (optional)
- Receive
    - operationsList :: List\<[OperationData](#OperationData)\>
      - Description: Returns the full list of existing Operations matching the filter parameters.
        If these parameters are empty, the exhaustive list of all existing Operations is returned.

## <a id="Models" />Models

### <a id="OperationData" />OperationData
```json
{
  "operationId" : "0424974c68530290458c8d58674e2637f65abc127057957d7b3acbd24c208f93",
  "transactionInfo" : {
    "contractName" : "UC4.Certificate",
    "transactionName" : "addCertificate",
    "parameters": "[
      \"EnrollmentID_001\",
      \"legit_certificate\"
    ]"
  },

  "state" : "(PENDING|FINISHED|REJECTED)",
  "reason" : "(A User denied with the following message: <message>
              |The Transaction failed with an error of type: 'HLConflict')",
  "existingApprovals": {
    "users": [
      "EnrollmentID_001"
    ],
    "groups": [
      "Student"
    ]
  },
  "missingApprovals": {
    "users": [
      "Lagom-Scala-Admin"
    ],
    "groups": [
      "Admins"
    ]
  }
}
```

### <a id="ApprovalList" />ApprovalList
```json
{
  "users": [
    "EnrollmentID_001",
    "EnrollmentID_002"
  ],
  "groups": [
    "Student"
  ]
}
```

### <a id="TransactionInfo" />TransactionInfo
```json
{
  "contractName" : "UC4.Certificate",
  "transactionName" : "addCertificate",
  "parameters": "[
    \"EnrollmentID_001\",
    "\legit_certificate\"
  ]"

}
```

## <a id="Checks" />Input Checks

- **operationId**
  - Check, if not null or an empty String.
- **contractName**
  - Check, if not null or an empty String.
- **transactionName**
  - Check, if not null or an empty String.