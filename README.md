Ansible Role to Create Network Inventory
=========

Ansible role to create network inventory folder/files structure based on device facts.

> NOTE: Inventory is populated based on facts located at: hostvars[inventory_hostname].ansible_facts

Role Variables
--------------

Variables are defined in `defaults/main.yml`.

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `autorun` | `False`  | Boolean to define if the role "autorun" (`tasks/main.yml`). Useful when you want to have dependencies solved by galaxy (`meta/main.yml`) but don't want it to run automatically.  |
| `network_inventory_os` | `{{ ansible_network_os }}` | If ansible_network_os is not set the default is `ios`. |

Examples
------------

Follow below different examples and ways to use this role.

> 1. Discover the Network Operating System.

```YAML
---
- name: "Network Discovery: PROBE"
  hosts: all
  connection: local
  gather_facts: false
  roles:
    - role: victorock.network_discovery
      network_discovery: "probe"
      autorun: true
```

> 2. Use Platform specific role to gather facts from Network Operating System.

```YAML
---
- name: "Gather Facts: NXOS"
# This group will be created by previous play who will discover the platforms.
  hosts: nxos
  connection: network_cli
  gather_facts: false
  roles:
    - role: ansible-network.cisco_nxos
      function: get_facts
```

> 3. Use Network Inventory role to create inventory Structure.

```YAML
---
- name: "Create Inventory"
  hosts: all
  connection: local
  gather_facts: false
  roles:
    - role: victorock.network_inventory
      autorun: true
```

> NOTE: Inventory files are created in the same directory of the playbook
calling the role `victorock.network_inventory`

Requirements
--------------

The best is to have facts populated by platform specific roles supported by Ansible Networking:
  - ansible-network.arista_eos
  - ansible-network.cisco_ios
  - ansible-network.cisco_nxos

License
------------

GPLv3

Author
------------

Victor da Costa (@victorock)
