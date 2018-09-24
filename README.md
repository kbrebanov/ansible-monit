[![No Maintenance Intended](http://unmaintained.tech/badge.svg)](http://unmaintained.tech/)

monit
=====

[![Build Status](https://travis-ci.org/kbrebanov/ansible-monit.svg?branch=master)](https://travis-ci.org/kbrebanov/ansible-monit)

Installs and configures Monit.

Requirements
------------

This role requires Ansible 1.9 or higher.

Role Variables
--------------

| Name                          | Default                         | Description                                              |
|:------------------------------|:--------------------------------|:---------------------------------------------------------|
| monit_check_services_interval | 120                             | Check services at this interval (in seconds)             |
| monit_start_delay             | 0                               | Delay the first check by this interval (in seconds)      |
| monit_mailservers             | []                              | List of mail servers to use                              |
| monit_alert_email             | ''                              | Global email used for receiving alerts                   |
| monit_alert_email_not_on      | []                              | List of events not to send alerts for                    |
| monit_eventqueue_slots        | 100                             | Event queue size                                         |
| monit_mail_format_from        | monit@$HOST                     | Email alert from address                                 |
| monit_mail_format_subject     | monit alert --  $EVENT $SERVICE | Email alert subject                                      |
| monit_mail_format_message     | See examples for default        | Email alert message body                                 |
| monit_httpd_enabled           | false                           | Enable embedded HTTP interface                           |
| monit_httpd_port              | 2812                            | HTTP interface port                                      |
| monit_httpd_address           | localhost                       | Address HTTP interface will listen on                    |
| monit_httpd_allow             | ["localhost", "admin:monit"]    | Addresses and/or users to allow access to HTTP interface |

Dependencies
------------

None

Example Playbook
----------------

Install monit
```yaml
- hosts: all
  roles:
    - name: kbrebanov.monit
```

Install monit and configure mail servers and mail format
```yaml
- hosts: all
  vars:
    monit_mailservers:
      - name: mail.bar.baz
        port: 25
      - name: backup.bar.baz
        port: 10025
      - name: localhost
        port: 25
    monit_mail_format_from: monit@$HOST
    monit_mail_format_subject: monit alert --  $EVENT $SERVICE
    monit_mail_format_message: |
      $EVENT Service $SERVICE
        Date: $DATE
        Action: $ACTION
        Host: $HOST
        Description: $DESCRIPTION

        Your faithful employee,
        Monit
  roles:
    - name: kbrebanov.monit
```

Install monit and configure global email for receiving alerts
```yaml
- hosts: all
  vars:
    monit_alert_email: alerts@example.com
    monit_alert_email_not_on:
      - action
      - instance
```

License
-------

BSD

Author Information
------------------

Kevin Brebanov
