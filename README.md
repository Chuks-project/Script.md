# Playbook/site.yml---
- name: import common file
  import playbook: ../static-assignments/common.yml
  tags:
  - always

- name: include env-vars file
  import playbook: ../dynamic-assignments/env-vars.yml
  tags:
  - always

- name: import database file
  import playbook: ../static-assignment ../static-assignments/db.yml

- name: import webservers file
  import playbook: ../static-assignments/webserver.yml

- name: import loadbalancers assignment
  import playbook: ../static-assignments/lb.yml
  when: loadbalancer_is_required
  
  
  # Dynamic-assignments/env-vars.yml
  
  ---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always

