# Ansbile role "papanito.diskmounter"  <!-- omit in toc -->

[![Ansible Role](https://img.shields.io/ansible/role/50326)](https://galaxy.ansible.com/papanito/diskmounter) [![Ansible Quality Score](https://img.shields.io/ansible/quality/50326)](https://galaxy.ansible.com/papanito/diskmounter) [![Ansible Role](https://img.shields.io/ansible/role/d/50326)](https://galaxy.ansible.com/papanito/diskmounter) [![GitHub issues](https://img.shields.io/github/issues/papanito/ansible-role-diskmounter)](https://github.com/papanito/ansible-role-diskmounter/issues) [![GitHub pull requests](https://img.shields.io/github/issues-pr/papanito/ansible-role-diskmounter)](https://github.com/papanito/ansible-role-diskmounter/pulls)

- [Requirements](#requirements)
- [Role Variables](#role-variables)
  - [General parameters](#general-parameters)
    - [Parameters for `smb_shares`](#parameters-for-smb_shares)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
- [License](#license)
- [Author Information](#author-information)


This role is used for simplified mounting of file shares.

> This is currently in an early stage and only works for smb shares

## Requirements

none

## Role Variables

### General parameters

These are all variables

|Parameter|Description|Default Value|
|---------|-----------|-------------|
|`smb_shares`|Dictionary of smb (cifs) shares to mount, see [Parameters for `smb_shares`](#parameters-for-smb_shares)|-|
|`smb_credentials_dir`|Directory where to put credentials file|`/etc`|

#### Parameters for `smb_shares`

`smb_shares` are always mounted with [type `cifs` and in state `mounted`](https://docs.ansible.com/ansible/latest/modules/mount_module.html). However there are some options to provide:

|Parameter|Description|Default Value|
|---------|-----------|-------------|
|`path`|[Mandatory] Path to the mount point|-|
|`src`|[Mandatory] Device to be mounted on `path`|-|
|`options`|Mount options (see fstab(5), or vfstab(4) on Solaris)|-|
|`mounted`|Whether the share shall be mounted (`yes`) or not (`no`)|`Yes`|
|`credentials`|If defined the role will add a credentials file at `/etc/sharename`. The role will aggregate the credential file to the `options` as `-o credentials=/etc/sharename`.|-|
|`credentials.username`|Username to connect to `src`, will be added to credentials file|-|
|`credentials.password`|Password to connect to `src`, will be added to credentials file. I recommend to encrypt the password|-|
|`credentials.domain`|Domain to connect to `src`, will be added to credentials file|-|

`sharename` represent the key for each share at `smb_shares` i.e. for the example below we have `demo` and `freenas`


**Examples:**

```yml
smb_shares:
  demo: # share which uses no credentails
    path: /mnt/demo
    src: //192.168.0.1/demo
    options: file_mode=0777,dir_mode=0777,rw,uid=1000,gid=1000
    mounted: no # will unmount
  freenas: # share with credentials
    path: /mnt/freenas
    src: //192.168.0.1/media
    options: file_mode=0777,dir_mode=0777,rw,uid=1000,gid=1000
    credentials:
      username: usera
      passsword: MySmbPassword
      domain: domain
      mounted: yes
```

> **Remark**
>
> I recommend to use `ansible-vault` to encrypt `username` and `password`

## Dependencies

none

## Example Playbook

The following example installs an ssh-tunnel for each `server`

```yaml
---
- hosts: localhost
  vars:
    smb_shares:
      demo: # share which uses no credentials
        path: /mnt/demo
        src: //192.168.0.1/demo
        options: file_mode=0777,dir_mode=0777,rw,uid=1000,gid=1000
      freenas: # share with credentials
        path: /mnt/freenas
        src: //192.168.0.1/media
        options: file_mode=0777,dir_mode=0777,rw,uid=1000,gid=1000
        credentials:
          username: usera
          passsword: MySmbPassword
          domain: domain

 roles:
    - papanito.diskmounter
```

## License

This is Free Software, released under the terms of the Apache v2 license.

## Author Information

Written by [Papanito](https://wyssmann.com) - [Gitlab](https://gitlab.com/papanito) / [Github](https://github.com/papanito)