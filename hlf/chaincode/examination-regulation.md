# Hyperledger ExaminationRegulation Api

In the following, the chaincode api for the "ExaminationRegulation"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## Transactions

### AddExaminationRegulation
- ID = addExaminationRegulation
- Send
    - name :: String 
    - examinationRegulation :: List\<String\>

- Receive
    - examinationRegulation :: List\<String\>
      -  Description: Success, returns the examination regulation (list of modules).

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
       
    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLConflict",
        "title": "There is already an ExaminationRegulation for the given name",
      }
      ```
    - Description: This error is returned, if an examination regulation with the given name is already present on the ledger.
  

### GetExaminationRegulation
- ID = getExaminationRegulation
- Send
    - name :: String
- Receive
    - examinationRegulation :: List\<String\>

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no examination regulation for the given name"
      }
      ```
      - Description: This error is returned, if the examination regulation for the name is not present on the ledger.
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
