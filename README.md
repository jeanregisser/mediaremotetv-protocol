# Introduction

This document describes the MediaRemoteTV protocol used by the [Apple TV remote app](https://itunes.apple.com/us/app/apple-tv-remote/id1096834193?mt=8) version 1.0 released on August 1, 2016.

This protocol allows the following:
- Navigate the Apple TV
- Send keyboard commands
- Send voice commands
- Send game controller commands
- Control playback

This is still very much a work in progress, ideas and contributions are very welcome.

The main goal of this document is to make it possible to write Apple TV remote apps for other platforms (Android, Windows, etc) or write integrations within Home Automation systems.

All this information has been gathered by using various techniques of reverse engineering, so it might be somewhat inaccurate and incomplete. Moreover, this document does not explain how to circumvent any kind of security implemented by Apple:

* It does not give any RSA keys.
* It does not explain how to decode iTunes videos protected with the FairPlay DRM.
* It does not explain the FairPlay authentication (SAPv2.5) used by iOS devices and OS X Mountain Lion to protect audio and screen content.

Please don’t e-mail me about this, I won’t reply. In fact, none of this is actually required to be able to control the Apple TV.

## Content

* [Service Discovery](discovery/README.md)
* [Communication](communication/README.md)
* [Pairing](pairing/README.md)
* [Encryption](encryption/README.md)
* [Controls](controls/README.md)

## Thanks

The pairing and encryption details wouldn't have been possible without the prior work done on reverse engineering the HomeKit protocol
- https://github.com/KhaosT/HAP-NodeJS

Website inspiration:
- https://nto.github.io/AirPlay.html
- http://redux.js.org/

