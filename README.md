Ansible Role: NUT
=================
[![Build Status](https://travis-ci.org/ntd/ansible-role-nut.svg?branch=master)](https://travis-ci.org/ntd/ansible-role-nut)
[![License](https://img.shields.io/github/license/jdm12989/ansible-role-nut)](LICENSE)

Installs and configures [NUT](http://networkupstools.org/) (Nework UPS
tools) on Debian based systems.

Role Variables
--------------

Available variables are listed below, along with default values (see
`defaults/main.yml`):

    nut_managed_config: true

If this is set to false, none of the following options will have any
effect, that is any and all changes under `/etc/nut/` will be your
responsibility. This is often desirable when you have complex
configurations.

    nut_host: localhost
    nut_user: monitor
    nut_password: Whatever...

## Operation Modes
Used to configure the operation of NUT services.

    nut_mode: standalone

> `none`: not configured, must be started through external commands
>
> `standalone`: (default) run driver, daemon, and monitor services, no network access
>
> `netserver`: same as standalone, but with network access
>
> `netclient`: only run the monitor service

## UPS Configuration
Mainly used for configuring the monitor user. A user in the NUT sense is
*not* the typical user a UNIX administrator is used to.

    nut_ups:
      - name: UPS
        driver: riello_ups
        device: /dev/ttyUSB0
        description: Some descriptive information
        extra: |
          maxretry = 10
          retrydelay = 1

`name` is an arbitrary string that must identify univocally the UPS.

`driver` depends on your hardware and must be one of the [available NUT
driver](http://networkupstools.org/stable-hcl.html). Be sure the NUT
version installed on your server has that specific driver available.

`device` is device where the UPS is listening (typically an USB port or
a serial device).

`description` is optional and is an arbitrary string used for debugging
and reporting purposes. 

`extra` is an optional multiline text to be appended verbatim at the end
of the UPS configuration block.

Example Playbook
================

    - hosts: all
      roles:
      - role: ntd.nut
        nut_ups:
          - name: riello
            driver: riello_usb
            device: /dev/ups
            description: iPlug 800
