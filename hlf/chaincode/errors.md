# <a id="Errors" /> Hyperledger Chaincode API Errors

In the following, the error returned by the uc4-chaincode transactions is described.

## <a id="GenericError" />GenericError
```json
{
  "type": "string",
  "title": "string"
}
```

## <a id="DetailedError" />DetailedError
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


# Specific Errors

## <a id="AllTransactions" />All Transactions

In the following, the specific errors that can be returned by every transaction are defined.

- [GenericError](#GenericError) 
  ```json
  {
    "type": "HLAccessDenied",
    "title": "You are not allowed to execute the given transaction"
  }
  ```
    - Description: This error is returned, if the user is not allowed to execute the transaction.

- [GenericError](#GenericError) 
  ```json
  {
    "type": "HLTransactionTimestampInvalid",
    "title": "The transaction you submitted contains a timestamp differing more than two minutes from the current system time"

  }
  ```
    - Description: This error is returned, if the timestamp differs more than two minutes from current system time.

## <a id="AllOperations" />All Operations

In the following, the specific errors that can additionally be returned by every operation are defined.
An operation is a transaction requiring multiple approvals.

- [GenericError](#GenericError) 
  ```json
  {
    "type": "HLInsufficientApprovals",
    "title": "The approvals present on the ledger do not suffice to execute this transaction"
  }
  ```
  - Description: This error is returned, if the required approvals are not present.

- [GenericError](#GenericError) 
  ```json
  {
    "type": "HLExecutionImpossible",
    "title": "The operation is not in pending state"
  }
  ```
  - Description: This error is returned, if the operation for this transaction is not pending.
