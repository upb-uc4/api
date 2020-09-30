# <a id="Exceptions" /> Hyperledger Scala API Errors

In the following, the general error - handling when using the HLF-Scala-API will be explained.
For additional insight in the General hlf-api, please refer to it's .Readme - File.

## Possible Exceptions

Here the possible Errors are presented, which can be thrown when accessing the hyperledger-network.

There are only 3 types of errors that can will be thrown.
- TransactionError(transactionID :: String, payload :: JsonError) :: Something went wrong during the action. The Model recognized an invalidity.
  These encapsulate all errors thrown by the hlf-chaincode.
  They are defined in [Errors](hlf_chaincode_api_errors.md#Errors).
- HyperledgerError(transactionID, payload :: innerException) :: Something went wrong with the Framework. Hyperledger threw an internal error.
- NetworkError(channel, chaincode, networkDescription, identity: String = null, organisationId, organisationName, innerException) :: You were not able to connect to the network.
