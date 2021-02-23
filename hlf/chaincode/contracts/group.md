# Hyperledger Group Api

In the following, the chaincode api for the "Group"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](../errors.md#Errors).

## ContractName "UC4.Group"

## Transactions

### Add User To Group
- ID = addUserToGroup
- Required Approvals
  - Users
    - lagom-scala-admin-org1 (hyperledger attr. (set when enrolling))
- Send
    - enrollmentId :: String
    - groupId :: String
- Receive
    - ""
      -  Description: Done; returns empty String on success.
    - [DetailedError](../errors.md#DetailedError) 
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
       - Description: This error is returned, 
         - if the given parameters could not be parsed. If some attributes within the json are not well formatted, they are listed in invalidParams.  
            For detailed informations see [parameter checks](#parameterChecks).
         - if the given parameters produce some [semantic error](#semanticChecks).
    
    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLInsufficientApprovals",
        "title": "The approvals present on the ledger do not suffice to execute this transaction"
      }
      ```
      - Description: This error is returned, if the required approvals are not present.


### Remove From Group
- ID = removeUserFromGroup
- Send
    - enrollmentId :: String
    - groupId :: String
- Receive
    - ""
      -  Description: Done; returns empty String on success.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no entry for the given enrollmentId and groupId."
      }
      ```
      - Description: This error is returned, if there is no match for the given enrollmentId and groupId.

    - [DetailedError](../errors.md#DetailedError) 
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
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams.  
      For detailed informations see [parameter checks](#parameterChecks).
      (enrollmentId, groupId)

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLInsufficientApprovals",
        "title": "The approvals present on the ledger do not suffice to execute this transaction"
      }
      ```
      - Description: This error is returned, if the required approvals are not present.

### Remove User From All Groups
- ID = removeUserFromAllGroups
- Send
    - enrollmentId :: String
- Receive
    - ""
      -  Description: Done; returns empty String on success.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no entry for the given enrollmentId"
      }
      ```
      - Description: This error is returned, if the enrollmentId is not present in any group.

    - [DetailedError](../errors.md#DetailedError) 
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
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams.  
      For detailed informations see [parameter checks](#parameterChecks).
      (enrollmentId)

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLInsufficientApprovals",
        "title": "The approvals present on the ledger do not suffice to execute this transaction"
      }
      ```
      - Description: This error is returned, if the required approvals are not present.



### Get All Groups
- ID = getAllGroups
- Receive
    - groupList :: List\<[Group](#Group)\>
      - Description: Exhaustive List of all Groups.
      Empty if none could be found.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.


### Get Users For Group
- ID = getUsersForGroup
- Send
    - groupId :: String
    
- Receive
    - userList :: List\<String\>
      - Description: Exhaustive List of all users in a group.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no group for the given groupId."
      }
      ```
      - Description: This error is returned, if the groupId does not exist on the ledger.

    - [DetailedError](../errors.md#DetailedError) 
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
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams.  
      For detailed informations see [parameter checks](#parameterChecks).

### Get Groups For User
- ID = getGroupsForUser
- Send
    - enrollmentId :: String

- Receive
    - groupIdList :: List\<String\>
          - Description: Exhaustive List of all groupIds the user is part of.
            Empty, if user is not part of any group.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.

    - [DetailedError](../errors.md#DetailedError) 
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
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams. It is also returned, if the user is not registered.  
      For detailed informations see [parameter checks](#parameterChecks) and [semanticChecks](#semanticChecks).

## <a id="Models" />Models

### <a id="Group" />Group
```json
{
  "groupId": "0123456ExampleGroup",
  "userList": ["EnrollmentIdExample123", "EnrollmentIdExample124"]
}
```

## <a id="Checks" />Input Checks
### <a id="parameterChecks" />Parameters
- Checks, if parseable Json.
- **enrollmentId**
  - Check, if not null or an empty String.
- **groupId**
  - Check, if not null or an empty String.

### <a id="semanticChecks" />SemanticChecks
- **userRegistered**
  - Check, if **enrollmentId** is registered in the system (existing certificate).
