# Deploy single node WAZUH 4.3 with Ansible

This repo is used for installing a single node of WAZUH version 4.3. 

Please refer to Known Issues for use. 



![Logo](https://wazuh.com/wp-content/themes/wazuh-v3/assets/images/home/dashboard-animation.gif?ver=1654085884)



## Deployment

To deploy this project run the playbook named wazuh_play.yml (work in progress)

Tested on Ubuntu 20.04. Should work on Ubuntu 16.04, 18.04, 20.04 ONLY. 


## Features

- Log data analysis
- File integrity monitoring
- Rootkits detection
- Configuration assessment
- System inventory
- Vulnerability detection
- Container security
- Regulatory compliance

## Playbooks

All in one
```javascript
---
- name: Wazuh installation 
  hosts: local
  gather_facts: no # for performance means only
#  ignore_errors: yes
  remote_user: <node-user>
  
  roles:
    - wazuh_indexer
    - wazuh_server
#    - wazuh_dashboard  (work in progress)
```

Unattended
```javascript
---
- name: Wazuh unattended installation 
  hosts: local # replace with hostname or group
  gather_facts: no # for performance means only
#  ignore_errors: yes
  remote_user: piet #replace with user
  
  tasks: 
  - name: Create directories
    ansible.builtin.file:
      path: /home/piet/wazuh_auto #directory for downloading, change as needed
      state: directory
       mode: '0755'

    - name: Downloading Wazuh
      become: yes
      get_url:
        url: https://packages.wazuh.com/4.3/wazuh-install.sh
        dest: /home/piet/wazuh_auto #match with above created directory

    - name: Script uitvoeren
      become: yes
      ansible.builtin.shell: bash /home/piet/wazuh_auto/wazuh-install.sh -a
```

Agent
```javascript

```


## Known Issues

Currently the step by step automation is showing issues starting the services of the wazuh-indexer and server. It is advised to use the unattended awaiting a solution. 


## Indexer config

Replace the node names and IP values with the corresponding names and IP addresses

```javascript
nodes:
  # Wazuh indexer nodes
  indexer:
    - name: node-1
      ip: <indexer-node-ip>
    # - name: node-2
    #   ip: <indexer-node-ip>
    # - name: node-3
    #   ip: <indexer-node-ip>

  # Wazuh server nodes
  # Use node_type only with more than one Wazuh manager
  server:
    - name: wazuh-1
      ip: <wazuh-manager-ip>
    # node_type: master
    # - name: wazuh-2
    #   ip: <wazuh-manager-ip>
    # node_type: worker

  # Wazuh dashboard nodes
  dashboard:
    - name: dashboard
      ip: <dashboard-node-ip>
```

## Documentation

- [Official WAZUH DOCU](https://documentation.wazuh.com/current/index.html)
- [Linux Ubuntu](https://ubuntu.com/)
