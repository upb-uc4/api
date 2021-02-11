# Hyperledger Scala API Matriculation

In the following, the hlf-api for the "matriculation data" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](../errors.md#Exceptions).

For "matriculation data" handling we offer the following interface. Additionally, as described in [General Communication](../general-communication.md), different proposal calls can be made.


## AddMatriculationData(MatriculationData :: Json([MatriculationData](../../chaincode/contracts/matriculation.md#MatriculationData)))
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](../../chaincode/contracts/matriculation.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
         
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned
           - If a matriculation data with the given enrollmentId is already present on the ledger.

## UpdateMatriculationData(MatriculationData :: Json([MatriculationData](../../chaincode/contracts/matriculation.md#MatriculationData)))
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](../../chaincode/contracts/matriculation.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the enrollmentId specified in the MatriculationData is not present on the ledger.
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

## GetMatriculationData(enrollmentId :: String)
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](../../chaincode/contracts/matriculation.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned,
          - If the enrollmentId is not present on the ledger.
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.

## AddEntriesToMatriculationData(enrollmentId :: String, entries :: List<Json([SubjectMatriculation](../../chaincode/contracts/matriculation.md#SubjectMatriculation))>)
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](../../chaincode/contracts/matriculation.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
      - => error is returned,
       
        - If the enrollmentId is not present on the ledger.
        - If the state of data on the ledger is not consistent with the curent model.
          - This error should only occurr if the model changes while the old ledger state remains without modification.
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
      - => error is returned
        - If the given parameters could not be parsed.
 
