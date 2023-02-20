# Managing disks

## Mounting disks

- [Mounting ntfs on linux](https://linuxconfig.org/how-to-mount-partition-with-ntfs-file-system-and-read-write-access)

### Errors

There may be metadata on the disk from windows that linux doesn't understand or you get an error that the disk is unclean. This can be fixed by running [`ntfsfix`](https://askubuntu.com/questions/462381/cant-mount-ntfs-drive-the-disk-contains-an-unclean-file-system) on the disk.

If it complains about windows hibernation perform complete shutdown with a comandline comand: `shutdown /s`
[Form link to solution](https://askubuntu.com/questions/462381/cant-mount-ntfs-drive-the-disk-contains-an-unclean-file-system)
