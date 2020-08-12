Az_vm
=========

Creates a Virtual Machine in Microsoft Azure

Requirements
------------

Requires Azure SDK 

pip install 'ansible[azure]'

Role Variables
--------------

Role takes following vars

resourcegroup = name of the resource group to create

location = Azure location to create resourcegroup in

tag_owner = Create tag owner with value

tag_project = Create tag project with value

vmname = Name for ressources to VM

virtualnetwork_name = Name for existing Virtual Network

networksecuritygroup_name = Security Group Name

vmsize = Azure Vm sizes eg. Standard_A1_v2

ostype = Windows or Linux

azurevmuser = Username for vm, administrator, admin or root is not acceptet

azurevmpw = Password for admin user

offer = RHEL or WindowsServer

publisher = RedHat or MicrosoftWindowsServer

sku = for RedHat = 8 for Windows = 2019-Datacenter

version = latest



Dependencies
------------

jesperberth.az_resourcegroup
jesperberth.az_virtualnetwork
jesperberth.az_securitygroup

Example Playbook
----------------

```ansible

- hosts: localhost
  name: Create Azure Security Group
  vars:
    resourcegroup: resourcegroupname
    location: northeurope
    tag_owner: jesper
    tag_project: demoproject
  tasks:
    - name: Azure Security Group
      include_role:
        name: jesperberth.az_vm
      vars:
        networksecuritygroup_name: SG_Network
        vmname: server1
        virtualnetwork_name: "VirtualNet"
        networksecuritygroup_name: "SG_Network"
        vmsize: "Standard_A1_v2"
        ostype: "Linux"
        azurevmuser: jesbe
        azurevmpw: P@ssw0rdToYou788
        offer: "RHEL"
        publisher: "RedHat"
        sku: "8"
        version: "latest"

```
With Multible rules in a loop

```ansible

- hosts: localhost
  name: Create Azure Security Group
  vars:
    resourcegroup: resourcegroupname
    location: northeurope
    tag_owner: jesper
    tag_project: demoproject
  tasks:
    - name: Azure Security Group
      include_role:
        name: jesperberth.az_vm
      vars:
        vmname: "{{ item.vmname }}"
        virtualnetwork_name "VirtualNetwork"
        networksecuritygroup_name: "SG_Network"
        vmsize: "Standard_A1_v2"
        ostype: "{{ item.ostype }}"
        azurevmuser: jesbe
        azurevmpw: P@ssw0rdToYou788
        offer: "{{ item.offer}}"
        publisher: "{{ item.publisher}}"
        sku: "{{ item.sku}}"
        version: "{{ item.version}}"
      loop:
        - { 'vmname': 'server1', 'ostype': 'Linux', 'offer': 'RHEL', 'publisher': 'RedHat', 'sku': '8', 'version': 'latest' }
        - { 'vmname': 'server2', 'ostype': 'Windows', 'offer': 'WindowsServer', 'publisher': 'MicrosoftWindowsServer', 'sku': '2019-Datacenter', 'version': 'latest' }
```

License
-------

BSD

Author Information
------------------

Jesper Berth