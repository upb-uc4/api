# Hyperledger Approval Api

In the following, the chaincode api for the "Approval"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## Transactions

### ApproveTransaction
- ID = approveTransaction
- Send
    - contractName, transactionName, param1, ..., paramN :: String
- Receive
    - approvals :: List\<String\>
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

### GetApprovals
- ID = getApprovals
- Send
    - contractName, transactionName, param1, ..., paramN :: String
- Receive
    - approvals :: List\<String\>

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There are no approvals for the given transaction"
      }
      ```
      - Description: This error is returned, if the transaction is not present on the ledger.
    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the current model. This error should only occur if the model changes while the old ledger state remains without modification.


## <a id="Models" />Models

- None