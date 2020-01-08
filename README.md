# On box network programmability

In this lab guide we'll be exploring some of the options we've got for On-box programmability with IOS-XE. In this guide we'll primarily focus on using Guestshell. Guestshell is a virtualized Linux-based environment, designed to run custom Linux applications, including Python for automated control and management of Cisco devices. It also includes the automated provisioning (Day zero) of systems. This container shell provides a secure environment, decoupled from the host device, in which users can install scripts or software packages and run them independant from IOS.

## Prerequisites

Before we get started with guestshell we'll need a test environment, one of the easiest test environments you'll find is on the Cisco DevNet Sandbox which has multiple options. These are completely free and can in some cases be accessed within seconds. https://developer.cisco.com/docs/sandbox/#!overview/all-networking-sandboxes

Please note you are free to use this with your own hardware or test environment. However the commands in this lab guide have been tested for the sandboxes they correspond to. For this lab guide we will be using the reservable IOS XE on CSR Recommended Code Sandbox which can be found on the Sandbox catalogue https://devnetsandbox.cisco.com/RM/Topology

**It's important to note that support for Guestshell and the resources available varies depending on the hardware platform and the software version running on the device. Please consult release documentation for up to date information about the level of the guestshell support your platform has.**

## Exercise 0 - Enabling guestshell

Cisco IOx is an end-to-end application enablement platform that provides application hosting capabilities. For more details, see here. The CLI to enable the IOx application framework is displayed below:

```
iox 
```
 
Is it really that easy? Only a single command? Starting with IOS XE 16.8 release you need to configure the Guestshell virtual interface as well:

``` 
app-hosting appid guestshell
 vnic management guest-interface 0
```
  
You might be wondering "why do I need that??". This change will make more sense when GuestShell will have the option to access both the management port and front panel data ports. Stay tuned.
exec

Guestshell is enabled with an exec command:

``` 
guestshell enable 
```
This might take a minute or so be patient, it's spinning up a container. Here's an example:

Management Interface will be selected if configured
Please wait for completion

You'll see this when it finishes:
```
 Guestshell enabled successfully
```
Management Interface will be selected if configured
Please wait for completion

OK, now it's time to drop into GuestShell. Here's an example:

```
cat9k#guestshell
[guestshell@guestshell ~]$
```

## Exercise 1 - Running a simple python script with the CLI library

In this first exercise we'll keep it as simple as it gets we'll use a Python module called CLI, which from a Guestshell envrironment allows you to excecute and read cli commands on the host device. 


```
cat9k#guestshell
[guestshell@guestshell ~]$
```


## Further examples

The following repo has some example Python scripts for On-box network programmability. After these initial exercises you may want to explore these more to get a better understanding of some of the capabiltiies available to you from Guestshell

https://github.com/CiscoDevNet/python_code_samples_network
