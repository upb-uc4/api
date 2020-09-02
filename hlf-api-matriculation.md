# Hyperledger Matriculation Api

In the following, the hlf-api for the "matriculation data" - handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File

## Methods

Here the possible Actions are presented, that can be invoked on the hyperledger-network.
> [!NOTE]
> Any Method can at any point return 2 types of errors. 
>  - TransactionError(methodName, JsonError) :: Something went wrong during the action. The Model recognized an invalidity.
>  - HyperledgerError(methodName, Exception) :: Something went wrong with the Framework. Hyperledger threw an internal error.

### AddMatriculationData(MatriculationData :: Json([MatriculationData](hyperledger_matriculation_api.md#MatriculationData)))
- Returns
    - "" :: String
        => Success
    - TransactionError :: Json (refer to: [DetailedError](hyperledger_matriculation_api.md#DetailedError---example))
        - => error is returned
          - If the given parameters could not be parsed.
          - If a matriculation data with the given matriculationId is already present on the ledger.
    - TransactionError :: Json (refer to: [GenericError](hyperledger_matriculation_api.md#GenericError---example))
        - => error is returned
          - If the given matriculation data could not be parsed as json.

### UpdateMatriculationData(MatriculationData :: Json([MatriculationData](hyperledger_matriculation_api.md#MatriculationData)))
- Returns
    - "" :: String
        - => Success
    - TransactionError :: Json (refer to: [GenericError](hyperledger_matriculation_api.md#GenericError))
        - => error is returned, 
          - If the matriculationID specified in the MatriculationData is not present on the ledger.
          - If the given matriculation data could not be parsed as json.
    - TransactionError :: Json (refer to: [DetailedError](hyperledger_matriculation_api.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

### GetMatriculationData(matriculationID :: String)
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](hyperledger_matriculation_api.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](hyperledger_matriculation_api.md#GenericError))
        - => error is returned,
          - If the matriculationID is not present on the ledger.
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.

### AddEntriesToMatriculationData(matriculationID :: String, entries :: List<Json[SubjectMatriculation](hyperledger_matriculation_api.md#SubjectMatriculation)>)
- Returns
    - "" :: String
      - => Success
    - TransactionError :: Json (refer to: [GenericError](hyperledger_matriculation_api.md#GenericError))
      - => error is returned,
        - If the given matriculation data could not be parsed as json.
        - If the matriculationID is not present on the ledger.
        - If the state of data on the ledger is not consistent with the curent model.
          - This error should only occurr if the model changes while the old ledger state remains without modification.
     TransactionError :: Json (refer to: [DetailedError](hyperledger_matriculation_api.md#DetailedError))
      - => error is returned
        - If the given parameters could not be parsed.