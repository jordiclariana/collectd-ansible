collectd-ansible
==========

This was forked from **Stouts.ansible** to provide apt package installation

Ansible role which help you with:

* Install and setup [Collectd](https://collectd.org/)

#### Variables

```yaml
collectd_enabled: yes               # Enable the role
collectd_apt_state: present

# General options
collectd_interval: 10
collectd_readthreads: 7
collectd_hostname: "{{ inventory_hostname }}"
collectd_fdqnlookup: false

# Collectd plugins
collectd_plugins: []                # Ex. [nginx, memcached]
collectd_plugins_options: {}        # See below for examples.

# Collectd default plugins
collectd_default_plugins: [cpu, df, interface, load, memory, swap]
collectd_default_plugins_options:
  swap:
  - ReportByDevice false
  interface:
  - Interface lo
  - IgnoreSelected true

# Additional types
# format: { name: ..., value: ... }
collectd_types: []

# Collectd graphite options
collectd_write_graphite: no
collectd_write_graphite_options:    # Setup write_graphite (https://collectd.org/wiki/index.php/Plugin:Write_Graphite)
  Host: "{{inventory_hostname}}"
  Port: 2003
  Prefix: stats.
  # Postfix: .collectd
  Protocol: tcp
  AlwaysAppendDS: 'false'
  EscapeCharacter: _
  LogSendErrors: 'true'
  StoreRates: 'true'

# Setup logs
collectd_logpath:                   # If it is not empty, will be used logfile
collectd_loglevel: info
collectd_logrotate: yes
collectd_logrotate_options:
  - compress
  - copytruncate
  - daily
  - dateext
  - rotate 7
  - size 10M
```

Example:

```yaml

- hosts: all

  roles:
    - Draiken.collectd

  vars:
    collectd_write_graphite: yes
    collectd_plugins: [nginx]
    collectd_write_graphite_options:
      Host: localhost
      LogSendErrors: 'true'
```

#### License

Licensed under the MIT License. See the LICENSE file for details.
