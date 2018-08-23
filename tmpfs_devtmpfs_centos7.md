# Understand the tmpfs and devtmpfs in CentOS 7

When you use `df -hP` you got following

```
[root@host /]# df -hP
Filesystem                     Size  Used Avail Use% Mounted on
/dev/mapper/rootvg-root         10G  2.5G  7.6G  25% /
devtmpfs                       189G     0  189G   0% /dev
tmpfs                          189G   12K  189G   1% /dev/shm
tmpfs                          189G  3.1G  186G   2% /run
tmpfs                          189G     0  189G   0% /sys/fs/cgroup
tmpfs                           38G     0   38G   0% /run/user/2005
tmpfs                           38G     0   38G   0% /run/user/1554385803
/dev/sdb2                     1014M  162M  853M  16% /boot
/dev/sdb1                      256M  9.8M  246M   4% /boot/efi
/dev/mapper/data1vg-kafka      400G   33M  400G   1% /kafka
/dev/mapper/data1vg-zookeeper  300G  808M  300G   1% /zookeeper
/dev/mapper/data1vg-hdp        2.6T  292G  2.3T  12% /hdp
/dev/mapper/data1vg-tmp        100G   96M  100G   1% /tmp
/dev/mapper/data1vg-usr_hdp    100G  6.2G   94G   7% /usr/hdp
/dev/mapper/rootvg-var         8.0G  3.3G  4.8G  41% /var
/dev/mapper/rootvg-opt         197G  1.4G  195G   1% /opt
/dev/mapper/data1vg-var_log    100G   35G   66G  35% /var/log
/dev/mapper/rootvg-home        4.0G  109M  3.9G   3% /home
```

You may ask what is the `tmpfs` and `devtmpfs` partition, and why they use so much space.

Where is `tmpfs` actually located:
* `tmpfs` is a temporary filesystem that resides in memory and/or your swap partition(s), depending on how much you fill it up.

Purpose:
* Mounting directories as tmpfs can be an effective way of speeding up accesses to their files, or to ensure that their contents are automatically cleared upon reboot.

Really use much memory?
* Your `devtmpfs` and `tmpfs` filesystems are not in reality using gigabytes of memory;
* They can potentially grow to the upper limit, eg: 189GB, however that size is just the upper limit of their growing.
* That upper limit is also configurable, and is not what they use; they only eat the part of the RAM where they have content.
* You can see from the above, they actually only use very little of RAM

What are they specifically?
* `/dev`: contains device files which are created and removed automatically by the udev daemon, as hardware is added or removed etc. (devtmps is just a tmpfs that was created specially by the kernel early in the boot process, which contains the core devices pre-created so that the boot process has something to work with before udevd is loaded.)
* `/dev/shm`: is used by the POSIX shared memory facilities.
* `/run`: contains resource locks and PID files etc. which are relevant to currently-running daemons. `/var/run` and `/var/lock` are symlinks back to `/run` for compatibility reasons.
* `/sys/fs/cgroup`: contains details for the cgroup system, which is used (mainly by systemd) to divide processes into groups for resource sharing etc.
* may also have `/media`: contains the mount-points of removable media (e.g. optical discs and USB drives), which are created and removed automatically. 
