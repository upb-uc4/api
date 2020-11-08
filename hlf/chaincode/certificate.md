# Hyperledger Certificate Api

In the following, the chaincode api for the "Certificate"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## Transactions

### Add Certificate
- ID = addCertificate
- Send
    - enrollmentId :: String
    - certificate :: String
- Receive
    - certificate :: String
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
       For detailed informations see [Input Checks](#Checks).

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLConflict",
        "title": "There is already a certificate for the given enrollmentId",
      }
      ```
      - Description: This error is returned, if a certificate for the given enrollmentId is already present on the ledger.


### Update Certificate
- ID = updateCertificate
- Send
    - enrollmentId :: String
    - certificate :: String
- Receive
    - certificate :: String
      -  Description: Done, returns the newly submitted certificate.

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no certificate for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId is not present on the ledger.

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
      - Description: This error is returned, if the given parameters could not be parsed. If some attributes within the json are not well formatted, they are listed in invalidParams.  
      For detailed informations see [Input Checks](#Checks).

### Get Certificate
- ID = getCertificate
- Send
    - enrollmentId :: String
- Receive
    - certificate :: String

    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no certificate for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId is not present on the ledger.
    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.
    
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
      - Description: This error is returned, if the given parameters could not be parsed. If some attributes within the json are not well formatted, they are listed in invalidParams.  
      For detailed informations see [Input Checks](#Checks).


## <a id="Models" />Models

- None

## <a id="Checks" />Input Checks
  - **enrollmentId**
    - Check, if not null or an empty String.
  - **certificate**
    - Check, if not null or an empty String.