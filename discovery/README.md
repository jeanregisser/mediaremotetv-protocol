# Service Discovery

MediaRemoteTV does not require any configuration to be able to find compatible devices on the network, thanks to DNS-based service discovery, based on multicast DNS, aka Bonjour.

> MediaRemoteTV service
```txt
name: 
type: _mediaremotetv._tcp
port: 49152
```

*Note: The port is randomized every time a device is rebooted. In fact, a device might even change port at any given time, so it is not possible to assume that a specific port is used.*