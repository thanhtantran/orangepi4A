# Custom Compiled from OrangePI Build Framework
These `.debs` can be installed as I have included multiple missing features. In the default kernel x3 important features (filesystem support) are missing

1. `cifs` support is added. In default kernel you cannot mount a samba shared directory!
2. `nfs` support is added
3. `binfmt_misc` filesystem support is added ~still missing. So you cannot run `box86` applications. I will latter add it.~
4. VPN service module *tun* is added
5. Wireguard module is added

> https://github.com/defencedog/orangepi3b_v2.1/blob/main/SAMBA_NAS_Videos.md

These kernerls can be used with official `jammy` image v1.0.4

## Update #1
Packages have been updated to resolve bluetooth connectivity issue with mouse / keyboards because of the absence of `UHID` module. Details here https://github.com/defencedog/orangepi4A/blob/main/Bluetooth%20Mouse%20Not%20Working%20Solved.md
## Update #2
`twingate` client is not running! Superficially it appears the problem is because of a [twingate bug](https://help.twingate.com/hc/en-us/articles/21042455954845--Linux-Client-Unable-to-authenticate-on-Ubuntu-24-04) which is also affecting Ubuntu 22. However logs via `sudo journalctl -u twingate --since "1 hour ago"` indicate actual problem is missing kernel module _tun_ as described [here](https://forums.raspberrypi.com/viewtopic.php?t=262317) The module is added & kernel is recompiled, otherwise, persistent error you will get while connecting `twingate` is 
> /dev/net/tun, error: 'No such file or directory'
