#################
GlusterFS cluster
#################

Deploy HAProxy, setup front/back services

Maintainer
==========

Flavien Hardy <flavien.hardy@objectif-libre.com>

Requirements
============

None

Role Vars
=========

Lot of vars... Please see <file://defaults/main.yml>.

Support for `http` and `tcp` modes.

Be careful with logging options, default is to log into `local0` facility, Debian family add a configuration file for rsyslog, RedHat family doesn't.

Dependencies
============

None

Distribution Support
====================

.. csv-table::
   :header: Distribution,Supported,Tested

   Ubuntu 16.04,Yes,Manually
   CentOS 7,Yes,Manually

Playbook examples
=================

Simple:

.. code-block:: yaml

   - hosts: haproxy
     become: yes
     roles:
       - haproxy

Service definition with front & back:

.. code-block:: yaml

    - hosts: haproxy
      roles:
        - role: haproxy
          haproxy_defaults_mode: tcp
          haproxy_defaults_options:
            - tcplog
            - dontlognull
          haproxy_services:
            - name: docker-services
              frontends:
                - name: main
                  address: "*"
                  port: 80
                  acls:
                    - name: "url_myfirstservice"
                      criterion: "hdr(host)"
                      flags: ""
                      operators: "-i"
                      value: "localhost"
                  use_backend: "main_back"
              backends:
                - name: main_back
                  balance: "roundrobin"
                  servers:
                    - name: serv1
                      address: "127.0.0.1"
                      port: 8080


TODO
====

* Better documentation (ex: more services examples).
* Add support for `Listen` directive.
