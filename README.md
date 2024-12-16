zpool import rpool -R /mnt -N -o readonly=on -f
zpool import bpool -R /mnt -N -o readonly=on -f

cryptsetup open -v /dev/zvol/rpool/keystore keystore-rpool
mount -o ro,noload /dev/mapper/keystore-rpool  /media
cat /media/system.key | zfs load-key -L prompt rpool
zfs mount $(zfs list | grep -m1 -e '^rpool\/ROOT\/ubuntu_' | awk '{print $1}' )
zfs mount $(zfs list | grep -m1 -e '^bpool\/BOOT\/ubuntu_' | awk '{print $1}' )
zfs mount -a
