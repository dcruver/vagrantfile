# Starting The VMs

This vagrantfile can be used to create both Linux and Windows VMs. You can use it to 
start up to 25 boxes of each OS. 

Examples:

    $ vagrant up linux1
    $ vagrant up linux25

    $ vagrant up windows1
    $ vagrant up windows25
    $ vagrant rdp windows25


# Box Configuration

Each box is given 8G of RAM, 4 CPUs and is headless by default. The windows boxes are
most easily accessed via RDP. Due to a problem with VirtualBox on Linux (and maybe other
OSes as well), the USB port is turned off by default.   

The Linux box is a Debian/Jessie distribution. The Windows box is Windows 2012r2. Each
box is configured with a bridged network adapter which will get its own IP address. This
makes it accessible from anywhere on the netowrk by IP. The Windows firewall is turned off 
by default. The ports on the Linux boxes are all open by default. 


# Provisioning

Each VM will be provisioned with Oracle JDK8, Maven 3.3 and git. The Windows boxes also 
include the chocolatey package manager.


# find-ip

This script will give the IP address of the requested box. This is useful when you need to 
find the IP address (so you can RDP into it, for example) of a box with a dynamically assigned 
IP address.

usage:
    $ ./find-ip <box name>

exmaple:
    $ ./find-ip windows12
    10.0.2.15
    10.101.2.39
