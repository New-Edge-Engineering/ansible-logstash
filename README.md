# Ansible logstash Role

Ansible role to install and configure logstash on RedHat/CentOS Debian/Ubuntu.
Feedback, bug-reports, requests, is welcomed and can be done via
[github issues](https://github.com/New-Edge-Engineering/ansible-ansible/issues).

Note that this role installs a syslog grok pattern by default; if you want to
add more filters, please add them inside the `/etc/logstash/conf.d/` directory.
As an example, you could create a file named `13-myapp.conf` with the appropriate
grok filter and restart logstash to start using it. Test your grok regex using
the [Grok Debugger](http://grokdebug.herokuapp.com/).

- Tested on Mac OS X with Ansible 1.5.

## Requirements

- Java
- Though other methods are possible, this role is made to work with Elasticsearch
  as a backend for storing log messages.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    logstash_listen_port_tcp: 5000
    logstash_listen_port_udp: 5000

The TCP and UDP ports over which logstash will listen for syslog messages.

    logstash_elasticsearch_host: localhost

The host on which Elasticsearch resides.

    logstash_ssl_dir: /etc/pki/logstash
    logstash_ssl_certificate_file: logstash-forwarder-example.crt
    logstash_ssl_key_file: logstash-forwarder-example.key

SSL configuration for Logstash to accept requests from logstash-forwarder running on remote hosts. **Security note**: On production or public-facing (e.g. any non-test) servers, you should create your own key/certificate pair and use that instead of the included default! You can use OpenSSL to create the key and certificate files, with a command like the following: `sudo openssl req -x509 -batch -nodes -days 3650 -newkey rsa:2048 -keyout logstash-forwarder.key -out logstash-forwarder.crt`.

    logstash_local_syslog_path: /var/log/syslog
    logstash_monitor_local_syslog: true

Whether configuration for local syslog file (defined as `logstash_local_syslog_path`) should be added to logstash. Set this to `false` if you are monitoring the local syslog differently, or if you don't care about the local syslog file. Other local logs can be added by your own configuration files placed inside `/etc/logstash/conf.d`.

## Dependencies

  - geerlingguy.elasticsearch

## Example Playbook

    - hosts: search
      roles:
        - role: smola.java
        - role: neel.elasticsearch
        - role: neel.logstash

## License

Licensed under the MIT License. See the LICENSE file for details.

