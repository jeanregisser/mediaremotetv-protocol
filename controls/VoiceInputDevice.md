To be able to send voice commands, the client first sends a `RegisterVoiceInputDeviceMessage`.

[include](../protobuf/RegisterVoiceInputDeviceMessage.proto)
[include](../protobuf/VoiceInputDeviceDescriptor.proto)
[include](../protobuf/AudioFormatSettings.proto)

> **CLIENT -> SERVER**
```txt
type: REGISTER_VOICE_INPUT_DEVICE_MESSAGE
identifier: "1946D664-A029-45E3-B954-84FA07BDEC71"
priority: 0
33 {
  1 {
    1 {
      1: "bplist00\323\001\002\003\004\005\006_\020\017AVSampleRateKey]AVFormatIDKey_\020\025AVNumberOfChannelsKey#@\317@\000\000\000\000\000\022opus\020\001\010\017!/GPU\000\000\000\000\000\000\001\001\000\000\000\000\000\000\000\007\000\000\000\000\000\000\000\000\000\000\000\000\000\000\000W"
    }
    2 {
      1: "bplist00\323\001\002\003\004\005\006_\020\017AVSampleRateKey]AVFormatIDKey_\020\025AVNumberOfChannelsKey#@\317@\000\000\000\000\000\022opus\020\001\010\017!/GPU\000\000\000\000\000\000\001\001\000\000\000\000\000\000\000\007\000\000\000\000\000\000\000\000\000\000\000\000\000\000\000W"
    }
  }
}
```

The Apple TV answers with a `RegisterVoiceInputDeviceResponseMessage`

[include](../protobuf/RegisterVoiceInputDeviceResponseMessage.proto)

> **SERVER -> CLIENT**
```txt
type: REGISTER_VOICE_INPUT_DEVICE_RESPONSE_MESSAGE
identifier: "1946D664-A029-45E3-B954-84FA07BDEC71"
priority: 0
[registerVoiceInputDeviceResponseMessage] {
  deviceID: 5
  errorCode: 0
}
```