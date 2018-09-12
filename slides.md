## Ansible Workshop

---

## Agenda

- Introduction
- Documentation overview & installation
- The `ansible` command
- Inventory & Hosts
- The `ansible-playbook` command
- Tasks, Roles & Variables
- Control flow & Debugging
- The `ansible-galaxy` command
- Developing Modules (and Plugins)

---

## What are your expectations?

---


## Introduction

- general purpose IT automation
- initially developed by Michael DeHaan (Cobbler, Puppet)
- written in Python, first release in 2013
- acquired by RedHat to automate OpenShift in 2015

---

## Motivation

- declarative approach, YAML instead of code
- agent-less operation using SSH (but requires Python,
  other connection types can be plugged in)
- work with heterogeneous environments
- promote simplicity & collaboration

---

## Docs overview

Landing page:  
https://docs.ansible.com

Ansible Documentation:  
https://docs.ansible.com/ansible/latest/index.html

User Guide:  
https://docs.ansible.com/ansible/latest/user_guide/index.html

Module Index:  
https://docs.ansible.com/ansible/latest/modules/modules_by_category.html

---

## Installation

- [Installation Guide / What version to pick?](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id10)

- [Installation Guide / Installing the control machine](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-machine)

---

## Quick start

Run one of these commands:
```
install-ansible.sh
```
```
vagrant up ansible-master
```
```
./docker-start-master.sh
```

---

## The `ansible` command

- used for ad-hoc tasks & debugging
  ```
  ansible <host-pattern> [options]
  ```

```
ansible localhost -m ping
```
```
ansible -i vagrant-inventory.ini all \  
-m shell -a "python --version"
```
```
ansible -i docker-inventory.ini all \  
-m raw -a "cat /etc/*-release"
```

---

## Inventory

- list of groups of hosts
- hosts can be in multiple groups
- various formats possible
- can be an executable script
- [Docs / Working with Inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
- [Team Infinity inventories](https://github.com/ePages-de/epages-infra/tree/dev/ansible/inventory)

---

## Hosts & Hostvars

- every host and group can have their own set of variables  
(but we would like to avoid host specific variables if possible)

- can be set directly in the inventory, e.g. connectivity settings  
(see `docker-inventory.ini`, `vagrant-inventory.ini`)

---

## Hosts & Hostvars

- `hostvars` of all hosts can be accessed in playbook
- `fact_caching` can be configured

- Playbook `vars` do not persist between included plays or playbooks!

---

## The `ansible-playbook` command

- for executing "playbooks"
  ```
  ansible-playbook [options] playbook.yml [playbook2 ...]
  ```
- needs an inventory file (or "localhost")
- `-v[vvv]` is generally helpful

---

## The `ansible-playbook` command

Examples:
```
ansible-playbook -i docker-inventory.ini playbook.yml
```
```
ansible-playbook -i vagrant-inventory.ini playbook.yml
```

---

## Playbooks

- contains a list of "plays"
- map groups of hosts to sets of roles
- must contain `hosts`
- can contain `vars`, `roles`, (`pre_`)- and (`post_`)-`tasks`,   
  `handlers`, among other attributes  
  (see [Playbook Keywords Reference](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html) for a complete list, including tasks!)
- can be composed using `import_playbook`, `include_playbook`

---

## Tasks & Modules

- unit of work
- a Task is an instance of a Module
- thousands of modules available ...
- should be given a `name`
- allows `loop` (and other attributes) for repetition

---

## Tasks & Modules

- outputs a JSON result
- can `register` to store result in `hostvars`
- implements idempotence (at least most modules do)
- can be nicely inspected using the `debug` module

---

## Handlers

- syntactically identical to tasks
- only invoked when being `notify`'d
- only invoked once per play
- referenced by their `name`

---

## Roles

- unit of reuse
- most contain `main.yml` *one of* `tasks`, `handlers`, `defaults`,  `vars`, `files`, `templates`, `meta` directories
- applied to a group of hosts in a playbook
- can depend on other roles

---

## Variables & Facts

- Variables are generally scoped by hosts, hence `hostvars`
- Variables discovered from the system are called "Facts"
- `set_fact` module to create variables,
  e.g. from the result of a task
- `register` and `set_fact` are effectively identical
- [Docs / Variable Precedence](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable)

---

## Control flow & Debugging

Tags:

- tags can be applied to group tasks, blocks or plays
- eventually, only tasks receive the tag
- used in conjunction with `--tags` and `--skip-tags`
  on the command line

---

## Control flow & Debugging

Start & Step:

- use `--start-at-task=<task_name>` and / or `--step` to gain  
  control over task execution

- general idea is to not fiddle, not comment stuff etc.

---

## The `ansible-galaxy` command

- used to pull high quality roles from Ansible Galaxy
- used to install required roles

---

## Developing Modules (and Plugins)

- lightweight integration using shell scripts
- can use any language (Python preferred)
- binary modules possible
- basic contract: take JSON arguments, return JSON

----

## Discussion & Feedback
