Ansible Role to Create Network Inventory
=========

Ansible role to create network inventory based on device facts gathered by platform
specific roles (`see vars/main.yaml`).

Contribute
--------------

If the platform you were looking for didn't get populated:
1. Fork this repository.
2. Update the file `vars/main.yaml`.
3. Commit your change.
4. Send your Pull Request.

> NOTE: Inventory is populated based on facts located at: hostvars[inventory_hostname][network_inventory_platform_map[network_inventory_os]].config

Role Variables
--------------

Variables are defined in `defaults/main.yml` and structured/encapsulated in `vars/main.yaml`.

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
- name: "Network Discovery: SNMP"
  hosts: all
  connection: local
  gather_facts: false

  vars:
    network_discovery_community: "mycommunity"

  roles:
    - role: victorock.network_discovery
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

Requirements
--------------

You need to have facts populated by platform specific roles supported by Ansible Networking:
  - ansible-network.cisco_ios
  - ansible-network.cisco_nxos
  - ansible-network.arista_eos

License
------------

GPLv3

Author
------------

Victor da Costa (@victorock)
