# Hyperledger Scala API Operation

In the following, the hlf-api for the "operation" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](errors.md#Exceptions).

For "operation" handling we offer the following interface. 

Additionally, as described in [General Communication](general-communication.md), different proposal calls can be made.


## ApproveTransaction(contractName: String, transactionName: String, params: String*))
- Returns
    - OperationData :: Json (refer to: [OperationData](../chaincode/operation.md#OperationData))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

## RejectTransaction(operationId: String, rejectMessage: String)
- Returns
    - OperationData :: Json (refer to: [OperationData](../chaincode/operation.md#OperationData))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned
          - If the operationId is not present on the ledger.
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.

## GetOperations(enrollmentId: String, state: String)

Gets the full List of existing Operations.
Applies filters to match enrollmendId, state.
If any of theses parameter is empty, its filter will not be applied.
- Returns
    - OperationsList :: List\<String Json (refer to: [OperationData](../chaincode/operation.md#OperationData))\> 
        - => Exhaustive List of all Operations, filtered by
            inputs "operationId", "state".

            Empty if no match could be found.

> Note! Invalid States on the ledger get ignored.
> We return all valid Operations that match the filters.

## GetOperationData(operationId: String)

- Returns
    - OperationData :: Json (refer to: [OperationData](../chaincode/operation.md#OperationData))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned
          - If the operationId is not present on the ledger.
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.
