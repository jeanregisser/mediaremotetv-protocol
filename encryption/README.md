# Decryption Example {#decryption-example}

This example assumes the inputKey derived from the [pairing](../pairing/README.md) process is `7bd32320 3a26f003 05d76353 6ef3bfd0 01edc6fe 602a0c81 526f2a11 4880ac12` (32 bytes) and the current inputNonce is `00000000 0b000000 00000000` (12 bytes).

The nonce is 0000000 + counter (little endian order) which must be incremented each time we decrypt something.
A different counter is used for encryption.

Original encrypted data received over the tcp connection:

> **SERVER -> CLIENT**

```txt
19bbc5d6 67362344 a13ebbf0 c7b4e2aa 03029710 0f0f3b33 fbe3
```

It's prefixed with its length as a [protobuf base 128 variant](https://developers.google.com/protocol-buffers/docs/encoding#varints), 0x19 so 25 bytes.
So the encrypted payload to decrypt is:

```txt
bbc5d667 362344a1 3ebbf0c7 b4e2aa03 0297100f 0f3b33fb e3
```

Using the python [cryptography](https://cryptography.io/en/latest/hazmat/primitives/aead/#cryptography.hazmat.primitives.ciphers.aead.ChaCha20Poly1305) ChaCha20Poly1305 module we can now decrypt it:

```txt
> from cryptography.hazmat.primitives.ciphers.aead import ChaCha20Poly1305
> data = filter(str.isalnum, "bbc5d667 362344a1 3ebbf0c7 b4e2aa03 0297100f 0f3b33fb e3").decode('hex')
> inputKey = filter(str.isalnum, "7bd32320 3a26f003 05d76353 6ef3bfd0 01edc6fe 602a0c81 526f2a11 4880ac12").decode('hex')
> inputNonce = filter(str.isalnum, "00000000 0b000000 00000000").decode('hex')
> chacha = ChaCha20Poly1305(inputKey)
> decryptedData = chacha.decrypt(inputNonce, data, None)
> decryptedData.encode('hex')
'08112000b201020800'
```

Then from the shell:

```txt
echo -n "08112000b201020800" | xxd -r -p | protoc --decode_raw
1: 17
4: 0
22 {
  1: 0
}
```

