# On box network programmability

In this lab guide we'll be exploring some of the options we've got for On box programmability with IOS-XE. In this guide we'll primarily focus on using Guestshell. Guestshell is a virtualized Linux-based environment, designed to run custom Linux applications, including Python for automated control and management of Cisco devices. It also includes the automated provisioning (Day zero) of systems. This container shell provides a secure environment, decoupled from the host device, in which users can install scripts or software packages and run them.
This module describes Guest Shell and how to enable it. This container shell provides a secure environment, decoupled from the host device, in which users can install scripts or software packages and run them.
This module describes what Guest Shell is and how to get started with using it.

## Step 0 - Enabling guestshell

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

## Further examples

The following guide has some example Python scripts for On-box network programmability.  

https://github.com/CiscoDevNet/python_code_samples_network
