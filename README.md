# ZFS Snap 

ZFS Snap is the idea of using ZFS+CRON+BASH to create a one-stop solution for automatic rolling snapshots.
This script was insipred by [AlBlue][http://alblue.bandlem.com/2008/11/crontab-generated-zfs-snapshots.html].

## License

ZFS Snap is free software and licensed under the GNU GPLv3 or later. You
are welcome to change and redistribute it under certain conditions. For more
information see the LICENSE file or visit http://www.gnu.org/licenses/gpl-3.0.html


## Running ZFS Snap


The script offers several switches to change the default behaviour, which is simply taking a snapshot with a timestamp.
Only a few standard programs are needed, which should be installed by default on any \*nix/Linux system. In my case
FreeBSD, which does not ship with bash by default, but can be installed from the ports.

Requirements:

```
zpool & zfs
tr
bash
cut
trail
e-/grep
date
sort
xargs
```

The only setup part is checking the header of the script and replacing the binary paths with the ones on your system using which. 
If you are running FreeBSD, this will probably work with the supplied script OOTB.

Options:

```
-d <default option> : hourly, daily, weekly, monthly, yearly
-l <label> default: Automatic
-p pretend, no snapshot creation or deletion
-d debug (not implemented)
-h help (not implemented) 
-r <number> : number of how many of those backups to retain ; default: 10
```

The -p switch emulates a 'pretend' action, printing out the actions taken by the script.
Five default behaviours have been implemented with following settings for label prefix and retention
hourly: AutoH 24 
daily: AutoD 7
weekly: AutoW 4
monthly: AutoM 12
yearly: AutoY 10

## Crontab

As you will most likely have to be logged in as root to use the zfs and zpool binaries. To change the crontab you will have to issue a
crontab -e
as root and enter following lines as  examples into your crontab editor to allow for the default behaviours

```
PATH=/etc:/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin
@hourly /root/zfs_round.sh -d hourly
@daily /root/zfs_round.sh -d daily
@weekly /root/zfs_round.sh -d weekly
@monthly /root/zfs_round.sh -d monthly
@yearly /root/zfs_round.sh -d yearly
```
The change to the PATH variable is optional, but necessary on FreeBSD. Other systems have not been tested.

## ZFS properties

I started my ZFS raid under Open Solaris and saw that my initial raid had a strange property that would
 not pop up anywhere apart from Solaris documentation: com.sun:auto-snapshot.
I use this property to determine if I back up a ZFS filesystem or not and therefore you will have to use this one
as well. I have prepared the script to make this an option, but could not think of another property to use.

## Shortcomings

Many, but I put this together with the best interest to users like me, who have a fairly small ZFS raid setup and who wish to keep
rolling snapshots without the need to worry about them too much.

## Finally

I hope this script is making other people's backup easier and maybe you have an idea on how to improve on this script, apart from that have fun! :)
