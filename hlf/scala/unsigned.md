# <a id="Unsigned" /> Hyperledger Scala API - Unsigned Transactions

## General Communication
For all connection purposes some basic information on the network to access has to be provided.
```scala
protected val walletPath: Path = "/hyperledger_assets/wallet/" // the directory containing your certificates.
protected val networkDescriptionPath: Path = "/hyperledger_assets/connection_profile.yaml" // the file describing the existing network.
protected val channel: String = "myc" // name of the shared channel a connection is requested for.
protected val chaincode: String = "mycc" // name of the chaincode a connection is requested for.
```

Depending on the type of network accessed, additional information is needed for authentication
```scala
protected val tlsCert: Path = "/hyperledger_assets/ca_cert.pem" // CA-certificate to have your client validate that the Server you are talking to is actually the CA.
protected val caURL: String = "172.17.0.3:30906" // address of the CA-server.

protected val username: String = "TestUser123" // this should in most cases be the name of the .id file in your wallet directory.
protected val password: String = "Test123" // a password used to register a user and receive/set a certificate for said user when enrolling.
protected val organisationId: String = "org1MSP" // the id of the organisation the user belongs to.
protected val organisationName: String = "org1" // the name of the organisation the user belongs to.
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
All chaincode-transactions are furthermore provided a an "unsigned" proposal as well.
- Here some sort of ADMIN has to provide their credentials [..] when initiating a connection.
```scala
val certificateConnection: ConnectionCertificateTrait = 
    ConnectionCertificate(adminUserName, this.channel, this.chaincode, this.adminWalletPath, this.networkDescriptionPath)
```
The ADMIN can then 
- request such an "unsigned" proposal
```scala
val proposal: Array[Byte] = certificateConnection.getProposalAddCertificate(enrollmentId, certificate)
```
- pass it on to the USER that wants to sign it
- have the USER create his signature for the "unsigned" proposal
- receive the signature from the USER (together with the original proposal?)
- submit the signed proposal ("unsigned" proposal, signature)
```scala
certificateConnection.submitSignedProposal(proposalBytes, signature)
```

For additional insight in the General hlf-api, please refer to it's .Readme - File.