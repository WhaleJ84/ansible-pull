# Ansible Pull

Custom local.yml playbook and requirements file used to configure my personal system. Used in conjunction with [duct-tape](https://github.com/WhaleJ84/duct-tape/) repository

**WARNING:** Unless you are me, do not run any of the code you see here. This repository is only public for my own convenience reasons. Code within can be unstable at any moment and is not tested on any systems other than my own. You have been warned.

## Requirements

Any requirements should be automatically installed by running `duct-tape` linked above.
When developing and testing locally, the latest requirements can be pulled via `ansible-galaxy install -r requirements.yml $TARGET --force`.

## Usage

After successful completion of `duct-tape`, it will prompt you to run `ansible-pull -KU https://github.com/WhaleJ84/ansible-pull.git`.
If 'local.yml' has been downloaded locally, it can be ran via `ansible-playbook -K local.yml`.
To run specific roles in the playbook, you can filter them using tags.
For example, `--tags firefox --skip-tags install` will only configure Firefox (not install) and not run any other roles.

## Related Repositories

Personal Ansible role repositories used within this project will be linked here:

- [ansible-role-firefox](https://github.com/WhaleJ84/ansible-role-firefox)
- [ansible-role-keepassxc](https://github.com/WhaleJ84/ansible-role-keepassxc)
- [ansible-role-pyenv](https://github.com/WhaleJ84/ansible-role-pyenv)
- [ansible-role-abcde](https://github.com/WhaleJ84/ansible-role-abcde)

## Development Notes

Any development notes relevant to this repository or any related ansible role repository will be kept here.

### Git Hooks

- pre-commit
	- Lint and syntax check code (requires pip modules: yamllint ansible-lint).

```shell
#!/bin/sh

yamllint . || exit 1
ansible-playbook local.yml --syntax-check || exit 1
ansible-lint . || exit 1
``` 

### Ansible Facts

Gather facts on the relevant local systems using `ansible localhost -m ansible.builtin.setup`.

### Using local roles

To prevent requiring pushing changes to the remote role repo after every change and redownloading the role, a simpler method exists.
Create a local environment variable called `ANSIBLE_ROLES_PATH` and point it to the directory where the roles exist.
Ensure the directory names within match the role names in the playbooks, and they will take priority over the downloaded counterparts.
