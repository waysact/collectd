Role Name
========

Ansible role to install and configure CollectD

See https://github.com/valentinogagliardi/collectd/wiki for detailed instructions.

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

collectd_include_rh: "/etc/collectd.d"
collectd_include_deb: "/etc/collectd"
```

Companions Roles
------------

Ansible role to install Facette: `valentinogagliardi.facette`

Example Playbook
-------------------------

    - hosts: CollectdServerDebian
      roles:
       - { role: valentinogagliardi.collectd,
                 collectd_packages:
                 [{ package: "collectd" },
                  { package: "collectd-utils" }],
                 collectd_plugins:
                 [{ plugin: "syslog", directive: 'Loglevel "info"' },
                  { plugin: "rrdtool", directive: 'DataDir "/var/lib/collectd/rrd"' },
                  { plugin: "network", directive: 'Listen "{{ ansible_eth0.ipv4.address }}"' },
                  { plugin: "write_graphite", type: "include",
                    Host: "10.0.8.15",
                    Port: "2003",
                    Prefix: "collectd",
                    Postfix: "collectd",
                    StoreRates: "true",
                    AlwaysAppendDS: "false",
                    EscapeCharacter: "_" }],
                 tags: ["collectd"] }

    - hosts: CollectdServerRH
      roles:
       - { role: valentinogagliardi.collectd,
                 collectd_packages:
                 [{ package: "collectd" },
                  { package: "collectd-rrdtool" }],
                 collectd_plugins:
                 [{ plugin: "syslog", directive: 'Loglevel "info"' },
                  { plugin: "rrdtool", directive: 'DataDir "/var/lib/collectd/"' },
                  { plugin: "network", directive: 'Listen "{{ ansible_eth0.ipv4.address }}"' }],
                 tags: ["collectd"] }

    - hosts: CollectdClientOne
      roles:
       - { role: valentinogagliardi.collectd,
                 collectd_plugins:
                 [{ plugin: "syslog", directive: 'Loglevel "info"' },
                  { plugin: "cpu" },
                  { plugin: "users" },
                  { plugin: "processes" },
                  { plugin: "memory" },
                  { plugin: "interface", directive: 'Interface "eth0"' },
                  { plugin: "network", directive: 'Server "{{ collectd_server }}"'  }],
                 tags: ["collectd"] }

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - valentino.g@servermanaged.it
