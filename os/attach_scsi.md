# How to attach a SCSI device

I create volumes on PureStorage FlashArray device. Then attach the volumes to nodes.

```bash 
yum install iscsi-initiator-utils -y
yum install lsscsi
yum install device-mapper-multipath
chkconfig iscsid on
service iscsid start
chkconfig iscsi on
service iscsi start
cat /etc/iscsi/initiatorname.iscsi
# make sure to put this iqn into purestorage's gui dashboard host tab
iscsiadm -m discovery -t st -p 10.10.xx.xx # IP is the scsi device
iscsiadm -m node -T iqn.2010-06.com.purestorage:flasharray.4f4b4d8e5b18c762 -p 10.10.xx.xx -l
iscsiadm -m node --targetname iqn.2010-06.com.purestorage:flasharray.4f4b4d8e5b18c762 -p 10.10.xx.xx --login
lsscsi
iscsiadm -m node -T iqn.2010-06.com.purestorage:flasharray.4f4b4d8e5b18c762 -p 10.10.201.20 --op update -n node.startup -v automatic
fdisk /dev/sdc
  p #
  n # create new
  p # primary
  1 # 1 logical
  w # write
mkfs.xfs /dev/sdc1
mkdir /mnt/iscsi
blkid
cp /etc/fstab /etc/fstabBAK
vim /etc/fstab
# Add a line UUID="from {blkid}" /mnt/iscsi xfs _netdev 0 0
mount /mnt/iscsi
df -h /mnt/iscsi/
```̨