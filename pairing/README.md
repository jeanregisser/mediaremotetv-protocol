# Pairing

The pairing process uses the [SRP protocol](https://en.wikipedia.org/wiki/Secure_Remote_Password_protocol). The actual process is more or less the same as the one used by HomeKit, so the [HomeKit API reference](https://developer.apple.com/support/homekit-accessory-protocol/) released by Apple is a good read when understading the process.

It uses `CryptoPairingMessage`.

[include](../protobuf/CryptoPairingMessage.proto)

`pairingData` contains [TLV8](https://en.wikipedia.org/wiki/Type-length-value) encoded data.

## Differences Compared to HomeKit

When deriving new encryption keys, use the following HKDF-parameters:

* Salt: MediaRemote-Salt
* Data: MediaRemote-Write-Encryption-Key

When deriving new decryption keys, use the following HKDF-parameters:

* Salt: MediaRemote-Salt
* Data: MediaRemote-Read-Encryption-Key

## Pairing with a New Apple TV {#new}

> **CLIENT -> SERVER**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: "\000\001\000\006\001\001"
  status: 0
}
```

>> *Decoded TLV*

```txt
0: 0x00 // METHOD
6: 0x01 // SEQUENCE_NUM
```

> **SERVER -> CLIENT**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: "\006\001\002\002\0204_\017\223\261\266\223l\004x\nm\356Jlw\003\377\265rK\301\017\'\227\374\"yUG\321\253mB\213\233\226\212\353\003C\225\302}[qQ\350\376A\262\251\367@\356\246\037\304\305g\020\315\231Z\306-\377\016\200\324>\010f\t\'\305\200<m\334V\\\206\345#\276T\310\316\322\034\340C\215\265\326F\0344\321\225\010\354\342\276!\320\311\276\200G\021.\352O\357)\'\277I\205\272^s\0330\035\267U\377\233MpF\213\000\235\244\006\277\255\t\227\273\000VX\370f\341i\357,D\202\234\213D\377\247\217Y\266T\223\\\302\322T\352\377\ng|\315\357\354\341c\3267\366\277\0021v]w\364\247(\362aM\326\230-\277.\314=n:\275\004X\006q\025\332XZ/\271\273\333\334g\2523}l\006\276\031\322\002UD\252\252\3426\264\030\177\271\3160-2\346\214\352\262I\267\317\025x\322!\3416\267\\\314\010Z\204\317\260m\r\334\340\367\352\205\233\3509\224\003\201>\371]\027\200\266\235\330\212\256m\315\253W\234\'V\313\357\365D2\370\227(\272\014P\364\010\023b\016z\207\027\270\213\266G\314\nFU\226)\265=\345\257\246\274L\036u-\326u\326O\233`\303\254\026\304L\003I\223\247\\1`\243\231:C\221\005\\E\355\347\3079/}\033\237\353\357Sn\263)\237r8\020\260.\177\217|\246\260\036/\331\251\254\261\006\232\223\317\374y\030c\314>Y\217:\200\277\336"
  status: 0
}
```

>> *Decoded TLV*

```txt
2: bytes[16] // SALT
3: bytes[384] // PUBLIC_KEY
6: 0x02 // SEQUENCE_NUM
```

At this point a 4 digits PIN is displayed on the Apple TV.
The client uses it to derive the password proof in the next message.

> **CLIENT -> SERVER**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: "\006\001\003\003\377\327\200\354\213\314)\241\257\240V\357L\027\361\356\017\256\244!\016\r\364\'U\217C\nP[Hx@\221\316\225\353\311\2676Sw\235\034\374\347O\235\333\234ZS\037\214\372\3542\374\262r\244\003\345| \204\017\024\331\351\364j\361\340\237\2771\001^*V4\377\013\216\251\207\221\260\017\027\027\276\252x)\333\034\327rD\345\366\t\312\370%\256\237X\006\340]\271d\004\244\377\371;\256H\207\2537y[\364N#W|\367\034\240A5!\033B\336\227\203\001\203sf:<\365\272\263t\204P\022p\2045r\236u\035i+\316\242\004T\023\217\373\214\316\232j\315#\220@H\000o\t\304\365\263\230B\266\204ns\213\220\320\222\021I\016\334%\031\252y\206\347\236\370\0162\215/U\312ATb\324W=\253\267(\207\270\342\356\304\244\344%\030\035\002\376j\314\321\'O\227D\330Z\264g\016\315:\037\232\033\350-}\003\201\330\353eC\250\342\241\365\273\345\010k\356T\343\210)\307$\376\347]C\031]\177x\201\204\203\031\020A<\333\220\006\355t=\272\375|\033z]\3706\206Q\225\263\254\033PT\347\370t\352\215\351\215\320SR\374w\250\227U\004\317\320\210W\000\352Q\250\302\300\317Mu!\025\352\271}/\214\303\352\342\247gL\372\\Q\362W2\020\312X\321\n\253;?\310\366b-\226\205\230\031\303\027\370\223zv\235\006\017\004@\360\251\213\336\315?\n`9\315\01070?r\000\275\3236|\032\326\265\365\225\325R\320\\Q\246\227\344\216\374\t7._\375\005\224\213\253k\323\266\214\223\353^\204\337\211`+\215\334tF \352\232\211"
  status: 0
}
```

>> *Decoded TLV*

```txt
3: bytes[384] // PUBLIC_KEY
4: bytes[64] // PASSWORD_PROOF
6: 0x03 // SEQUENCE_NUM
```

> **SERVER -> CLIENT**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: ??
  status: 0
}
```

