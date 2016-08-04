# Keyboard

The remote client first retrieves the keyboard session.

[include](../protobuf/GetKeyboardSessionMessage.proto)

> **CLIENT -> SERVER**
```txt
type: GET_KEYBOARD_SESSION_MESSAGE
identifier: "C3869A52-F9FC-4EBC-899E-8A4F04F7150A"
priority: 0
‏[getKeyboardSessionMessage]: ""
```

The Apple TV answers with a `KeyboardMessage`

[include](../protobuf/KeyboardMessage.proto)
[include](../protobuf/TextEditingAttributes.proto)
[include](../protobuf/TextInputTraits.proto)

> **SERVER -> CLIENT**
```txt
```