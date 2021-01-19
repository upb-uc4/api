# <a id="General Communication" /> Hyperledger Scala API - General Communication

## API
Following API calls can be made on all connections

### GetChaincodeVersion()
- Returns 
    - ChaincodeVersion :: String 

### SubmitSignedProposal(proposalBytes: Array[Byte], signature: Array[Byte], userId: String)
Submits the given Proposal to the existing Connection
- Returns
    -  ChaincodeAnswer :: String 
        - Description: Whatever the connection would usually return upon being directly invoked with the transaction

## General Communication
For all connection purposes some basic information on the network to access has to be provided.
```scala
protected val walletPath: Path = "/hyperledger_assets/wallet/" // the directory containing your certificates.
protected val networkDescriptionPath: Path = "/hyperledger_assets/connection_profile.yaml" // the file describing the existing network.
protected val channel: String = "myc" // name of the shared channel a connection is requested for.
protected val chaincode: String = "mycc" // name of the chaincode a connection is requested for.
```

Depending on the type of network accessed, further information is needed for authentication
```scala
protected val tlsCert: Path = "/hyperledger_assets/ca_cert.pem" // CA-certificate to have your client validate that the server you are talking to is actually the CA.

protected val caURL: String = "172.17.0.3:30906" // address of the CA-server.

protected val username: String = "TestUser123" // this should in most cases be the name of the .id file in your wallet directory.
protected val password: String = "Test123" // a password used to register a user and receive/set a certificate for said user when enrolling.
protected val organizationId: String = "org1MSP" // the id of the organization the user belongs to.

protected val organizationName: String = "org1" // the name of the organization the user belongs to.

```

## Regular Communication
All chaincode-transactions are per default provided via a Connection-Pattern.
- Here the USER has to provide their credentials to authenticate themselves when initiating a connection.
    This is usually done by providing a wallet (walletPath) containing the Keys of the User ( in a file "userName.id")
```scala
val certificateConnection: ConnectionCertificateTrait = 
    ConnectionCertificate(userName, this.channel, this.chaincode, this.walletPath, this.networkDescriptionPath)
```
- Subsequent proposals are then signed with the credentials belonging to the USER initiating the connection.
    This simplifies the API-handling to method-calls.
```scala
val certificate: String = Source.fromURL(getClass.getResource("/testid.csr")).mkString
certificateConnection.addCertificate(enrollmentId, certificate)
```

## THIRD-PARTY-Signed Communication
All chaincode-transactions are furthermore provided as an "unsigned" proposal as well.

- Here some sort of ADMIN has to provide their credentials [..] when initiating a connection.
```scala
val certificateConnection: ConnectionCertificateTrait = 
    ConnectionCertificate(adminUserName, this.channel, this.chaincode, this.adminWalletPath, this.networkDescriptionPath)
```
The ADMIN can then 
- request such an "unsigned" proposalBytes
```scala
val (approvalResult: String, proposalBytes: Array[Byte]) = certificateConnection.getProposalAddCertificate(userCertificate, enrollmentId, certificate)
```
The approvalResult is a JsonObject [OperationData](../chaincode/operation.md#OperationData).
The proposalBytes have to be relayed to the USER to sign it. The proposal itself is for the transaction defined in [ApproveOperation](./operation.md#ApproveOperation); an approval for the initiated transaction, to be signed by the user.
**_IMPORTANT: When asking for a proposal, the credentials used for setting up the initial connection are used to approve the transaction as well (in this case as the ADMIN)_**

- pass the proposalBytes with approvalResult on to the USER who wants to sign it
- have the USER create their signature for the "unsigned" proposalBytes
- receive the signature from the USER (together with the original proposalBytes)

- submit the signed proposal ("unsigned" proposalBytes, signatureBytes) to retrieve an usigned Transaction.
> While the proposal is fetched from the specific connections (e.g. MatriculationConnection, CertificateConnection, ...), the getUnsignedTransaction, which submits the signed proposal, is called on the OperationConnection.
```scala
val transactionBytes = operationConnection.getUnsignedTransaction(proposalBytes: Array[Byte], signatureBytes: Array[Byte])
```

- pass the transactionBytes on to the USER who wants to sign it
- have the USER create his signature for the "unsigned" transactionBytes
- receive the signature from the USER (together with the original transactionBytes)

- submit the signed transaction ("unsigned" transactionBytes, signatureBytes)
>The Connection receiving the signedTransaction is always the OperationConnection.
```scala
val approvalResult = operationConnection.submitSignedTransaction(transactionBytes: Array[Byte], signatureBytes: Array[Byte])

val transactionResult = operationConnection.executeTransaction(jsonOperationData: String)
```
The approvalResult is a JsonObject [OperationData](../chaincode/operation.md#OperationData). Invoking executeTransactionFromOperation executes the transaction described by the transactionInfo in the OperationData.  
The transactionResult is the chaincode response to the desired transaction
(e.g. an InsufficientApproval Error, or information about the successfully changed ledger state).

If submitSignedTransaction succeeds, the approval of the User has been stored on the OperationContract.  
If executeTransaction succeeds, the "real" transaction (e.g. "AddCertificate") is executed.  
If the approval or the "real" transaction fails due to the transaction itself being invalid (e.g. due to changed ledger state), the operation is rejected.

========================  

The initiator of an operation calls "getProposal[TransactionX]", "getUnsignedTransaction", "submitSignedTransaction", and "executeTransaction" as described above.

Every subsequent approval is given by fetching the corresponding proposal for [ApproveOperation](./operation.md#ApproveOperation), identified by operationId.
```scala
val proposal = operationConnection.getProposalApproveOperation(operationId: String)
```
Rejection is performed analogously, by fetching a proposal for [RejectOperation](./operation.md#RejectOperation), identified by operationId. Also, a rejectMessage must be supplied.
```scala
val proposal = operationConnection.getProposalRejectOperation(operationId: String, rejectMessage: String)
```

The USER then proceeds with "getUnsignedTransaction", "submitSignedTransaction", "executeTransaction" as before.

If not all required approvals are present, the "executeTransaction" (e.g. "AddCertificate") will fail (i.e. with InsufficientApproval Error).  
If all required approvals are present if the transaction is still valid, "executeTransaction" will perform the desired transaction on chain.

For additional insight in the General hlf-api, please refer to its .Readme - File.
