# Ansible Role Samba4

## Overview

This Ansible role configures and manages Samba on Linux systems, supporting both Samba4 AD DC Provisioning aswell as joining existing domains and member server configurations.

## Requirements

- **Ansible:** 2.9+
- **Tested on:** Ubuntu

## Example playbook

```yaml
- hosts: servers
  become: true
  roles:
    - role: ansible-role-samba
      vars:
        samba_role: "ad_dc_provision"
        samba_domain: "example.com"
        samba_realm: "EXAMPLE.COM"
        samba_admin_password: "YourStrongPassword"
        samba_dns_backend: "BIND9_DLZ"
        samba_config:
          workgroup: "WORKGROUP"
          netbios_name: "MYADDC"
          log_file: "/var/log/samba/%m.log"
          log_level: 3
          max_log_size: 2000
          security: "ADS"
          passdb_backend: "tdbsam"
          printing: "cups"
          printcap_name: "cups"
          load_printers: "yes"
          disable_spoolss: "no"
          show_additional_options: true
        samba_printers:
          enabled: true
          printer_path: "/var/spool/samba"
          printer_browseable: "yes"
          printer_guest_ok: "yes"
          printer_writable: "yes"
          printer_printable: "yes"
        samba_advanced_options:
          map to guest: "Bad User"
          guest account: "nobody"
        samba_users:
          - name: "jdoe"
            password: "SuperSecretPassword"
            comment: "John Doe"
        samba_shares:
          - name: "public"
            path: "/srv/samba/public"
            browseable: "yes"
            guest_ok: "yes"
            read_only: "no"
            additional_options:
              create mask: "0644"
              directory mask: "0755"
          - name: "private"
            path: "/srv/samba/private"
            browseable: "no"
            guest_ok: "no"
            read_only: "no"
            owner: "jdoe"
            group: "jdoe"
            mode: "0700"
            additional_options:
              create mask: "0600"
              directory mask: "0700"
        samba_home_directories:
          enabled: true
          path: "/home/%U"
          browseable: "no"
          writable: "yes"
          create_mask: "0700"
          directory_mask: "0700"

```
