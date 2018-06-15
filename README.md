# GPG Keys Role for Ansible

This is a role which creates trusted private / public GPG keys

## Variables

Variables for this module should be stored under the `group_vars` as GPG keys will be applied to all hosts in the group: Here is an example variables file for the Vagrant environment public keys. This can be done outside of Vault as they are public keys

```
---
  gpg
    directory: /root/.gnupg
    public:
      <KEYNAME>:
        id: <KEYID>
        content: |
          <KEYCONTENT>
```

### Ansible Vault

Due to the sensitive nature of this module it leverages Ansible Vault to keep private keys secure. Here is an example variables file after decryption:

```
---
  vault_gpg
    private:
      <KEYNAME>:
        id: <KEYID>
        content: |
          <KEYCONTENT>
```

All secure variables are stored within a `vault.yml` file and are prefixed with `vault_`

#### Working with the Vault

1. Add the following line to `ansible.cfg` - `vault_password_file = ./.vault_pass`
1. Create `.vault_pass` file - **This has been ignored from version control**
1. Populate it with the passwords used to encrypt files

When you run Anisble it will read the `.vault_pass` file when needing access to secure variables. This is to avoid needing to input a password. Alternatively `--ask-vault-pass` can be used. When working with the Vault be sure to re encrypt the sensitive data before commiting! Due to the nature of Ansible Vault encryption output will be different each time so files will need to be recommitted

##### Useful commands

`ansible-vault decrypt vault.yml` - Decrypts the Vault file with the stored password
`ansible-vault encrypt vault.yml` - Encrypts the Vault file with the stored password
`ansible-vault edit vault.yml` - Decrypts and opens the Vault file for editing

## Example Task with role

```
- name: gpgkeys
  hosts: all
  roles:
    - edr.gpgkeys
```
