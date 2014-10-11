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
    - role: valentinogagliardi.collectd
      collectd_plugins:
        - plugin: load
        - plugin: memory
      collectd_plugins_multi:
        rrdtool: >-
          Datadir "/var/lib/collectd/rrd"
        network: >-
          <Server "{{ ansible_eth0.ipv4.address }}">
            SecurityLevel "Encrypt"
            AuthFile "/etc/collectd/auth_file"
          </Server>
      tags: collectd

- hosts: ApacheWebServers
  roles:
    - role: valentinogagliardi.collectd
      collectd_plugins:
        - plugin: load
        - plugin: vmem
        - plugin: memory

      collectd_plugins_multi:
        interface: >-
          Interface "eth0"
        tcpconns: >-
          LocalPort "80"
        disk: >-
          Disk "sda"
        apache: >-
          URL "http://{{ ansible_lo.ipv4.address }}/server-status?auto"
        network: >-
          <Server "1.2.3.4">
            SecurityLevel "Encrypt"
            Username "CHANGEME"
            Password "1rht44tg2i"
          </Server>
      tags: collectd

- hosts: NginxWebServers
  roles:
    - role: valentinogagliardi.collectd
      collectd_packages_RedHat:
        - package: collectd
        - package: collectd-nginx
      collectd_plugins:
        - plugin: load
        - plugin: vmem
        - plugin: memory
      collectd_plugins_multi:
        interface: >-
          Interface: "eth0"
        disk: >-
          Disk: "sda"
        nginx: >-
          URL: "http://{{ ansible_lo.ipv4.address }}/nginx_status"
        network: >-
          <Server "1.2.3.4">
            SecurityLevel "Encrypt"
            Username "CHANGEME"
            Password "1rht44tg2i"
          </Server>
      tags: collectd
```
Role Variables
--------------

```yaml
collectd_packages_RedHat:
 - { package: "collectd" }
 - { package: "collectd-apache" }
 - { package: "collectd-mysql" }

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
