# Infra DID Method Specification

[InfraBlockchain](https://infrablockchain.com) is a blockchain solution 
without cryptocurrency for the public sectors and enterprises. 
Infra DID method is designed for the underlying InfraBlockchain network to be use as DID    
The Infra DID method specification conforms to the requirements specified in
the [DID specification](https://w3c-ccg.github.io/did-core/), currently published by the
W3C Credentials Community Group.

## 1. Infra DID

### 1.1 Infra DID Method Name

The namestring that shall identify this DID method is: `infra`

A DID that uses this method MUST begin with the following prefix: `did:infra`. Per the DID specification, this string
MUST be in lowercase. The remainder of the DID, after the prefix, is specified below.


### 1.2 Infra DID Method Specific Identifier

The Infra DID method specific identifiers are categorized as two DID types, 
public-key-based DID (`PubKey DID`) and blockchain-account-based DID (`Account DID`).   

```
infra-did = "did:infra:" + infra-did-specific-idstring
infra-did-specific-idstring = network-id + ":" + public-key / blockchain-account
network-id = chain-name / chain-id-prefix(4bytes) 
public-key = "PUB_K1_{base58-encoded-secp256k1-public-key-plus-checksum}" / "PUB_R1_{base58-encoded-secp256r1-public-key-plus-checksum"
blockchain-account = EOSIO-blockchain-account-name
```
* Infra DID examples

| did                                                       | type        | description
|:----------------------------------------------------------|-------------|-----------------------------------------------------------------------------
| did:infra:test01:PUB_K1_7nxEa8qHEiy34d...h8n5ikapZZrzjx   | PubKey DID  | a `did` whose info can be checked from `test01` blockchain network, network-specific id is base58-encoded secp256k1 public key    
| did:infra:sentinel:PUB_R1_4vSVS9mQbiKu...bw8Mti7mc2F5     | PubKey DID  | a `did` on `sentinel` network, id is secp256r1(p256) public-key
| did:infra:kornet:hu23nfowuehx                             | Account DID | a `did` representing blockchain account `hu23nfowuehx` on `kornet` blockchain network 


#### 1.2.1 Network-ID

`network-id` is a identifier representing a specific instance of InfraBlockchain network. Each InfraBlockchain network used as DID registry 
must have standard Infra DID registry smart contracts deployed at publicly known addresses on the blockchain network.
Open source Infra DID registry contract codes are available on below repository.

Infra DID registry contracts : [https://github.com/InfraBlockchain/infra-did-registry](<>)

* Known InfraBlockchain `network-id` list

Name         | network-id  | DID Registry address
-------------|-------------|----------------
Yosemite     | `yos`       | infradidregi
Sentinel     | `sentinel`  | infradidregi
Vapp Testnet | `vapptest1` | fmapkumrotfc
..           |             |

#### 1.2.2 Public-Key-based DID (`PubKey DID`)

Any public/private key pair generated off-chain is regarded as a valid DID on InfraBlockchain network.
Currently supported elliptic curve types for key-pairs are `secp256k1` and `secp256r1`(p256).
The DID owner needs to securely save and manage the private key to verify the ownership
and to control the status and data of the 'PubKey DID' on a blockchain network.
`PubKey DID` is designed to be used as *light-weight* DID minimizing blockchain transactions.
A `PubKey DID` with a public key encoded on the "did string" itself is a valid (not-revoked, active) DID 
even if the public key of the DID is not recorded on a specific blockchain, i.e. no blockchain transaction 
needed for basic DID creation. Blockchain transactions occur only when the status of a DID is modified. 
(e.g. DID is revoked, registered service endpoint for a DID, controller key is changed, and so on)


(EOSIO-compatible)
( {33bytes pub} + {4byte checksum} )

#### 1.2.3 Blockchain-Account-based DID (`Account DID`)

(upto 13characters)




### 2. DID Document
```json
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:infra:local:PUB_K1_7nxEa8qHEiy34dpuYH4yE2zRWaAoeT1gsdTnh8n5ikapZZrzjx",
  "publicKey": [
    {
      "id": "did:infra:local:PUB_K1_7nxEa8qHEiy34dpuYH4yE2zRWaAoeT1gsdTnh8n5ikapZZrzjx#controller",
      "type": "Secp256k1VerificationKey2018",
      "controller": "did:infra:local:PUB_K1_7nxEa8qHEiy34dpuYH4yE2zRWaAoeT1gsdTnh8n5ikapZZrzjx",
      "publicKeyHex": "037e84547231650e816a32eb5b79028e71ac7459bbcd8e81e6697ac9022e64a407"
    }
  ],
  "authentication": [
    {
      "type": "Secp256k1SignatureAuthentication2018",
      "publicKey": "did:infra:local:PUB_K1_7nxEa8qHEiy34dpuYH4yE2zRWaAoeT1gsdTnh8n5ikapZZrzjx#controller"
    }
  ],
  "service": [
    {
      "type": "MessagingService",
      "serviceEndpoint": "https://infradid.com/pk/3/mysvcr4"
    }
  ]
}
```

