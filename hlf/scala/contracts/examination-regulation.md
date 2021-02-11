# Hyperledger Scala API Examination Regulation

In the following, the hlf-api for handling the "examination regulation data" will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](../errors.md#Exceptions).

For "examination regulation data" handling we offer the following interface. Additionally, as described in [General Communication](general-communication.md), different proposal calls can be made.



## AddExaminationRegulation(examinationRegulation :: Json([ExaminationRegulation](../../chaincode/contracts/examination-regulation.md#ExaminationRegulation)))
- Returns
    - ExaminationRegulation :: Json (refer to: [ExaminationRegulation](../../chaincode/contracts/examination-regulation.md#ExaminationRegulation))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned
          - This error is returned, if an examination regulation with the given name is already present on the ledger.

## GetExaminationRegulations(nameList :: String)
- Returns
    - List<Json(ExaminationRegulation)> :: Json (refer to: [ExaminationRegulation](../../chaincode/contracts/examination-regulation.md#ExaminationRegulation))
        - => Success
            - If no name is given (i.e. empty list etc.) return all examination regulations
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned
          - If the state of data on the ledger is not consistent with the current model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.

## CloseExaminationRegulation(name :: String)
- Returns
    - ExaminationRegulation :: Json (refer to: [ExaminationRegulation](../../chaincode/contracts/examination-regulation.md#ExaminationRegulation))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned,
          - If the examination regulation with the given name is not present on the ledger.
          - If the state of data on the ledger is not consistent with the current model. 
            - This error should only occur if the model changes while the old ledger state remains without modification.
