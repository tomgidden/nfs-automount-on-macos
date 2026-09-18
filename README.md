# NFS automount on macOS Golden Gate

Keeping automount NFS shares working on _macOS_ is a constantly-changing target. This setup seems to work for me in _macOS 27 (Golden Gate)_ and on _Tahoe_ before that.

I still use NFS as SMB doesn't really function the way I'd like, and I think NFS performance is still better. I've tried switching to SMB more than a few times, and each time I end up going back to NFS, which is a pity because some of the more configurable features of Samba (eg. being able to veto files like `._*` and `.DS_Store`) would be valuable.

For the server I'm using TrueNAS 25.04 with my main RAID-Z3 exported on `/mnt/raid1`

## Installation

1. Edit `etc/auto_nfs` to taste
2. `sudo install -m 700 -o root etc/auto_nfs /etc/auto_nfs`
3. `sudo dscl . -create "/Automount/\/nfs"`
4. `sudo dscl . -create "/Automount/\/nfs" 'dsAttrTypeStandard:MetaAutomountMap' auto_master`
5. `sudo dscl . -create "/Automount/\/nfs" AutomountInformation "auto_nfs -nobrowse,hidefromfinder,nosuid"`

and if you're adding the optional synthetics:
1. Edit `etc/synthetic.d/shortcuts.conf` as needed
2. `sudo install -m 755 -o root -d /etc/synthetic.d`
3. `sudo install -m 644 -o root etc/synthetic.d/shortcuts.conf /etc/synthetic.d/shortcuts.conf`
4. `/System/Library/Filesystems/apfs.fs/Contents/Resources/apfs.util -t`

**IMPORTANT: Don't do the `apfs.util` line without first editing the `shortcuts.conf` file;** once that's run, the new links will persist until you restart the machine, and you cannot delete them at runtime!

## History

[An earlier revision of this](https://gist.github.com/tomgidden/1bf13ce0c5e4cf234c4fd6505031839a/2bc2ebc5eba7fcff229e3ce73f54285075b02db4) edited the `/etc/auto_master` file directly, which works, but then requires re-patching after OS updates.  [feoh/macos-nas-automount](https://github.com/feoh/macos-nas-automount) uses a Directory Services approach using the main `+auto_master` map instead, which I've adopted in the instructions above.

That repo might be more useful for what you're trying to achieve, by the way, but my approach is more suitable for my purposes, albeit improved by borrowing the `dscl` commands!
