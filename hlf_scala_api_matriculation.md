# Hyperledger Scala API Matriculation

In the following, the hlf-api for the "matriculation data" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](hlf_scala_api_errors.md#Exceptions).

For "matriculation data" handling we offer the following interface.

## AddMatriculationData(MatriculationData :: Json([MatriculationData](hlf_chaincode_api_matriculation.md#MatriculationData)))
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](hlf_chaincode_api_matriculation.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](hlf_chaincode_api_errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
          - If a matriculation data with the given enrollmentId is already present on the ledger.
    - TransactionError :: Json (refer to: [GenericError](hlf_chaincode_api_errors.md#GenericError))
        - => error is returned
          - If the given matriculation data could not be parsed as json.

## UpdateMatriculationData(MatriculationData :: Json([MatriculationData](hlf_chaincode_api_matriculation.md#MatriculationData)))
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](hlf_chaincode_api_matriculation.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](hlf_chaincode_api_errors.md#GenericError))
        - => error is returned, 
          - If the enrollmentId specified in the MatriculationData is not present on the ledger.
          - If the given matriculation data could not be parsed as json.
    - TransactionError :: Json (refer to: [DetailedError](hlf_chaincode_api_errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

## GetMatriculationData(matriculationID :: String)
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](hlf_chaincode_api_matriculation.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](hlf_chaincode_api_errors.md#GenericError))
        - => error is returned,
          - If the enrollmentId is not present on the ledger.
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.

## AddEntriesToMatriculationData(matriculationID :: String, entries :: List<Json[SubjectMatriculation](hlf_chaincode_api_matriculation.md#SubjectMatriculation)>)
- Returns
    - MatriculationData :: Json (refer to: [MatriculationData](hlf_chaincode_api_matriculation.md#MatriculationData))
        - => Success
    - TransactionError :: Json (refer to: [GenericError](hlf_chaincode_api_errors.md#GenericError))
      - => error is returned,
        - If the given matriculation data could not be parsed as json.
        - If the enrollmentId is not present on the ledger.
        - If the state of data on the ledger is not consistent with the curent model.
          - This error should only occurr if the model changes while the old ledger state remains without modification.
     TransactionError :: Json (refer to: [DetailedError](hlf_chaincode_api_errors.md#DetailedError))
      - => error is returned
        - If the given parameters could not be parsed.