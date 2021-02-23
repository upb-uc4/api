# Hyperledger Scala API Group

In the following, the hlf-api for the "group" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](../errors.md#Exceptions).

For "group" handling we offer the following interface. 
Additionally, as described in [General Communication](../general-communication.md), different proposal calls can be made.


## AddUserToGroup(enrollmentId: String, groupId: String)
- Returns
    - ""
    => Success
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
          - If the given transaction could not be performed due to semantic reasons.
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the required approvals are not present on the ledger.

## RemoveUserFromGroup(enrollmentId: String, groupId: String)
- Returns
    - ""
    => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the groupId specified is not present on the ledger.
          - If the required approvals are not present on the ledger.
          - If there is no match for enrollmenId and groupId.
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

## RemoveUserFromAllGroups(enrollmentId: String)
- Returns
    - ""
    => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If there is no entry for the user on the ledger.
          - If the required approvals are not present on the ledger.
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
              
## GetAllGroups()
- Returns
    - groupList :: List\<[Group](../../chaincode/contracts/group.md#Group)\>
    => Success

> Note! Invalid States on the ledger get ignored.
> We return all valid Groups that match the filters.

## GetUsersForGroup(groupId: String)
- Returns
    - userList :: List\<String\>
    => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If there is no entry for the group on the ledger.
          - If the required approvals are not present on the ledger.
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

## GetGroupsForUser(enrollmentId: String)
- Returns
    - groupIdList :: List\<String\>
    => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If there is no entry for the user on the ledger.
          - If the required approvals are not present on the ledger.
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
  
 >Note! Invalid States on the ledger get ignored.
> We return all valid Groups that match the filters.
