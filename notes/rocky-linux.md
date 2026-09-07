# Born2beroot - Rocky Linux

## [Rocky Linux](https://rockylinux.org/)
[Rocky Linux - Wikipedia](https://en.wikipedia.org/wiki/Rocky_Linux)

- free and open source downstream release of [Red Hat Enterprise Linux
  (RHEL)](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux)
- has SELinux (Security Enhanced Linux)


### [SELinux](https://selinuxproject.github.io/)

[Security-Enhanced Linux for mere mortals - YouTube](https://youtu.be/_WOKRaM-HI4)

**SELinux** is a Linux kernel security module (LSM) that provides flexible
Mandatory Access Control (MAC) for Linux.

- created by the NSA as a set of patches to the Linux kernel using Linux
  Security Modules (LSM) --> released under GNU GPL in 2000
- adopted by the upstream Linux kernel in 2003
- integrated into a wide variety of Linux distributions and into Android


#### DAC vs. MAC

##### DAC - discretionary access control

Type of access control that restricts access to objects based on the identity of
subjects and/or groups. The owner of a resource has the ability to change its
permissions. Historically, Linux and Unix systems have had DAC.

- users can execute `chmod +rwx` on their home directory if they want; nothing
  will prevent other users or processes from accessing all contents of their
  home directory
- the `root` user is omnipotent


##### MAC - mandatory access control

On a system with **mandatory access control**, access is limited by
administratively set and fixed policy.

- if a user changes DAC permissions on a sensitive file, MAC will prevent
  access to it if there's a policy in place for that effect
- MAC policies can be very fine grained. Policies can be set to determine access
  between: users, files, directories, memory, sockets, tcp/udp ports, etc...

**SELinux** provides mandatory access control.

###### Types of MAC policy

In RHEL, you'll generally see:

- 'targeted' -- default policy
    - only protects the resources explicitly targeted
- 'mls' -- multi-level/multi-category security
    - can be very complex
    - commonly used in government organizations
