# my-ansible-isu-templete

## ISUCON my templete
webapp:
  ref: private-isu

### shell
```
# archive
tar zcvfp /tmp/webapp.tar.gz /home/isucon/private_isu/webapp

# mysqldump
mysqldump -u isuconp isuconp | gzip > /tmp/isuconp.dump.sql.gz 

# scp
scp ubuntu@x.x.x.x:/tmp/webapp.tar.gz ./
scp ubuntu@x.x.x.x:/tmp/isuconp.dump.sql.gz ./
scp ubuntu@x.x.x.x:/etc/nginx/nginx.conf ./

# remove
rm -f /tmp/webapp.tar.gz
rm -f /tmp/isuconp.dump.sql.gz
```

### bench
```
# bench (user:isucon)
/home/isucon/private_isu.git/benchmarker/bin/benchmarker -u /home/isucon/private_isu.git/benchmarker/userdata -t http://192.168.11.41
```

### collect
```
cat <<EOF > /tmp/pattern.txt
apparmor.service
apport.service
blk-availability.service
cloud-config.service
cloud-final.service
cloud-init-local.service
cloud-init.service
console-setup.service
cron.service
dbus.service
finalrd.service
getty@tty1.service
irqbalance.service
keyboard-setup.service
kmod-static-nodes.service
lvm2-monitor.service
multipathd.service
networkd-dispatcher.service
packagekit.service
plymouth-quit-wait.service
plymouth-quit.service
plymouth-read-write.service
polkit.service
rsyslog.service
serial-getty@ttyS0.service
setvtrgb.service
snapd.apparmor.service
snapd.seeded.service
snapd.service
ssh.service
systemd-fsck-root.service
systemd-fsck@dev-disk-by\x2dlabel-UEFI.service
systemd-journal-flush.service
systemd-journald.service
systemd-logind.service
systemd-machine-id-commit.service
systemd-modules-load.service
systemd-networkd-wait-online.service
systemd-networkd.service
systemd-random-seed.service
systemd-remount-fs.service
systemd-resolved.service
systemd-sysctl.service
systemd-sysusers.service
systemd-timesyncd.service
systemd-tmpfiles-setup-dev.service
systemd-tmpfiles-setup.service
systemd-udev-trigger.service
systemd-udevd.service
systemd-update-utmp.service
systemd-user-sessions.service
ufw.service
unattended-upgrades.service
user-runtime-dir@1000.service
user@1000.service
EOF

systemctl list-units --all --type=service | grep -v inactive > /tmp/systemd-info.txt
grep -vf /tmp/pattern.txt /tmp/systemd-info.txt > /tmp/result.txt

# scp
scp ubuntu@x.x.x.x:/tmp/result.txt ./

# remove
rm -f /tmp/pattern.txt /tmp/systemd-info.txt /tmp/result.txt
```
