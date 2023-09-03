# Infra
  Infra is an Ansible playbook used to set up and update a home server. The main goal of this project is to build a home server for storing data with backups and running Docker containers with access over the internet.

  This playbook is designed to set up all the necessary infrastructure, even on a fresh Ubuntu server. The infrastructure includes the following features:
  - Creation of a non-root user
  - Updated SSH security
  - Disk drive mounting
  - SnapRAID configuration
  - Samba file sharing over local network
  - Running Docker containers

## Installation instructions
  - Clone the repository
  - Create files from examples
  - Replace variables and secrets
  - Run `ansible-playbook run.yaml`

  To test, you can use a local virtual machine from the `multipass` library. To do so, turn on the variables in the `multipass` section to true. This will disable disk mounting and SnapRAID.

# Useful Commands

## Ansible
  - `ansible-playbook run.yml`
  - `ansible-vault edit group_vars/all/secret.yml`

## Multipass
  - `multipass launch --cloud-init multipass/cloud-init.yaml -c 4 -d 50G -m 4G -n ubutest --network name=en0,mode=manual,mac=C1:66:F1:GH:85:C4`
  - `multipass list`
  - `multipass delete`
  - `multipass purge`

## Docker
  - `docker ps` - containers info
  - `docker logs --tail 100 <container ID>`
  - `docker stop $(docker ps -a -q)`
  - `docker rm $(docker ps -a -q)`
  - `docker-compose up -d reverse-proxy`
  - `docker restart portainer`


## SnapRAID
  - `python3 /home/username/snapraid-runner/snapraid-runner.py --conf /etc/snapraid-runner.conf`
  - `snapraid sync`
  - `snapraid scrub`


## Deluge
  default user/password of admin/deluge.


## Filebrowser
  default user/password of admin/admin.


## SSH

### Generate new ssh key
  `ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/keyname -C email@mailbox.org`

### Put pbkey to the server
  `ssh-copy-id -i ~/.ssh/keyname username@192.168.000.000`

### Connect
  `ssh username@192.168.000.000 -p 22`


## Disk utils

### Mount
  `sudo mount UUID=C8C89BE5C89BD054 /mnt/folder`

### Get info
  - `df -a -T -h` size of all mounted disks
  - `parted -l` disks list with partitions
  - `blkid` disks list with labels, uuid, partuuid, fs type
  - `lshw -C disk` data specific for the devices
  - `du -hs /folder` get folder size recursively
  - `cp -vr /folder /destination` copy folder recursively

### Extend default LVM space
  - `lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv`
  - `resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv`

### ext4 Formating
  `mkfs.ext4 -b 4096 -M 0 -L Label /dev/folder`

  - -b (block size)
  - -M 0 (root user reserved size)
  - -L (fs label)
  - /dev/folder (path to dir)

  Extand file sistem options
  `tune2fs -O has_journal,extent,huge_file,flex_bg,uninit_bg,dir_nlink,dir_index /dev/folder`

  - has_journal (Enables the ext4 journal)
  - extent (Enables extents, see above)
  - huge_file (Enables files over 2GB in size)
  - flex_bg (Allows inode metadata to be placed anywhere on the partition, as opposed to traditionally at the start)
  - uninit_bg (Enables new blocks to not be initialised and enables block checksumming. Significantly speeds up creation and fsck)
  - dir_nlink (Enables unlimited sub directories)
  - dir_index (Enables a hashed B-tree for linking subdirectories)

  Change a disk label
  `e2label /dev/sdd Label`

### Test for new drive
  smartctl command from the smartmontools package:

  `smartctl /dev/sdd -H`
  `smartctl /dev/sdd -t conveyance`
  `smartctl /dev/sdd -t short`
  `smartctl /dev/sdd -t long`

  View test log
  `smartctl -a /dev/sdd`

  badblocks utility that will do read and write test for the disk and destroys all contents on the drive

  `badblocks /dev/sdd -wsv`