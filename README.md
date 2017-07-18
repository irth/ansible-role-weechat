# `weechat` role for ansible

## What is this?
An ansible role that installs and configures weechat

## Requirements
`apt` on the server, with `weechat` available in the repos

## Usage
include the role in your playbook, e.g.

```yaml
- hosts: ircbox.example.com
  roles:
    - weechat
```

Specify the config in `host_vars` or wherever else you want.

## Arguments
* **weechat_dir** *(default: .weechat)* - the dir for weechat config
* **weechat_plugins** *(optional)* - list of plugins to install
  ```yaml
  weechat_plugins:
    - go.py
    - iset.pl
  ```
* **weechat\_settings, weechat\_settings\_encrypted, weechat_secure** - dicts with settings, see below for detail
* **weechat\_secure\_password** - if set, it will be used to encrypt weechat's sec.conf
* **weechat\_commands, weechat\_commands\_encrypted** - lists of commands to run (include the leading slash, `/command`)

## Configuration
The format for `weechat_settings` and `weechat_settings_encrypted` is as follows:

```yaml
weechat_settings:
  foo.bar.baz: 1
```

you can't insert new options (basically it can only do what /set can)

don't manage `sec.*` from here, as it will be overwriten anyway


**weechat\_settings\_encrypted** is provided for convenience, it's merged with **weechat_settings**. You can store it in a separate, Vault-encrypted vars file to hide some of the config that's sensitive but not suitable for storing in weechat's `sec.conf`.

**weechat_secure** is a simple dict, it's values will be stored in sec.conf.
```yaml
weechat_secure:
  foo: bar
```
To access `foo`'s value in `weechat_settings*` use `${sec.data.foo}`.
Make sure to set `weechat_secure_password` so that the data is encrypted at rest.