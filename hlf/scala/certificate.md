# Hyperledger Scala API Certificate

In the following, the hlf-api for the "certificate" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](errors.md#Exceptions).

For "Certificate" handling we offer the following interface. Additionally as described in [General Communication](general-communication.md) different proposal calls can be made.

## AddCertificate(enrollmentID :: String, certificate :: String)
- Returns
    - certificate :: String
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned
          - If a certificate for the given enrollmentId is already present on the ledger.

## UpdateCertificate(enrollmentID :: String, certificate :: String)
- Returns
    - certificate :: String
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the enrollmentId is not present on the ledger.
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

## GetCertificate(enrollmentID :: String)
- Returns
    - Certificate :: String
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned,
          - If the enrollmentId is not present on the ledger.
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.