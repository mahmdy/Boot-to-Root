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

# Step TWO Web Enummeration process:

In most of the Boot to Root machines we will find web service enabled ethier on the regural port 80 or any other port
you need to start by browsing the web server it self

A very **IMP** note: ***check the source code of the web pages***, its a treasure.

## Web directory brute-forcing:

There are a lot of tools used for directory broute-forcing, to the date I am using dirb, and ffuf (if i staretd to use others I will update this section)

**dirb**

dirb is very easy to use:

```
dirb http://[IP]
```
dirb has its defuilt wordlist that it uses, but incase you want to use diffrent wordlist

```
dirb http://[IP] [the full apth of your wordlist]
```

if the web service is running on other port than port 80

```
dirb http://[IP]:[port number]
```

in case you want to look for the files under a specific directory you found

```
dirb http://[IP]/[Directory Name]
```

some times you would be intersted in finding files under specific extention/s

```
dirb http://[IP] -X .[ext1],.[ext2],.....
```

**fuff**

Although **dirb** is easy to use but tools like **ffuf* would be more usefule and efficent

```
ffuf -w [bath for your wordlist] -u http://[IP]/FUZZ
```

using **ffuf** would just be like **dirb** when we are spacifing the port number, or the directorty we want to brute-force, bt for file extensions we will use **-e** insted of **-X** we used with **dirb**

```
ffuf -w [bath for your wordlist] -u http://[IP]/FUZZ -e .[ext1],.[ext2],....
```

we can use **/** after the **-e** to include directory with file extensions

ffuf also provide fiture for subdomain brute-forcing

```
ffuf -w [path for the dubdomain wordlist] -u http://[IP] -H "Host: FUZZ.[IP]" -fs 0
```

again:

**-w** this is the wordlist path

**-u** stands for URL, which is the target IP

**-H** for the subdomains discovery

**-fs 0** filters responces with error to show only the found subdomains

**hint:** sometimes it is usefule to address the target machine by its name not the ip address, to add the target machine into your hosts file

```
echo [IP] [machine name] |sudo tee -a /etc/hosts
```

## web vulnarability scanning

for the vulnarability scanning we will use **nikto**, or **nmap vulnarability scanning scripts**

**nikto**

```
nikto -h http://[IP]
```

nikto can scan also https


```
nikto -h https://[IP]
```

or any other specific port, also we can scan for vulnarabilities under sub driectories just as how we did it in directory brute-forcing

**nmap**

```
nmap [ip] -sC --script=vuln
```

**-sC** script scanning

you ca use **-sC** only for defuilt nmap scripts

or you can spacify a spacific script to run from the **nmap** scripts avilable in ***/usr/share/nmap/scripts***
