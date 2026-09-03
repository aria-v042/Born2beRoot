# Born2beRoot - Subject requirements

"You can do anything you want to do. This is your world."

Create a virtual machine in **VirtualBox** using specific instructions --
  set up an operating system while implementing strict rules

- use **VirtualBox**
- submit a **signature.txt** with the machine's virtual disk signature at
  the root of the repository
- **no snapshots** may exist
- **no graphical interface** allowed

## Mandatory part

### Operating system
---

Latest stable **Debian** or **Rocky**

- You will be asked a few questions about the OS you chose. Know the differences
  between *aptitude* and *apt*, what is *SELinux* or *AppArmor*... -- Understand what you use!

Rocky:

- KDump setup is not required
- SELinux must be running at startup and its config has to be adapted for the
  project's needs

Debian:
- AppArmor must be running at startup


### Partitioning
---

Create at least 2 encrypted partitions using **LVM**

Example of a possible partitioning:

```bash
wil@wil:~$ lsblk
NAME                 MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0     8G  0 disk
├─sda1                 8:1    0   487M  0 part  /boot
├─sda2                 8:2    0     1K  0 part
└─sda5                 8:5    0   7.5G  0 part
  └─sda5_crypt       254:0    0   7.5G  0 crypt 
    ├─wil--vg-root   254:1    0   2.8G  0 lvm   /
    ├─wil--vg-swap_1 254:2    0   976M  0 lvm   [SWAP]
    └─wil--vg-home   254:3    0   3.8G  0 lvm   /home
sr0                   11:0    1  1024M  0 rom
```

> [!NOTE]
> The example shows arbitrary disk sizes.
> You need to determine the appropriate size for each partition to ensure proper
> operation while avoiding unnecessary disk usage.


### SSH
---

An SSH service must be running on port 4242 in your virtual machine. For
security reasons, it must not be possible to connect using SSH as root.

> [!NOTE]
> The use of SSH will be tested during the defense by setting up a new account.
> You must therefore understand how it works.


### Firewall
---

You have to configure your operating system with the UFW firewall (Debian) or
firewalld (Rocky) firewall and leave only port 4242 open in your virtual machine.

Your firewall must be active when you launch your virtual machine.


### Hostname
---

The hostname of your virtual machine must be your login ending with 42 (e.g. wil42).

You will have to modify this hostname during your peer review


### Password policy
---

You have to implement a strong password policy.

To set up a strong password policy, you have to comply with the following
requirements:

- Your password has to expire every 30 days.
- The minimum number of days between password changes must be set to 2.
- The user has to receive a warning message 7 days before their password expires.
- Your password must be at least 10 characters long. It must contain an
  uppercase letter, a lowercase letter, and a number. Also, it must not contain
  more than 3 consecutive identical characters.
- The password must not include the name of the user.

Of course, your root password has to comply with this policy.
- The following rule does not apply to the root password: the password must
  contain at least 7 characters that were not part of the previous password.

> [!WARNING]
> After setting up your configuration files, you will have to change all the
> passwords of the accounts present on the virtual machine, including the root
> account.


### sudo
---

You have to install and configure `sudo` following strict rules.

To set up a strong configuration for your sudo group, you have to comply with
the following requirements:

- Authentication using `sudo` has to be limited to 3 attempts in the event of an
  incorrect password.
- A custom message of your choice has to be displayed if an incorrect password
  is entered when using `sudo`.
- Each action performed with sudo has to be logged, including both inputs and
  outputs. The log file has to be saved in the `/var/log/sudo/` folder.
- The TTY mode has to be enabled for security reasons.
- For security reasons, the paths that can be used by sudo must also be
  restricted. Example:
  `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`


### Users
---

In addition to the root user, a user with your login as the username has to be
present.

This user has to belong to the user42 and sudo groups.

> [!NOTE]
> During your peer review, you will have to create a new user and assign it to a
> group.


### Monitoring script
---

Finally, you have to create a simple script called `monitoring.sh`. It must be
written in `bash`.

At server startup, the script will display the information listed below on all
terminals, and every 10 minutes (take a look at wall). The banner is optional.
No errors should be displayed.

Your script must always be able to display the following information:

- The architecture of your operating system and its kernel version.
- The number of physical processors.
- The number of virtual processors.
- The currently available RAM on your server and its utilization rate as a percentage.
- The currently available storage on your server and its utilization rate as a percentage.
- The current CPU utilization rate as a percentage.
- The date and time of the last reboot.
- Whether LVM is active or not.
- The number of active connections.
- The number of users using the server.
- The IPv4 address of your server and its MAC (Media Access Control) address.
- The number of commands executed with the `sudo` program.

> [!NOTE]
> During your peer review, you will be asked to explain how this script works.
> You will also have to interrupt it without modifying it. Take a look at cron.

This is an example of how the script is expected to work:

```bash
Broadcast message from root@wil (tty1) (Sun Apr 25 15:45:00 2021):

    #Architecture: Linux wil 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64 GNU/Linux
    #Physical CPU: 1
    #vCPU: 1
    #Memory Usage: 74/987MB (7.50%)
    #Disk Usage: 1009/2Gb (49%)
    #CPU load: 6.7%
    #Last boot: 2021-04-25 14:45
    #LVM use: yes
    #TCP Connections: 1 ESTABLISHED
    #User log: 1
    #Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
    #Sudo: 42 cmd
```


### Check requirements
---

Below are two commands you can use to check some of the subject’s requirements:

For **Rocky**:

