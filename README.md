# Ansible Playbook for FreeBSD

## Introduction

The is an Ansible playbook to provision a basic FreeBSD system.  It can be used as a baseline for either
a desktop or server system.


## How to use

### Prerequisites

Ansible Server:
- Ansible
- sshpass
  - notes:
    - sshpass has potential security concerns, and is not a best practice to use
    - preferred practice is to deploy and utilize SSH keys (assuming minimum Ansible requirements are met)
    - to install sshpass on Debian based Linux ```sudo apt install sshpass```

Ansible Client:
- FreeBSD (tested with 13.1)
- Python3
  - the base role will check for Python 3, if it's not installed, it will proceed to install it
- SSH server enabled
  - non-privilege user access
  - root access

### Quickstart

```git clone https://github.com/richlamdev/ansible-server-freebsd.git```

```cd ansible-server-freebsd```

Edit inventory file at the root of the repo to reflect the hostname and/or IP to have this playbook applied to.

Edit the vars file at role/base/defaults/main.yml, to your requirements.

```ansible-playbook main.yml -bkKu <username>```

Enter the <username> password when prompted:

**SSH password:**

Enter the root password when prompted:

**BECOME password[defaults to SSH password]:**

*NOTE: This playbook was designed for standard FreeBSD deployment.  FreeBSD, by convention, does not have\
sudo installed, consequently, privilege escalation is achieved via su, as opposed to sudo.  Therefore the connection\
is established via non-privileged user by SSH, followed by the root password for privilege escalation.  See below\
considerations section for potential implementation options.*


### Base role

1. Checks presence of Python 3, will install Python 3, if it is not already installed.

2. Deploys the following static configuration files:
    * pf.conf
    * loader.conf.local
    * rc.local
    * newsyslog.conf

3. Deploys the following dynamic configuration files:
    * ntp.conf
    * rc.conf
    * syslog.conf

This role is idempotent.


## Notes, General Information & Considerations

1. This playbook minimizes installation of additional software, to minimize potential security vulnerabilities.

2. As mentioned above, sudo is not installed or assumed to be installed for the execution of this playbook.  Privilege\
escalation is achieved via su.

3. Python is a requirement for Ansible.  (to use the full potential of modules, which will enable idempotency)

4. This [Vagrant file](https://github.com/richlamdev/vagrant-files/blob/main/freebsd/Vagrantfile) works with this repo to start an FreeBSD virtual machine for testing.
The setup.sh forces the root password of each virtual machine to be password1. (obviously not secure, but for the purposes of testing and life of these virtual machines, not so much an issue)
Additionally the vagrant user is added to the wheel group.

Naturally, you will need Vagrant and VirtualBox, use and installation is information beyond the scope of this repo.
