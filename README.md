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

Personal Ansible role repositories related to this project will be linked here:

- [ansible-role-abcde](https://github.com/WhaleJ84/ansible-role-abcde)
- [ansible-role-apple-superdrive](https://github.com/WhaleJ84/ansible-role-apple-superdrive)
- [ansible-role-bash](https://github.com/WhaleJ84/ansible-role-bash)
- [ansible-role-firefox](https://github.com/WhaleJ84/ansible-role-firefox)
- [ansible-role-git](https://github.com/WhaleJ84/ansible-role-git)
- [ansible-role-gnome-desktop](https://github.com/WhaleJ84/ansible-role-gnome-desktop)
- [ansible-role-gnome-terminal](https://github.com/WhaleJ84/ansible-role-gnome-terminal)
- [ansible-role-gns3](https://github.com/WhaleJ84/ansible-role-gns3)
- [ansible-role-keepassxc](https://github.com/WhaleJ84/ansible-role-keepassxc)
- [ansible-role-lxml](https://github.com/WhaleJ84/ansible-role-lxml)
- [ansible-role-nftables](https://github.com/WhaleJ84/ansible-role-nftables)
- [ansible-role-pip](https://github.com/WhaleJ84/ansible-role-pip)
- [ansible-role-psutil](https://github.com/WhaleJ84/ansible-role-psutil)
- [ansible-role-pyenv](https://github.com/WhaleJ84/ansible-role-pyenv)
- [ansible-role-signal](https://github.com/WhaleJ84/ansible-role-signal)
- [ansible-role-syncthing](https://github.com/WhaleJ84/ansible-role-syncthing)
- [ansible-role-timeshift](https://github.com/WhaleJ84/ansible-role-timeshift)
- [ansible-role-timeshift-autosnap-apt](https://github.com/WhaleJ84/ansible-role-timeshift-autosnap-apt)
- [ansible-role-tmux](https://github.com/WhaleJ84/ansible-role-tmux)
- [ansible-role-vim](https://github.com/WhaleJ84/ansible-role-vim)

## Development Notes

Any development notes relevant to this repository or any related ansible role repository will be kept here.

### Tags

Tags are catorised into the following sections:

- Shared
- Role Specific

#### Shared

Shared tags used in playbooks should be documented here.
Shared tags are used in all playbooks and should not be specific to a single role and therefore should be condisidered as reliable for filtering.
The below shared tags can be used inconjuction with `--skip-tags` to fine-tune operation:

- install
- configure

#### Role Specific

Role specific tags are used to denote sections that are only applicable to their respective role.
For example, `whalej84.example` should have a tag of `example` (omitting everything before the first full-stop).

Used in conjunction with shared tags, operation can be specified for areas of interest.
For example, if only the **whalej84.example** role needed to be configured but not installed, `--tags example --skip-tags install` would do just that.
This would rely on only the *install* and *configure* shared tags existing.
If more tags existed, you would need to ignore all irrelevant tags.
It should follow a structure of `--tags $ROLE_SPECIFIC --skip-tags $SHARED`.

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
