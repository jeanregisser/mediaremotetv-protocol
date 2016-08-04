# Notifications

`NotificationMessage` are sent by the Apple TV once the [client updates config](ClientUpdatesConfig.md) has been sent.

[include](../protobuf/NotificationMessage.proto)

> **SERVER -> CLIENT**
```txt
type: NOTIFICATION_MESSAGE
priority: 0
[notificationMessage] {
  notification: "kMRTelevisionRemoteNowPlayingArtworkChanged"
}
```
