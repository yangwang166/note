# Ansible General

Ensure there is an file

```
- name: ensure file exists
  copy:
    content: ""
    dest: /your_file
    force: no
    group: user_group
    owner: user_name
    mode: 0644
```
