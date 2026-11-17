Ansible Role for OpenBao
=========

Example Requirements
--------------------

```yml
---
roles:
  - name: ansible-role-pkcs11
    src: git+https://github.com/moukail/ansible-role-pkcs11.git
    version: main
  - name: ansible-role-openbao
    src: git+https://github.com/moukail/ansible-role-openbao.git
    version: main

```

Example Playbook
----------------

```yaml
---
- name: Install OpenBao
  hosts: all
  become: true

  vars:
    softhsm_token_label: OpenBao
    openbao_seal_type: shamir
    openbao_port: 8200
    openbao_version: 2.7.2

  tasks:
    - name: Prepare SoftHSM
      ansible.builtin.include_role:
        name: ansible-role-pkcs11

    - name: Install OpenBao
      ansible.builtin.include_role:
        name: ansible-role-openbao

  post_tasks:
    - name: Show information message
      ansible.builtin.debug:
        msg:
          - "OpenBAO address: https://{{ ansible_facts['default_ipv4']['address'] }}:{{ openbao_port }}"
          - "OpenBAO token: {{ openbao_token }}"
      when: openbao_token is defined

```