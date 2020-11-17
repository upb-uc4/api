# Hyperledger Scala API Examination Regulation

In the following, the hlf-api for the "examtination regulation data" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](errors.md#Exceptions).

For "examtination regulation data" handling we offer the following interface. Additionally as described in [General Communication](general-communication.md) different proposal calls can be made.



## AddExaminationRegulation(examinationRegulation :: Json([ExaminationRegulation](../chaincode/examination-regulation.md#ExaminationRegulation)))
- Returns
    - ExaminationRegulation :: Json (refer to: [ExaminationRegulation](../chaincode/examination-regulation.md#ExaminationRegulation))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned
          - If the given ExaminationRegulation data could not be parsed as json.

## GetExaminationRegulations(nameList :: String)
- Returns
    - List<Json(ExaminationRegulation)> :: Json (refer to: [ExaminationRegulation](../chaincode/examination-regulation.md#ExaminationRegulation))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned
          - If the ???  is not present on the ledger.
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.

## CloseExaminationRegulation(name :: String)
- Returns
    - ExaminationRegulation :: Json (refer to: [ExaminationRegulation](../chaincode/examination-regulation.md#ExaminationRegulation))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned,
          - If the examination regulation with the given name is not present on the ledger.
