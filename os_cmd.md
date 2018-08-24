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
