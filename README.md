# Boot to Root Methodology and Strategy

Here I am trying to set a list of activities that you need; to acheive root access on a **Boot to Root** Machine

## Step ZERO (if the machine is attached to your virtual envinroment):

1- you need to make sure that the **target machine** is in the same network range of your **attack machine** ( I prefer to setup both machines is a NAT mode
)

2- identify the IP address of the target machine
you can use **netdiscover** OR **arp-scan**

```
sudo netdiscover -r [network name]/[subnet mask]
```
netdiscover command requires root access that's why we use *sudo*

**-r** is to specify the network range to scan

**network name/subnet mask** this would be something like 192.168.1.0/24

```
sudo arp-scan [network name]/[subnet mask]
```

arp-scan command requires root access thats why we use *sudo*

**network name/subnet mask** this would be something like 192.168.1.0/24

the output of the scan would contain more than
one IP address, regularly the host machine of your virtual envinroment IP address, the VMWare DNS server if you use VMWare, the multicast IP address of your virtual network, and the target machine IP address 

![image](https://github.com/user-attachments/assets/62fee9f7-2230-4901-8b1d-d79050937594)


## Step ONE (finding the **IP address** of the **target Machine**):

we can use both **nmap** or **rustscan** ( rustscan is conisderably faster than nmap)

### basic nmap scan

```
nmap [IP]

```
**IP** is the IP address of the *target Machine*

### rustscan

*note:* you can get **ruscsnan** from 

```
rustscan -a [IP]
```

**IP** is the IP address of the *target Machine*

both scans would return the open ports of the Top known 1000 ports, however if we need to look for all the ports

```
nmap [IP] -p-
```
**-p-** scans for all the *65,535* ports 

doing the same with rustscan 

```
rustscan -a [IP] --range 0-65535
```

after we get knowledge about the open ports on the target machine, we would need to learn what is the version of software running the  service on these open ports

```
nmap [IP] -p [port number / port numbers] -sV
```

**-p** used to specify the port / ports under scan

**-sV** the -s is for specifing the scan type, and -V is for **Version** identification

this is the basic network enummeration.

# Web Enummeration process:

In most of the Boot to Root machines we will find web service enabled ethier on the regural port 80 or any other port
you need to start by browsing the web server it self
