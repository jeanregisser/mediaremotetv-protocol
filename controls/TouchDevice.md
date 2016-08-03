To be able to send touch events, the client sends a `RegisterHIDDeviceMessage`.

[include](../protobuf/RegisterHIDDeviceMessage.proto)
[include](../protobuf/VirtualTouchDeviceDescriptor.proto)

> **CLIENT -> SERVER**
```txt
type: REGISTER_HID_DEVICE_MESSAGE
identifier: "B368593E-2BF9-4ECC-9016-11DEB3121FA6"
priority: 0
[registerHIDDeviceMessage] {
  deviceDescriptor {
    absolute: false
    integratedDisplay: false
    screenSizeWidth: 320
    screenSizeHeight: 282
  }
}
```

The Apple TV answers with a `RegisterHIDDeviceResultMessage`

[include](../protobuf/RegisterHIDDeviceResultMessage.proto)

> **SERVER -> CLIENT**
```txt
type: REGISTER_HID_DEVICE_RESULT_MESSAGE
identifier: "B368593E-2BF9-4ECC-9016-11DEB3121FA6"
priority: 0
[registerHIDDeviceResultMessage] {
  errorCode: 0
  deviceIdentifier: 9
}
```