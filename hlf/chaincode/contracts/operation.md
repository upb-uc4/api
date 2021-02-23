# Hyperledger Operation Api

In the following, the chaincode api for the "Operation"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](../errors.md#Errors).

## Transactions

### InitiateOperation
- ID = initiateOperation

- Send
    - initiator :: String (enrollmentId)
    - contractName :: String
    - transactionName :: String
    - params :: List\<String\>

- Receive
    - operationData :: [OperationData](#OperationData) 
      -  Description: Success, returns the list of approvals.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLParameterNumberError",
        "title": "The given number of parameters does not match the required number of parameters for the specified transaction"
      }
      ```
       - Description: This error is returned, if the given number of parameters for the specified transaction does not match the number of required parameters.
    
    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLApprovalDenied",
        "title": "You are not allowed to approve the given operation"
      }
      ```
       - Description: This error is returned, if the user trying to approve is not allowed to approve the operation.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLApprovalImpossible",
        "title": "The operation is not in pending state"
      }
      ```
       - Description: This error is returned, if the user is trying to approve an operation that is not pending.

    - [DetailedError](../errors.md#DetailedError) 
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

### ApproveOperation
- ID = approveOperation
- Send
    - operationId :: String

- Receive
    - operationData :: [OperationData](#OperationData) 
      -  Description: Success, returns the list of operations.
    
    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLApprovalDenied",
        "title": "You are not allowed to approve the given operation"
      }
      ```
       - Description: This error is returned, if the user trying to reject is not allowed to reject the operation.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLApprovalImpossible",
        "title": "The operation is not in pending state"
      }
      ```
       - Description: This error is returned, if the user is trying to reject an operation that is not pending

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no Operation for the given operationId"
      }
      ```
      - Description: This error is returned, if there is no operation for the specified operationId present on the ledger.
    
    - [DetailedError](../errors.md#DetailedError) 
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

### RejectOperation
- ID = rejectOperation
- Send
    - operationId :: String
    - rejectMessage :: String
- Receive
    - operationData :: [OperationData](#OperationData) 
      -  Description: Success, returns the list of operations.

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLRejectionDenied",
        "title": "You are not allowed to reject the given operation"
      }
      ```
       - Description: This error is returned, if the user trying to reject is not allowed to reject the operation.

- [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLRejectionImpossible",
        "title": "The operation is not in pending state"
      }
      ```
       - Description: This error is returned, if the user is trying to reject an operation that is not pending

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the current model. This error should only occur if the model changes while the old ledger state remains without modification.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no Operation for the given operationId"
      }
      ```
      - Description: This error is returned, if there is no operation for the specified operationId present on the ledger.
    - [DetailedError](../errors.md#DetailedError) 
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
    - operationIds :: List\<String\> (optional)
    - existingEnrollmentId :: String (optional)
    - missingEnrollmentId :: String (optional)
    - initiatorEnrollmentId :: String (optional)
    - involvedEnrollmentId :: String (optional)
    - states :: List\<String\> (optional)
- Receive
    - operationsList :: List\<[OperationData](#OperationData)\>
      - Description: Returns the full list of existing Operations matching the filter parameters.
        All filters are applied consecutively (logical AND).
      - If all these parameters are empty, the exhaustive list of all existing Operations is returned.
      - "involvedEnrollmentId" represents an OR-Filter for existing-/missing-/initiator-enrollmentId
      - List parameters are also a logical OR over all their entries

## <a id="Models" />Models

### <a id="OperationData" />OperationData
The operationId in OperationData is constructed as follows:  
operationId = String(Base64Url(SHA256($contractName:$transactionName:$parameters))) // parameters is stripped of all spaces (replace(" ", ""))


with the String given to the hash function being UTF8 encoded.
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
  "initiator" : "EnrollmentID_001"
  "initiatedTimestamp" : "2004-06-14T23:34:30" \<DATE ISO 8601 YYYY-MM-DDThh:mm:ss\>
  "lastModifiedTimestamp" : "2004-06-14T23:34:30" \<DATE ISO 8601 YYYY-MM-DDThh:mm:ss\>
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
