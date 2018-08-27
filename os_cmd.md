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
