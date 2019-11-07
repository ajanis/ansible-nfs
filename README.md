# Ansible-NFS

This roles installs  NFS server and client configurations

<!-- MarkdownTOC -->

- Dependencies
- Requirements
- Role Variables
  - defaults/main.yml
- Example Playbooks
      - Configure an NFS server:
    - Configure NFS Client:
- License
- Author Information

<!-- /MarkdownTOC -->



## Dependencies

None

## Requirements

This role requires Ansible 2.8 or higher, and platform requirements are listed
in the metadata file.

Inventory Groups required for deploying Server components:
- nfsservers: NFS Server packages and configuration

If you have an OpenLDAP server deployed via the [ansible-openldap][] role, then you can configure AutoFS (automount) mount options for your NFS shares.

## Role Variables

The variables that can be passed to this role and a brief description about
them are as follows:

### defaults/main.yml
```
www_domain: "home.example.com"

### NFS CLIENT CONFIG
nfs_server_domain_name: "nfs.{{ www_domain }}"
nfs_server_ip: "10.253.228.33"
use_ldap_automount: False


### STORAGE DIRECTORY CONFIG
data_mount_root: "/data"
media_directory: "media"
www_directory: "public_html"

### NFS EXPORTS CONFIGS
nfs_export_root: "/exports"

nfs_exports:
  media:
    nfs_export: "{{ nfs_export_root }}/{{ media_directory }}"
    mount_opts: "*(rw,sync,no_root_squash,no_subtree_check)"
  public_html:
    nfs_export: "{{ nfs_export_root }}/{{ www_directory }}"
    mount_opts: "*(rw,sync,no_root_squash,no_subtree_check)"
```


## Example Playbooks

##### Configure an NFS server:

```
- name: Deploy NFS Server
  hosts: nfsservers
  remote_user: root
  tasks:
    - import_role:
        name: common
    - import_role:
        name: nfs
```

#### Configure NFS Client:
```
- name: Deploy NFS Client
  hosts: all
  remote_user: root
  tasks:
    - import_role:
        name: common
    - import_role:
        name: nfs

```


## License

MIT

## Author Information

Created by Alan Janis

[ansible-openldap]: https://github.com/ajanis/ansible-openldap.git