>> *Decoded TLV*

```txt
4: bytes[64] // PASSWORD_PROOF
6: 0x04 // SEQUENCE_NUM
```

> **CLIENT -> SERVER**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: ??
  status: 0
}
```

>> *Decoded TLV*

```txt
5: bytes[154] // ENCRYPTED_DATA
6: 0x05 // SEQUENCE_NUM
```

> **SERVER -> CLIENT**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: ??
  status: 0
}
```

>> *Decoded TLV*

```txt
5: bytes[154] // ENCRYPTED_DATA
6: 0x06 // SEQUENCE_NUM
```

Client then starts verifying the pairing.

## Verifying Existing Pairing {#existing}

> **CLIENT -> SERVER**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: ??
  status: 0
}
```

>> *Decoded TLV*

```txt
3: bytes[32] // PUBLIC_KEY
6: 0x01 // SEQUENCE_NUM
```

> **SERVER -> CLIENT**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: ??
  status: 0
}
```

>> *Decoded TLV*

```txt
3: bytes[32] // PUBLIC_KEY
5: bytes[120] // ENCRYPTED_DATA
6: 0x02 // SEQUENCE_NUM
```

> **CLIENT -> SERVER**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: ??
  status: 0
}
```

>> *Decoded TLV*

```txt
5: bytes[120] // ENCRYPTED_DATA
6: 0x03 // SEQUENCE_NUM
```

> **SERVER -> CLIENT**

```txt
type: CRYPTO_PAIRING_MESSAGE
priority: 0
[cryptoPairingMessage] {
  pairingData: "\006\001\004"
  status: 0
}
```

>> *Decoded TLV*

```txt
6: 0x04 // SEQUENCE_NUM
```

Once pairing is successful, all subsequent messages are encrypted using ChaCha20-Poly1305.

## Outcome of Pairing

After pairing has succeeded each side (Apple TV and Remote app) will have the following data:

* Long-Term-Signing-Key (LTSK) - 32 byte
* Long-Term-Public-Key (LTPK) - 32 byte
* Apple TV Identifier - 36 byte
* iOS Identifier - 36 byte

The keys (LTSK and LTPK) are generated per-device, meaning they are different on each device. LTSK is never publically exchanged. These parameters must be persistently stored and are used to derive a "shared secret" (=encryption key) for new sessions without having to pair again. If they are lost, the pairing process must be performed again.
