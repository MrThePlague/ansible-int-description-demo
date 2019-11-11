# Ansible Dyanmic Interface Descriptions
A simple Ansible playbook to set interface descriptions on **Cisco IOS** and **NX-OS** devices based on neighbour device details learned through CDP.

## Getting Started

### Prerequisites

This playbook uses the resource modules introduced in **Ansible 2.9**, so ensure you have Ansible 2.9 installed. CDP must be enabled on the devices.

```
$ ansible --version
ansible 2.9.0
  config file = None
  configured module search path = ['/Users/rchambers/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/Cellar/ansible/2.9.0/libexec/lib/python3.7/site-packages/ansible
  executable location = /usr/local/bin/ansible
  python version = 3.7.5 (default, Nov  1 2019, 02:16:32) [Clang 11.0.0 (clang-1100.0.33.8)]
```

### Installing

Clone this repository onto your Ansible control node or workstation:

```
git clone https://github.com/MrThePlague/ansible-int-description-demo.git
```

## Define the inventory

Edit the `inventory.ini` to list your devices, and group them as you please, but esure that the `ansible_network_os` variable is defined correctly for each group of devices.

## Run the playbook

To run the playbook, simply navigate into the cloned directory and run `ansible-playbook`. Pass the `-u <username>` switch to input the SSH username, where `<username>` is the account used to administer the device, and the `-k` switch to be prompted for the SSH password.

```
$ ansible-playbook -u <username> -k set-int-descriptions.yml
```

The interface description will be overwritten with information gathered through CDP, in the format `<< Neighbour Hostname >> - << Remote Interface >>`. 

`Router-1` connected to `Router-2`, each on interface Gig0/1, e.g.:

```
Router-1# show run int gi0/1
interface GigabitEthernet0/1
 description Router-2.example - GigabitEthernet0/1
```

## Exclude an interface

Interfaces may be excluded from having their descriptions updated by editing the `when:` to exclude the desired interface, e.g.:

```
when: ansible_network_os == 'nxos' and item.key != "mgmt0"
```

The above will exlcude the interface named `mgmt0`.