# Ferm / iptables managment

- **Github**: [![Build Status](https://travis-ci.org/hudecof/ansible_ferm.svg?branch=master)](https://travis-ci.org/hudecof/ansible_ferm)


This role manages the iptables using [ferm](http://ferm.foo-projects.org) script.

As it's very hard to write generic iptables template, this role just moves **user defined** ferm config snippets to the server and generate the ruleset using iptables.

## Requirements

- ansible: 2.1
- Redhat/CentOS: EPEL
- Ubuntu: multiverse repository

## Role Variables

### OS based variables

Some variables are based on OS. These variables are locaten in `vars/os-<OS>.yml` files.

### Generic Variables

- `ferm_directory`: ferm configuration directory, defaults to **/etc/ferm**
- `ferm_service_state`: if the ferm should be started
- `ferm_service_enabled`: if the ferm should be enabled in boot sequence

### Firewal rules

- `ferm_rules_directory`: where should I look for the firewall rules files, default to playbook templates directory
- `ferm_net_mngt`: list of management networks, defaultd allow any
- `ferm_domains`: to which ip version generate the rules, defaults **IPv4** and **IPv6**
- `ferm_rules`: list of rules to apply. defualt allow only SSH and ICMP


Power of the templating engine and the ferm engine to generate rules for **IPv4** and **IPv6**. The hard work to write the rules is still on you, but you have it fully **under control**.

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

## License

BSD

## Author Information

Peter Hudec
