# Ansible role: image-builder

This Ansible role is used to generate a custom Red Hat Linux Enterprise (RHEL) image.

**Disclaimer**: This role was created to build RHEL image tailored to my homelab requirements. It might not support all available customizations ([see here](https://osbuild.org/docs/user-guide/blueprint-reference/)).

# Requirements

To build a RHEL image, you must have an `external RH username` or an `RH developer account`, as RHEL repositories are not publicly available.

**Note**: If you are trying to build a RHEL image on a Fedora host, you will need `subscription-manager` package, then register your system using `sudo subscription-manager register` with your credentials.

# Usage

requirements.yaml

```yaml
---
- name: image-builder
  scm: https://github.com/ht-vo/ansible-role-image-builder.git
``` 

playbook.yaml

```yaml
---
- hosts: localhost
  become: true
  gather_facts: yes

  vars:
    blueprint_name: 'rhel-base'
    blueprint_description: ''
    blueprint_version: '1.0.0'
    # Retrieve the list of supported distributions
    # Run the following command: composer-cli distros list
    blueprint_distro: 'rhel-9.5'

    # Set the hostname
    blueprint_hostname: 'vm1'

    # Set the system locale
    # Retrieve the values supported by languages
    # Run the following command: localectl list-locales
    blueprint_languages:
      - en_US.UTF-8
      - fr_FR.UTF-8

    # Set the keyboard layout
    # Retrieve the values supported by keyboard
    # Run the following command: localectl list-keymaps
    blueprint_keyboard: 'us'

    # Set the system time zone
    # Retrieve the values supported by timezone
    # Run the following command: timedatectl list-timezones
    blueprint_ntp_timezone: 'Europe/Paris'
    blueprint_ntp_servers:
      - 0.fr.pool.ntp.org
      - 1.fr.pool.ntp.org
      - 2.fr.pool.ntp.org
      - 3.fr.pool.ntp.org

    # Users to create
    blueprint_users:
      - name: user1
        password: user1
        ssh_key: ssh-ed25519 xxxx
        home: /home/user1
        shell: /usr/bin/bash
        groups: ["ssh-users", "users", "wheel"]

    # Groups to create
    blueprint_groups:
      - ssh-users
      - automation-users

    # Packages to install
    blueprint_packages:
      - jq
      - vim-enhanced
      - subscription-manager-cockpit
      - cockpit-storaged
      - cockpit-podman
      - cockpit-machines
      - libvirt
      - libguestfs-tools
      - qemu-kvm
      - virt-install
      - virt-top
      - virt-viewer

    # Check supported output formats
    # Run the following command: composer-cli compose types
    image_builder_type: 'image-installer'

  roles:
    - role: image-builder
```

## Author

[Thanh VO](mailto:thanh@thanh-vo.com)

## License

This project is [MIT](https://github.com/ht-vo/ansible-role-image-builder/blob/main/LICENSE) licensed.
