# Client Updates Config

The remote client sends a `ClientUpdatesConfigMessage` to tell the Apple TV that it wants to receive specific updates.

[include](../protobuf/ClientUpdatesConfigMessage.proto)

> **CLIENT -> SERVER**
```txt
type: CLIENT_UPDATES_CONFIG_MESSAGE
priority: 0
[clientUpdatesConfigMessage] {
  artworkUpdates: true
  nowPlayingUpdates: true
  volumeUpdates: false
  keyboardUpdates: true
}
```
