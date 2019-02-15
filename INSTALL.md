Installation
=========

To use this Ansible role skeleton, as [described in Ansible Galaxy documentation](https://docs.ansible.com/ansible/latest/reference_appendices/galaxy.html#using-a-custom-role-skeleton):

```
ansible-galaxy init --role-skeleton=ansible-role-skeleton role_name
cd ./role_name/
find ./molecule -name 'playbook.yml' -exec sed -i -e '$a\' {} \;
find ./molecule -name 'molecule.yml' -exec sed -i -e '$a\' {} \;
find ./vars -name '*.yml' -exec sed -i -e '$a\' {} \;
find ./ -name .travis.yml -exec sed -i -e '$a\' {} \;
find ./meta -name main.yml -exec sed -i -e '$a\' {} \;
find ./handlers -name main.yml -exec sed -i -e '$a\' {} \;
find ./defaults -name main.yml -exec sed -i -e '$a\' {} \;
rm INSTALL.md
```

or add this to ansible.cfg:

```
[galaxy]
role_skeleton = /path/to/skeleton
role_skeleton_ignore = ^.git$,^.*/.git_keep$
```

Followed by `ansible-galaxy init role_name`.

Don't include `ansible-role` to the role name, for example use `java` instead of `ansible-role-java`.

To allow Slack notifications, set `slack_token` as a (secret) variable in Travis CI.
