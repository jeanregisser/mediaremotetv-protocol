# Game Controller

After sending the [ready state](Ready.md) message, the Apple TV sends back a `RegisterForGameControllerEventsMessage`.

[include](../protobuf/RegisterForGameControllerEventsMessage.proto)

> **SERVER -> CLIENT**
```txt
type: REGISTER_FOR_GAME_CONTROLLER_EVENTS_MESSAGE
priority: 0
[registerForGameControllerEventsMessage] {
  inputModeFlags: 0
}
```
