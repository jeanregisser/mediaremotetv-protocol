# Communication

MediaRemoteTV is a TCP based protocol which uses length prefixed protobuf encoded messages to communicate between the client and server. The prefixed length is encoded as a Google Protobuf [base 128 variant](https://developers.google.com/protocol-buffers/docs/encoding#varints).

The Apple TV acts as the server and clients can connect to it in order to issue various commands (playback, keyboard, voice, game controller, etc).

All messages are encrypted after negotiating the [pairing](../pairing/README.md) between the client and the Apple TV.

## Protocol Message {#protocol-message}

Each message exchanged between the Apple TV and the remote client is wrapped in a common envelope which contains the type of the message among other fields.

[include](../protobuf/ProtocolMessage.proto)

### Identifiers {#protocol-message-identifier}

As MediaRemoteTV is an asynchronous protocol, identifiers are used to map a responses to requests. By setting `identifier` in the envelope message (`ProtocolMessage`) to a random UUID4 before sending it, the response will contain the same identifier upon reception.

Not all messages use identifiers, nor does all of them support it. A message will only contain an identifer if it was triggered by a request. No "events" (e.g. status updates about playing media) messages will contain identifiers. Also, some other messages implicitly does not support it, like `CryptoPairingMessage`. In the latter case, no messages of other types can occur simultaneously so identifiers are not needed.

An example of a message using an `identifier` can be seen below: `DeviceInfoMessage`

## Initiating a Connection {#connection}

After [discovering](../discovery/README.md) the MediaRemoteTV service, you open a TCP connection to the discovered port, `49152` on tvOS version 9.2.1.

Once the connection is open, the Apple TV expects a `DeviceInfoMessage` to be sent.

[include](../protobuf/DeviceInfoMessage.proto)

> **CLIENT -> SERVER**
```txt
type: DEVICE_INFO_MESSAGE
identifier: "4D673CA3-03A8-4343-82CF-4FA2B6C0CFF1"
priority: 0
[deviceInfoMessage] {
  uniqueIdentifier: "B8D8678C-9DA9-4D29-9338-5D6B827B8063"
  name: "Jean\'s iPhone"
  localizedModelName: "iPhone"
  systemBuildVersion: "13F69"
  applicationBundleIdentifier: "com.example.myremote"
  applicationBundleVersion: "5"
  protocolVersion: 1
}
```

> **SERVER -> CLIENT**
```txt
type: DEVICE_INFO_MESSAGE
identifier: "4D673CA3-03A8-4343-82CF-4FA2B6C0CFF1"
priority: 0
[deviceInfoMessage] {
  uniqueIdentifier: "B52D1E7B-66FD-4632-8FBB-644614325855"
  name: "Apple TV"
  systemBuildVersion: "13Y825"
  applicationBundleIdentifier: "com.apple.mediaremoted"
  protocolVersion: 1
}
```

This exchange allows the client and server to known whether they've already paired each other.

Then the [pairing](../pairing/README.md) process begins.


