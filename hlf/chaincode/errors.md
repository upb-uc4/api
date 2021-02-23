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

In the following, the specific error that can be returned by every transaction not part of the [OperationContract](./contracts/operation.md#OperationApi)  due to our operation workflow.

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
    "type": "HLParticipationImpossible",
    "title": "The operation is not in pending state"
  }
  ```
  - Description: This error is returned, if the operation for this transaction is not pending.

- [GenericError](#GenericError) 
  ```json
  {
    "type": "HLAccessDenied",
    "title": "You are not allowed to execute in the given transaction"
  }
  ```
    - Description: This error is returned, if the user trying to execute the transaction is not allowed to participate in the operation.
