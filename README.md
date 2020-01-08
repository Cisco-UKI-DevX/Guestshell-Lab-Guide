# On box network programmability

In this lab guide we'll be exploring some of the options we've got for On-box programmability with IOS-XE. In this guide we'll primarily focus on using Guestshell. Guestshell is a virtualized Linux-based environment, designed to run custom Linux applications, including Python for automated control and management of Cisco devices. It also includes the automated provisioning (Day zero) of systems. This container shell provides a secure environment, decoupled from the host device, in which users can install scripts or software packages and run them independant from IOS.

## Prerequisites

Before we get started with guestshell we'll need a test environment, one of the easiest test environments you'll find is on the Cisco DevNet Sandbox which has multiple options. These are completely free and can in some cases be accessed within seconds. https://developer.cisco.com/docs/sandbox/#!overview/all-networking-sandboxes

Please note you are free to use this with your own hardware or test environment. However the commands in this lab guide have been tested for the sandboxes they correspond to. For this lab guide we will be using the reservable IOS XE on CSR Recommended Code Sandbox which can be found on the Sandbox catalogue https://devnetsandbox.cisco.com/RM/Topology

**It's important to note that support for Guestshell and the resources available varies depending on the hardware platform and the software version running on the device. Please consult release documentation for up to date information about the level of the guestshell support your platform has.**

## Exercise 0 - Enabling guestshell

Cisco IOx is an end-to-end application enablement platform that provides application hosting capabilities. For more details on iox please refer to the pages on the DevNet site [here](https://developer.cisco.com/docs/ios-xe/#application-hosting-quick-start-guide). The CLI to enable the IOx application framework is displayed below:

```
iox 
```
 
Starting with IOS XE 16.8 release you need to configure the Guestshell virtual interface as well. This isn't specifically required but will make more sense when GuestShell will have the option to access both the management port and front panel data ports. Keep your eyes peeled

``` 
app-hosting appid guestshell
 vnic management guest-interface 0
```

Finally,Guestshell is enabled with an exec command:

``` 
guestshell enable 
```
This might take a minute or so be patient, it's spinning up a container on the device. When it finishes, you'll be met with the following message
```
 Guestshell enabled successfully
```
Management Interface will be selected if configured
Please wait for completion

![](./images/guestshell-1.gif)

OK, now it's time to drop into GuestShell. Use the commands like so below:

```
cat9k#guestshell
[guestshell@guestshell ~]$
```

You'll now see we have full access to a Linux bash shell, this is our environment which we'll use in this lab guide. 

![](./images/guestshell-2.gif)


## Exercise 1 - Running a simple python script with the CLI library

In this first exercise we'll keep it as simple as it gets we'll use a Python module called CLI, which from a Guestshell envrironment allows you to excecute and read cli commands on the host device. First step we need to do is again drop into our guest shell environment like so

```
cat9k#guestshell
[guestshell@guestshell ~]$
```

Now lets take a look at the python script we're going to run which is under the folder 'code' within this repository. Excluding the comments we have two lines I want to quickly examine here, it's quite self explanatory. The first line is for importing the module CLI and the second is to run the command show ip route and print it to the screen. 

```
import cli
print cli.execute('show ip route')

```

Theres a couple of ways we can excecute this on the box. By far the easiest is to start is to enter into the python interpreter on our Guestshell environment and enter the code. 

```
[guestshell@guestshell ~]$ python
Python 2.7.5 (default, Jun 17 2014, 18:11:42) 
[GCC 4.8.2 20140120 (Red Hat 4.8.2-16)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> import cli
>>> print cli.execute('show ip route')
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is 10.10.20.254 to network 0.0.0.0
S*    0.0.0.0/0 [1/0] via 10.10.20.254, GigabitEthernet1
      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.10.20.0/24 is directly connected, GigabitEthernet1
L        10.10.20.48/32 is directly connected, GigabitEthernet1
>>> 
```

Alternatively we could also use a file server to copy the python executable across and run with the command below. Which would again, give the same output as above.

```
[guestshell@guestshell ~]$ python exercise1.py
```

Give yourself a pat on the back, you've just enabled and ran your first script on Guestshell.

As you appreciate, this is a basic example, but as you hopefully appreciate now we can use the flexibility of a language like Python to start to interact with the device On-box in a way we previously couldn't before.

## Further examples

The following repo has some example Python scripts for On-box network programmability. After these initial exercises you may want to explore these more to get a better understanding of some of the capabiltiies available to you from Guestshell

https://github.com/CiscoDevNet/python_code_samples_network
