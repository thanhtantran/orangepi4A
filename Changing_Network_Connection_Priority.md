# Changing Connection Priorities Linux
Sometimes easy to use `nm-connection-editor` gui is not available in repos, so CLI option to be used
## Check existing priorities
```
nmcli connection show
NAME                    UUID                                  TYPE       DEVICE          
khan 5G_Plus            15ef0c5f-4ae3-4607-b1d2-a338b2189626  wifi       wlan0           
khan 5G_Plus_5G         3da1190f-bc16-40bf-84a5-18be8f87b483  wifi       wlx90de8017e3ad 
Usama's S23 FE Network  96f0f868-9c0c-427e-818d-fe5dc2292851  bluetooth  --              
Wired connection 1      82b4e4d6-300e-39ca-871d-dc814651e5e4  ethernet   --  
```
if you have `netplan` installed (preinstalled in Armbian) your network names will have some netplan prefix.
```
ukhan@radxa-zero3:~$ nmcli connection show
NAME                                  UUID                                  TYPE      DEVICE
netplan-wlx90de8017e3ad-khan 5G_Plus  c8bfa654-b142-31de-bbc0-6f96819f577d  wifi      wlx90de8017e3ad
netplan-wlx90de80ea18d5-khan          c826ec95-8343-3e5c-adfc-a27c6391c6d6  wifi      wlx90de80ea18d5
tailscale0                            8875de67-3fcd-4056-8608-afa99045dfa5  tun       tailscale0
br-5ad81b4dfa83                       83b35aee-cf1d-4346-a36c-c3c18318c9d7  bridge    br-5ad81b4dfa83
docker0                               28ce79ab-40b1-466e-8c6b-34f2e393d2c1  bridge    docker0
lo                                    6eeccb25-b4f5-4cba-a137-bdc9139453ef  loopback  lo
netplan-end1                          e659fd80-ec36-30f4-974a-2c16fc6cc0bc  ethernet  --
```
## Modifying priority
Modifying via `nmcli` is [less preferred](https://superuser.com/a/1858280)
Most likely its preinstalled if not `sudo apt install ifmetric`
The interface with **lower metric** is preferred for Internet.
The following file has a problem. There are duplicate `metric` of 200
```
ukhan@radxa-zero3:~$ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.1.1     0.0.0.0         UG    200    0        0 wlx90de8017e3ad
0.0.0.0         192.168.1.1     0.0.0.0         UG    200    0        0 wlx90de80ea18d5
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
172.18.0.0      0.0.0.0         255.255.0.0     U     0      0        0 br-5ad81b4dfa83
192.168.1.0     0.0.0.0         255.255.255.0   U     102    0        0 wlx90de8017e3ad
192.168.1.0     0.0.0.0         255.255.255.0   U     601    0        0 wlx90de80ea18d5
```
If such a situation exists `cd /etc/netplan` & verify any duplicate `yaml` files. [More about this](https://superuser.com/a/1469343)
In my case I deleted all except x2 `00-default-use-network-manager.yaml` & `armbian.yaml` I edited the latter to set `metrics` directly (200 & 400) & rebooted. Correct one is as follows
```
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.1.1     0.0.0.0         UG    200    0        0 wlx90de8017e3ad
0.0.0.0         192.168.1.1     0.0.0.0         UG    400    0        0 wlx90de80ea18d5
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
172.18.0.0      0.0.0.0         255.255.0.0     U     0      0        0 br-5ad81b4dfa83
192.168.1.0     0.0.0.0         255.255.255.0   U     600    0        0 wlx90de80ea18d5
192.168.1.0     0.0.0.0         255.255.255.0   U     601    0        0 wlx90de8017e3ad
```
## GUI way
https://superuser.com/a/1701437
