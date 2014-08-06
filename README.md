Role Name
========

Ansible role to install and configure CollectD

Requirements
------------

none

Role Variables
--------------

```yaml
# Epel repos for RH
epel6_url: "http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm"
epel7_url: "http://dl.fedoraproject.org/pub/epel/beta/7/x86_64/epel-release-7-0.2.noarch.rpm"

# Basic collectD package
collectd_packages:
 - { package: "collectd" }

# Global configuration for CollectD
collectd_global:
 - { directive: "Hostname {{ ansible_fqdn }}" }
 - { directive: "FQDNLookup false" }
 - { directive: "Interval 10" }
 - { directive: "Timeout 2" }
 - { directive: "ReadThreads 5" }
 - { directive: "AutoLoadPlugin false" }

# CollectD plugins to load. You have to define your own in site.yml
collectd_plugins:
 - { plugin: "none" }

# For convenience, an ip address of your centralized CollectD server
collectd_server: "CHANGEME"

# Configuration files for CollectD
collectd_conf_rh: "/etc/collectd.conf"
collectd_conf_deb: "/etc/collectd/collectd.conf"
```

Dependencies
------------

none

Example Playbook
-------------------------

    - hosts: CollectdServerDebian
      roles:
       - { role: collectd,
                 collectd_packages:
                 [{ package: "collectd" },
                  { package: "collectd-utils" }],
                 collectd_plugins:
                 [{ plugin: "syslog", directive: 'Loglevel "info"' },
                  { plugin: "rrdtool", directive: 'DataDir "/var/lib/collectd/rrd"' },
                  { plugin: "network", directive: 'Listen "{{ ansible_eth0.ipv4.address }}"' }],

    - hosts: CollectdServerRH
      roles:
       - { role: collectd,
                 collectd_packages:
                 [{ package: "collectd" },
                  { package: "collectd-rrd" }],
                 collectd_plugins:
                 [{ plugin: "syslog", directive: 'Loglevel "info"' },
                  { plugin: "rrdtool", directive: 'DataDir "/var/lib/collectd/"' },
                  { plugin: "network", directive: 'Listen "{{ ansible_eth0.ipv4.address }}"' }],

    - hosts: CollectdClientOne
      roles:
       - { role: collectd,
                 collectd_plugins:
                 [{ plugin: "syslog", directive: 'Loglevel "info"' },
                  { plugin: "cpu" },
                  { plugin: "users" },
                  { plugin: "processes" },
                  { plugin: "memory" },
                  { plugin: "interface", directive: 'Interface "eth0"' },
                  { plugin: "network", directive: 'Server "{{ collectd_server }}"'  }],

Known Limitations
--------------

At this moment you can specify only one directive per plugin inside the playbook. If you have plan to use plugins which takes more than one directive you can use a dedicated template.

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - valentino.g@servermanaged.it
