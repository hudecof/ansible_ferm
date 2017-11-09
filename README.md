# Ferm / iptables managment

- **CNC**: [![build status](https://source.cnc.sk/ansible/role-ferm/badges/master/build.svg)](https://source.cnc.sk/ansible/role-ferm/commits/master)
- **Github**: [![Build Status](https://travis-ci.org/hudecof/ansible_ferm.svg?branch=master)](https://travis-ci.org/hudecof/ansible_ferm)


This role manages the iptables using [ferm](http://ferm.foo-projects.org) script.

As it's very hard to write generic iptables template, this role just moves **user defined** ferm confing snippets to the server and generate the ruleset using iptables.

## Requirements

- ansible: 2.1
- Redhat/CentOS: EPEL
- Ubuntu: multiverse repository

## Role Variables

There are only 3 variables needed to properly setup this role

- `ferm_service_state`, defaults to **started** defined the service state
- `ferm_service_enabled`, defaults to **yes**, defined wheather the service should start on boot
- `ferm_test`, defaults to **false** is used during the testing in docker, you do not need to modify it.
- `ferm_rules` is list of ferm rules to load. The filaname shoud have suffix **.conf.j2**. Defaults is pointgto provided rules which will allow only incomming ICMP and SSH.
- `ferm_rules_directory` is point do role directory. To use your own rule templates you need to modify this.


All the others variables are used in my sample rules sets. As you can see I use the power of the templating engine and the ferm engine to generate rules for **IPv4** and **IPv6**. The hard work to write the rules is still on you, but ypu have it fully **under control**.

## Example

### host/group variables
```
ferm_rules_directory: {{ playbook_dir }}/files/ferm

ferm_rules:
  - vars
  - default_rules
  - connection_tracking
  - input_icmp
  - managment
  - service_zabbix-agent
```
In this case you should create following files

- `{{ playbook_dir }}/files/ferm/rules/vars.conf.j2`
- `{{ playbook_dir }}/files/ferm/rules/default_rules.conf.j2`
- `...`

You should rewrite the `ferm_rules` in **group_var** or **host_vars** for each group or server as needed.


### playbook

For example the `ferm variables` in your `group_vars/all` could be

```
- hosts: ferm
  roles:
     - hudecof.ferm
```

## Dependencies

None

License
-------

BSD

Author Information
------------------

Peter Hudec
CNC, a.s.
