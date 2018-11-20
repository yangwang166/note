# OS related command

## Check OS type

`cat /etc/redhat-release`

## Shutdown Server immediately

`shutdown 0`

## Increase history of bash

`echo 'export HISTTIMEFORMAT="%d/%m/%y %T "' >> ~/.bash_profile`
`echo 'export HISTSIZE=10000' >> ~/.bash_profile`

## List user

`awk -F':' '{ print $1}' /etc/passwd`

## lsof usage

To see which process using port

> `lsof -i <port>`

To see the process using which files:

> `lsof -c httpd`

To see which process using the file

> `lsof file_name`

Ref: [lsof Command Usage and Examples](https://www.slashroot.in/lsof-command-usage-and-examples)

## Check GPU device

To check device
> `/sbin/lspci`

To Check driver

> `navidia-smi`

## yum

Check package version

> `yum info package_name`

Redhat Satellite repo, list enabled repo

> `subscription-manager repos --list-enabled`

Show repo list, can know which status of repo whether enable or disable

> `yum repolist all`

Show what particular version of package are available

> `yum --showduplicates list package_name`

Install a particular version package

> `yum install <package name>-<version info>`

eg:

> `yum install <httpd>-<2.4.6-6>`

Enable a repo

> `subscription-manager repos --enable=repo_name`

Disable a repo

> `subscription-manager repos --disable=repo_name`

Find the external mounting point

> `cat /proc/mount | grep what_you_want`

## System monitoring

Watch memory change

> `watch -n 2 free -h`

## Tmux

Start a session

> `tmux new -s dev`

Attach a session

> `tmux attach -t dev`

## iostat

Useful to check the disk io and cpu usage

Check iostat 5 times and 1 sec interval

> `iostat 1 5`

## Change uid and gid of a user

* Foo’s old UID: 1005
* Foo’s new UID: 2005
* Our sample group name: foo
* Foo’s old GID: 2000
* Foo’s new GID: 3000

```bash
usermod -u 2005 foo
groupmod -g 3000 foo
find / -group 2000 -exec chgrp -h foo {} \;
find / -user 1005 -exec chown -h foo {} \;
```

