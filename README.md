# Ansible role to configure a USB IR transmitter as a remote
This role configures a host to use USB IR transmitter (aka "blaster")
to emulate a remote, such as a stereo or TV remote.

Features:
- installs latest lirc
- installs remote configuration files (two examples included)
- configures lirc to send commands to a USB IR blaster
- configures lirc to run as a service at startup (via systemd)

# Requirements
Provisioning host:
- ansible 2.9 or later
- lirc configuration file for your remote
  - First check [http://lirc.sourceforge.net/remotes/](http://lirc.sourceforge.net/remotes/)
  to see if a configuration file has already been created
  - Otherwise, try searching the web for someone who has created the file or for
    instructions on how to create your own.

Host that will run lirc-blaster:
- Ubuntu 18.04 or later (or Debian-based Linux distro running apt)
- USB IR transmitter
  - This role is tested with [FTDI Based USB IR Blaster](http://www.irblaster.info/usb_blaster.html)

# How to use this role
### Method 1: Install using ansible-galaxy

The most robust way to install this role is to use ansible-galaxy,
which is installed with ansible by default. ansible-galaxy is effectively a package manager for ansible. It installs roles
from the community, or from private git repos, into your local machine for use in your playbooks.

Start by adding this role to an ansible-galaxy dependency file. Typically this file lives alongside your playbook file and by convention is named `requirements.yml`.

Add the following section to `requirements.yml`:

```
roles:
  - name: elgeeko1-lirc-ansible
    src: https://github.com/elgeeko1/elgeeko1-lirc-ansible
    version: main
```

If there is already a `roles` section, simply append this role to
add it as a dependency.

Then, install the role using ansible-galaxy:

`ansible-galaxy install -r requirements.yml -v`

### Method 2: Clone this repository into a `roles` directory

Use this method if you plan to modify this role, or if for some
reason the ansible-galaxy method fails.

Starting from your playbook directory, change into the `roles`
directory and clone:

```
$ ls
 > playbook.yml
 > roles/
$ cd roles/
roles$ git clone https://github.com/elgeeko1/elgeeko1-lirc-ansible
```

# Configuring
### Place your remote configuration file where Ansible will find it
Usually in the `files` directory adjascent to your playbook.

### Find the serial number of your USB blaster
Use `dmesg` to find the serial number of your USB blaster.
See [FTDI USB IR Blaster / Transmitter using LIRC](https://www.mythtv.org/wiki/FTDI_USB_IR_Blaster_/_Transmitter_using_LIRC) for more instructions.

Set the role variable `LIRC_BLASTER_SERIAL` to the serial number of your remote.

### Configure remotes

Set the role variable `LIRC_BLASTER_REMOTES` to the list of remote configuration
files to be installed with LIRC.

Using the two examples included in this repository:
```
- name: lirc-blaster role
  role: elgeeko1-lirc-blaster-ansible
  vars:
    LIRC_BLASTER_SERIAL: "B33FG00D"
    LIRC_BLASTER_REMOTES:
      - smsl-rc1.lircd.conf
      - yamaha-ras13.lircd.conf
```

# Hardware
- USB IR blaster: https://www.irblaster.info/usb_blaster.html

# Resources
- https://www.irblaster.info/usb_blaster.html
- https://www.mythtv.org/wiki/FTDI_USB_IR_Blaster_/_Transmitter_using_LIRC
