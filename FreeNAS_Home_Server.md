# FreeNAS Home Server

In the spring of 2019 I build a home server for backup and media streaming tasks. Hardware was chosen for low power, low noise, and reliable operation. [FreeNAS](https://www.freenas.org/) was the operating system of choice because of its ZFS file system features and proven track record.

The server was set up with a ZFS mirror on 2x 4TB WD Red hard drives for data retention in case of a hardware failure, resulting in an effective capacity of just under 4TB. A mirrored boot device pool was created using 2x 16GB memory sticks connected to an internal USB3.0 adapter, this way one of the boot devices could fail without taking down the system.

The software was configured with one ZFS pool split for backup, jail/VM, and media purposes. Password protected SMB shares were made for media and backup respectively. The following plugins were downloaded:
* [Transmission bittorrent client](https://transmissionbt.com/) jail - Using the web interface to enter magnet links and downloading to the media share. Jail only has access to its own folder, so files need to be copied to the relevant media folders manually.
* [Plex Media Server](https://www.plex.tv/) jail - Plex is a media server that has client apps for browser and mobile devices and allows for organisation and streaming of your own media files. I use the free version mainly in combination with the Android TV app. At first I had remote streaming enabled with port forwarding on my router, but I later disabled this because it was never used.
* [Resilio Sync jail](https://www.resilio.com/) - Resilio Sync uses bittorrent sync to synchronize files between devices, I use this in combination with Android and iOS apps to backup photos and other files from phones to a separate folder on the backup share. The Android version of this works a lot better than the iOS version, as it has access to all files on the device and is able to automatically run in the background, keeping backups up to date. The iOS version can only backup photos and needs to be run manually periodically.

In addition to the backup and media serving capabilities of this home server, I created a minimal virtual machine (1 CPU, 512MB RAM) running Debian Server and the [PiHole](https://pi-hole.net/) service. This acts as a DNS server on the network and blocks ad serving hostnames based on a community blacklist. This was there is no local overhead when browsing a webpage with ads as with a traditional adblocker and this has the advantage of working on all devices on the network. However, because it only blocks ads, often the containers to serve the ads will pop-up, albeit without flashy content.

## Experiences so far
Having used this home server for almost a year now I have a number of experiences I want to share and things I would change if I were to start over, which I might do at some point.

Firstly, I have found no issues with the chosen hardware. One of the boot memory sticks failed but upon receiving an email notification I was able to replace the stick with one of identical volume and the filesystem was resilvered (rebuilt) without downtime. If I had to buy parts all over I would have looked for a motherboard with a larger PCIe expansion slot, the current one only has a PCIe 1x slot, however, a PCIe 4x or more would be benefitial for installing an SSD or other high bandwidth peripheral.

Secondly, at some point I want to replace Plex Media Server because I dislike the lack of transcoding in the free version and the lack of disabling CPU encoding at all. With CPU encoding enabled and starting playback of a high-bitrate file the system will hang while trying to transcode because of the underpowered CPU, making matters worse is the inability to force disable transcoding or even change it away from the default setting. In the case of switching I would try [Jellyfin](https://jellyfin.org/), a fork of Emby, where all features are available for free including hardware transcoding.

## Future upgrades and improvements
* Add an offsite backup for improved data security. (Currently in progress)
* Switch Plex to Jellyfin.
* Setup DNS over HTTPS on PiHole for more secure browsing.
* Add SSD for VM/Jails IO to reduce constant HDD use.
* Switch to Linux based OS for more available software, for example Proxmox.

## Parts used
Hardware:
* ASRock J4205-ITX (Intel Pentium J4205 onboard)
* 8GB DDR3 SODIMM 1600MHz (x2)
* WD Red 4TB (x2)
* Fractal Design Node 304
* be quiet! Pure Power 11 400W CM
* 19 pin to double type A USB3.0 internal adapter
* Generic 16GB USB3.0 Memory Stick (x2)

Software:
* FreeNAS 11.2
* Transmission Client jail plugin
* Plex Media Server jail plugin
* Resilio Sync plugin
* PiHole on a Debian Server VM
