# Communication

MediaRemoteTV is a TCP based protocol which uses length prefixed protobuf encoded messages to communicate between the client and server.

The Apple TV acts as the server and clients can connect to it in order to issue various commands (playback, keyboard, voice, game controller, etc).

All messages are encrypted after negotiating the [pairing](../pairing/README.md) between the client and the Apple TV.

## Protocol Message {#protocol-message}

Each message exchanged between the Apple TV and the remote client is wrapped in a common envelope which contains the type of the message among other fields.

[include](../protobuf/ProtocolMessage.proto)

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


