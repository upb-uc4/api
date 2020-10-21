# Hyperledger Draft Api

In the following, the chaincode api for the "Draft"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## Transactions

### AddOrExtendSignature
- ID = addOrExtendSignature
- Send
    - hash(transactionName, params) :: String
    - signature :: String (base64)
- Receive
    - signatureData :: [SignatureData](#SignatureData)
      -  Description: Success, returns the submitted certificate.

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

### GetSignatures
- ID = getSignatures
- Send
    - hash(transactioName, params) :: String
- Receive
    - signatures :: List\<String\>

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There are not signatures for the given transaction"
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

    - 

## <a id="Models" />Models

### <a id="SignatureData" />SignatureData
```json
{
  "transactionHash": "string",
  "signatures": [
    "string",
    "string"
  ]
}
```
