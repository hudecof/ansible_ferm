Ferm / iptables managment
=========

This role manages the iptables using ferm script.

As it's very hard to write generic iptables template, this role just moves **user defined** ferm confing snippets to the server and generate the ruleset using iptables.

Requirements
------------

For Redhat/CentOS distributions **EPEL** si needed.

Role Variables
--------------

There are only 2 varables needed to properly setup this role

- `ferm_pkg_state`, defaults to `present`

- `ferm_rules_directory`, defaults to `.`, means points to role template directory

- `ferm_rules`, defaults to `[]` is the list of ferm rules to copy on the servers, without the **.conf.j2** suffix

All the others variables in the are used in my sample rules sets. As you can see I use the power of the templating engine and the ferm engine to generate rules for **IPv4** and **IPv6**. The hard work to write the rules is still on you, but ypu have it fully **under control**.

For example the `ferm variables` in your `group_vars/all` could be

```
ferm_rules_directory: ../../../files/ferm

ferm_rules:
  - vars
  - default_rules
  - connection_tracking
  - input_icmp
  - managment
  - service_zabbix-agent
```

You should rewrite the `ferm_rules` in **group_var** or **host_vars** for each group or server as needed.


Dependencies
------------

The ferm package is default in the Debian/Ubuntu based distributions.

For EL you need EPEL to be enabled. There is numerous playbooks to do this stuff, so choose any. 

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: ferm
      roles:
         - hudecof.ferm

License
-------

BSD

Author Information
------------------

Peter Hudec
CNC, a.s.
