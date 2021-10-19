---
description: Quick Start For Wallets Using Swift Client (iOS)
---

# Swift Client (iOS)

Swift implementation of WalletConnect v.2 protocol for native iOS applications.

:::caution
Note: The Swift client is in Alpha and should only be used for testing.
:::

## Requirements

* iOS 13
* XCode 13
* Swift 5

## Usage

#### Instantiate Client

```swift
let url = URL(string: "wss://relay.walletconnect.com")!
let options = WalletClientOptions(apiKey: String, name: String, isController: true, metadata: AppMetadata(name: String?, description: String?, url: String?, icons: [String]?), relayURL: url)
let client = WalletConnectClient(options: options)
```

#### Pair Clients

Pair client with a uri generated by the dapp.

```swift
let uri = "wc:..."
try! client.pair(uri: uri)
```

#### Approve Session

Sessions are always proposed by the `Proposer` client so `Responder` client needs either reject or approve a session proposal.

```swift
class ClientDelegate: WalletConnectClientDelegate {
...
    func didReceive(sessionProposal: SessionType.Proposal) {
        client.approve(proposal: proposal)
    }
...
```

or

```swift
    func didReceive(sessionProposal: SessionType.Proposal) {
        client.reject(proposal: proposal, reason: Reason)
    }
```

#### Handle Delegate methods

```swift
    func didSettle(session: SessionType.Settled) {
        // handle settled session
    }
    func didReceive(sessionProposal: SessionType.Proposal) {
        // handle session proposal
    }
    func didReceive(sessionRequest: SessionRequest) {
        // handle session request
    }
```

#### JSON-RPC Payloads

#### Receive
 You can parse JSON-RPC Requests received from "Requester" in `didReceive(sessionRequest: SessionRequest)` delegate function.

 Request parameters can be type casted based on request method as below:
 ```Swift
             let params = try! sessionRequest.request.params.get([EthSendTransaction].self)
 ```
 ##### Respond

 ```Swift
             let jsonrpcResponse = JSONRPCResponse<AnyCodable>(id: request.id, result: AnyCodable(responseParams))
             client.respond(topic: sessionRequest.topic, response: jsonrpcResponse)
 ```

## API Keys

For api keys look at [API Keys](../../api/api-keys.md).