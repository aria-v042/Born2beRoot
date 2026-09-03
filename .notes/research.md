# Born2beRoot - Research

## Debian vs Rocky Linux

### Rocky Linux

[Rocky Linux - Wikipedia](https://en.wikipedia.org/wiki/Rocky_Linux)

A free and open source downstream release of [Red Hat Enterprise Linux
(RHEL)](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux)

// ...

## AppArmor vs SELinux

### SELinux

[Security-Enhanced Linux for mere mortals - YouTube](https://youtu.be/_WOKRaM-HI4)

**SELinux** is a Linux kernel security module (LSM) that provides flexible
**Mandatory Access Control (MAC)** for Linux.

- created by the NSA as a set of patches to the Linux kernel using Linux
  Security Modules (LSM) --> released under GNU GPL in 2000
- adopted by the upstream Linux kernel in 2003
- integrated into a wide variety of Linux distributions and into Android

#### Mandatory Access Control (MAC)

With **Discretionary Access Control (DAC)**, access is restricted based on the
identity of users and/or groups alone (rwx permissions) -- This is the Linux and
Unix historical default.

- users can execute `chmod +rwx` on their home directory if they want; nothing
  will prevent other users or processes from accessing all contents of their
  home directory
- the `root` user is omnipotent


**Mandatory Access Control (MAC)** implements the ability to limit access based
on administratively set and fixed policy.

- if a user changes DAC permissions on a sensitive file, MAC will prevent
  access to it if there's a policy in place for that effect
- MAC policies can be very fine grained. Policies can be set to determine access
  between: users, files, directories, memory, sockets, tcp/udp ports, etc...

**SELinux** provides mandatory access control.


// ...

## VirtualBox vs UTM

// ...
