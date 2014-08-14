Role Name
========

Ansible role to install and configure CollectD

See https://github.com/valentinogagliardi/collectd/wiki for detailed instructions.

Suggested Companions Roles
------------

Ansible role to install Facette: https://galaxy.ansible.com/list#/roles/1298

Example Playbooks
-------------------------

```yaml
- hosts: CollectdServerDebian
  roles:
   - { role: valentinogagliardi.collectd,
             collectd_plugins:
             [{ plugin: "load" },
              { plugin: "memory" }],

             collectd_plugins_multi:
             { rrdtool: { Datadir: '"/var/lib/collectd/rrd"' },
               network: { Listen: '"{{ ansible_eth0.ipv4.address }}"'}},

             tags: ["collectd"] }

- hosts: ClientOne:ClientTwo
  roles:
   - { role: valentinogagliardi.collectd,
             collectd_plugins:
             [{ plugin: "load" },
              { plugin: "memory" }],

             collectd_plugins_multi:
             { interface: { Interface: '"eth0"' },
               disk: { Disk: '"sda"' },
               network: { Server: '"CHANGEME"' }},
              tags: ["collectd"] }
```
Role Variables
--------------

```yaml
collectd_packages_RedHat:
 - { package: "collectd" }

collectd_packages_Debian:
 - { package: "collectd" }
 - { package: "collectd-utils" }

collectd_global:
 - { directive: "Hostname {{ ansible_fqdn }}" }
 - { directive: "FQDNLookup false" }
 - { directive: "Interval 10" }
 - { directive: "Timeout 2" }
 - { directive: "ReadThreads 5" }
 - { directive: "AutoLoadPlugin false" }

collectd_plugins: "none"
collectd_plugins_multi: "none"

collectd_conf_rh: "/etc/collectd.conf"
collectd_conf_deb: "/etc/collectd/collectd.conf"

collectd_include_rh: "/etc/collectd.d"
collectd_include_deb: "/etc/collectd"
```

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - valentino.g@servermanaged.it
