# Ansible Resources

Learn how to automate Raspberry Pi setup with Ansible. The Raspberry Pi is a tiny and affordable computer that you can use to learn, test and experiment concepts about distributed computing. The first challenge is always how to keep the Rasbperry Pi’s up to date. Having four Rasbperry Pi seems manageable, but If you have more than 4 Rasbperry Pi, you'll need an automation tool like ```Ansible``` to automate repetitive task like ```installing```, ```updating``` software on your Rasbperry Pi’s.


In this brief tutorial, I'll walk you through the basics of Ansible using a cluster of four Raspberry Pi.

![alt text](https://github.com/sayems/ansible.resources/blob/master/imgages/architecture.png "Logo Title Text 1")

&nbsp;

## Table of contents
- [Ansible Installation](#installing-ansible-on-mac)
    - [Install Ansible on Mac](#installing-ansible-on-mac)
    - [Test Ansible on Localhost](#test-ansible)
    - [Configure your macOS ```/etc/hosts``` for Ansible](#edit-your-macos-etchosts-for-ansible)
    - [Enable Passwordless SSH access for ansible](#enable-passwordless-ssh-access-for-ansible)
    - [Configure Ansible ```/etc/ansible/hosts```](#ansible-hosts-and-groups)
        - [Ansible playbook to ping all server](#ansible-playbook-to-ping-all-server)
        - [Check python version with ansible](#check-python-version-with-ansible)
- [YAML Basics](https://github.com/sayems/ansible.resources/wiki/YAML-Basics)
- [Ad hoc Commands](https://github.com/sayems/ansible.resources/wiki/Ad-hoc-Commands)
- [Ansible Inventory](https://github.com/sayems/ansible.resources/wiki/Ansible-Inventory)
- [Ansible Playbooks](https://github.com/sayems/ansible.resources/wiki/Ansible-Playbooks)
- [Ansible Roles](https://github.com/sayems/ansible.resources/wiki/Ansible-Roles)
- [Ansible Variables](https://github.com/sayems/ansible.resources/wiki/Ansible-Variables)
- [Advanced Execution](https://github.com/sayems/ansible.resources/wiki/Advanced-Execution)
- [Ansible Vault](https://github.com/sayems/ansible.resources/wiki/Ansible-Vault)
- [Ansible Container](https://github.com/sayems/ansible.resources/wiki/Ansible-Container)
- [Troubleshooting](https://github.com/sayems/ansible.resources/wiki/Troubleshooting)
- [Ansible Books and courses](https://github.com/sayems/ansible.resources/wiki/Ansible-books-and-courses)
- [Additional Resources](https://github.com/sayems/ansible.resources/wiki/Additional-Resources)




[top](#table-of-contents)
&nbsp;



Installing Ansible on Mac
--

The best way to get started on a macOS is to use [Homebrew](https://brew.sh/). If you already have [Homebrew](https://brew.sh/) installed, you can install ```ansible``` by running the following command
```
brew install ansible
```

If you don't already have [Homebrew](https://brew.sh/) installed, I would strongly recommend that you install it! It's incredibly useful for installing software dependencies like OpenSSL, MySQL, Postgres, Redis, SQLite, and more.

You can install it by running the following command:
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

[top](#table-of-contents)
&nbsp;

Test Ansible
--

Run the following command:
```
ansible localhost -m ping
```
```
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

[top](#table-of-contents)
&nbsp;

Edit your macOS ```/etc/hosts``` for Ansible
--
1. Open your terminal, type this command:
```
sudo nano /etc/hosts
```

2. Add ```192.168.1.x``` to end of the ```/etc/hosts``` file.
```
# Raspberry Pi
192.168.1.6		raspi1    
192.168.1.7		raspi2
192.168.1.8		raspi3
192.168.1.9		raspi4
```

3. To save your changes, press ```Ctrl+x``` on your keyboard.


[top](#table-of-contents)
&nbsp;


Enable Passwordless SSH access for ansible
--

In order to fully control a remote machine we need to be able to execute command on the remote machines as user ```root```. 

To enable passwordless sudo:

1. Log in to the Raspberry Pi command-line interface. The default user name and password of the Raspberry Pi hardware are ```pi``` and ```raspberry```, respectively.

2. In the command-line interface, type this command:
```
sudo nano /etc/sudoers.d/010_pi-nopasswd
```
3. Enable passwordless sudo access by adding this line:
```
<user name> ALL=(ALL) NOPASSWD: ALL
```
For example, to provide passwordless sudo access to a user ```pi```, type:
```
pi ALL=(ALL) NOPASSWD: ALL
```

4. To save your changes, press ```Ctrl+x``` on your keyboard.

5. For these changes to take effect, restart Raspberry Pi by typing this command:
```
sudo reboot
```

[top](#table-of-contents)
&nbsp;


### Ansible Hosts and Groups

1. Open your terminal, type this command create ```/etc/ansible/hosts``` directory
```
sudo mkdir -p /etc/ansible
```

2. Add ```hostname``` to ```/etc/ansible/hosts``` file.
```
[gui]
raspi1 
raspi2 

[nogui]
raspi3 
raspi4 
```

You also can define ‘host list’ with ranges

```
[gui]
raspi[1:2]

[nogui]
raspi[3:4]
```

[top](#table-of-contents)
&nbsp;

##### Ansible playbook to ping all server

```
ansible -m ping all
```
```
raspi2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
raspi1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
raspi4 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
raspi3 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

[top](#table-of-contents)
&nbsp;

##### Check python version with ansible

```
ansible -m shell -a 'python -V' gui

raspi1 | CHANGED | rc=0 >>
Python 2.7.13

raspi2 | CHANGED | rc=0 >>
Python 2.7.13
```

```
ansible -m shell -a 'python -V' all

raspi3 | CHANGED | rc=0 >>
Python 2.7.13

raspi4 | CHANGED | rc=0 >>
Python 2.7.13

raspi1 | CHANGED | rc=0 >>
Python 2.7.13

raspi2 | CHANGED | rc=0 >>
Python 2.7.13
```

[top](#table-of-contents)
&nbsp;
