# Boot to RootMethodology and Strategy

Here I am trying to set a list of activities that you need to achive root access on a **Boot to Root** Machine

## Step zero (if the machine is attached to your virtual envinroment)

1- you need to get sure that the **target machine** is in the same network range of your **attack machine** ( I brefer to setup both machine is a NAT mode
)

2- identiefy the IP address of the target machine
you can use netdiscover OR arp-scan

```
sudo netdiscover -r [network name]/[subnet mask]
```
netdiscover command requires root access thats why we use *sudo*

**-r** is to spacify the network range to scan

**network name/subnet mask** this would be something like 192.168.1.0/24

```
sudo arp-scan [network name]/[subnet mask]
```

arp-scan command requires root access thats why we use *sudo*

**network name/subnet mask** this would be something like 192.168.1.0/24