```bash
[root@wil wil]# head -n 2 /etc/os-release
NAME="Rocky Linux"
VERSION="8.7 (Green Obsidian)"
[root@wil wil]# sestatus
SELinux status:                enabled
SELinuxfs mount:               /sys/fs/selinux
SELinux root directory:        /etc/selinux
Loaded policy name:            targeted
Current mode:                  enforcing
Mode from config file:         enforcing
Policy MLS status:             enabled
Policy deny_unknown status:    allowed
Memory protection checking:    actual (secure)
Max kernel policy version:     33
[root@wil wil]# ss -tunlp
Netid State  Recv-Q Send-Q   Local Address:Port    Peer Address:Port Process
tcp   LISTEN 0      128            0.0.0.0:4242         0.0.0.0:*     users:(("sshd",pid=28429,fd=6))
tcp   LISTEN 0      128               [::]:4242            [::]:*     users:(("sshd",pid=28429,fd=4))
[root@wil wil]# firewall-cmd --list-service
ssh
[root@wil wil]# firewall-cmd --list-port
4242/tcp
[root@wil wil]# firewall-cmd --state
running
[root@wil wil]# _
```

For **Debian**:

```bash
root@wil:~# head -n 2 /etc/os-release
PRETTY_NAME="Debian GNU/Linux 10 (buster)"
NAME="Debian GNU/Linux"
root@wil:/home/wil# ss -tunlp
Netid State  Recv-Q Send-Q   Local Address:Port    Peer Address:Port

tcp   LISTEN 0      128            0.0.0.0:4242         0.0.0.0:*     users:(("sshd",pid=523,fd=3))
tcp   LISTEN 0      128               [::]:4242            [::]:*     users:(("sshd",pid=523,fd=4))
root@wil:/home/wil# /usr/sbin/ufw status
Status: active

To                         Action      From
--                         ------      ----
4242                       ALLOW       Anywhere
4242 (v6)                  ALLOW       Anywhere (v6)
```

<br>

## Bonus part

### Partitioning
---

Set up the partitions correctly so that you obtain a structure similar to the
one below:

``` bash
# lsblk
NAME                       MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda                          8:0    0  30.8G  0 disk
├─sda1                       8:1    0   500M  0 part  /boot
├─sda2                       8:2    0     1K  0 part
└─sda5                       8:5    0  30.3G  0 part
  └─sda5_crypt             254:0    0  30.3G  0 crypt 
    ├─LVMGroup-root        254:1    0    10G  0 lvm   /
    ├─LVMGroup-swap        254:2    0   2.3G  0 lvm   [SWAP]
    └─LVMGroup-home        254:3    0     5G  0 lvm   /home
    └─LVMGroup-var         254:4    0     3G  0 lvm   /var
    └─LVMGroup-srv         254:5    0     3G  0 lvm   /srv
    └─LVMGroup-tmp         254:6    0     3G  0 lvm   /tmp
    └─LVMGroup-var--log    254:7    0     4G  0 lvm   /var/log
sr0                         11:0    1  1024M  0 rom
```

> [!NOTE]
> The example shows arbitrary disk sizes. You need to determine the appropriate
> size for each partition to ensure proper operation while avoiding unnecessary
> disk usage.


### Website and services

Set up a functional **WordPress** website with the following services: **lighttpd**, **MariaDB**, and **PHP**.

Set up a service of your choice that you think is useful (NGINX / Apache2
excluded!). During the defense, you will have to justify your choice.

> [!NOTE]
> To complete the bonus part, you have the possibility to set up extra services.
> In this case, you may open more ports to suit your needs. Of course, the
> UFW/firewalld rules have to be adapted accordingly.


<br>

## Readme requirements

A Project description section must also explain your choice of operating system
(Debian or Rocky), including their respective advantages and disadvantages. It
must describe the main design choices made during the setup (partitioning,
security policies, user management, services installed) and provide a comparison
between:

- Debian vs Rocky Linux
- AppArmor vs SELinux
- UFW vs firewalld
- VirtualBox vs UTM


<br>

## Submission and peer-evaluation

**Submission:** `README.md` and `signature.txt` files at the root of the Git
repository

### `signature.txt`

Paste the signature of your machine's virtual disk into `signature.txt`:

1. Open the default installation folder (folder where the VMs are saved)

    - Windows: `%HOMEDRIVE%%HOMEPATH%\VirtualBox VMs\`
    - Linux: `~/VirtualBox VMs/`
    - MacM1: `~/Library/Containers/com.utmapp.UTM/Data/Documents/`
    - MacOS: `~/VirutalBox VMs/`

<br>

2. Retrieve the signature from the ".vdi" file (or ".qcow2" for UTM users) of
   your virtual machine in `sha1` format. Below are four command examples for a
   `rocky_serv.vdi` file:

    - Windows: `certUtil -hashfile rocky_serv.vdi sha1`
    - Linux: `sha1sum rocky_serv.vdi`
    - MacM1: `shasum rocky.utm/Images/disk-0.qcow2`
    - MacOS: `shasum rocky_serv.vdi`

    <br>

    This is an example of the output you will get:

    - `6e657c4619944be17df3c31faa030c25e43e40af`

> [!NOTE]
> Please note that your virtual machine’s signature will be altered as soon as
> you start the virtual machine again. To allow signature verification for all
> your evaluations, you can either duplicate the virtual machine disk file, or
> use a snapshot for each evaluation.

> [!WARNING]
> During the defense, the signature of the signature.txt file will be compared
> with the one of your virtual machine. If the two of them are not identical,
> your grade will be 0.

> [!WARNING]
> The use of snapshots is restricted. Each defense must start without any
> snapshots. A snapshot dedicated to the defense will then be created and
> deleted at the end of the defense. You are encouraged to make tests with the
> snapshot features before submitting your project.
