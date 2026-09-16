# NFS automount on macOS Golden Gate

Keeping automount NFS shares working on _macOS_ is a constantly-changing target.
This setup seems to work for me in _macOS 27 (Golden Gate)_ and on _Tahoe_ before
that.

I still use NFS as SMB doesn't really function the way I'd like, and I think NFS
performance is still better. I've tried switching to SMB more than a few times,
and each time I end up going back to NFS, which is a pity because some of the more
configurable features of Samba (eg. being able to veto files like `._*` and 
`.DS_Store`) would be valuable.

For the server I'm using TrueNAS 25.04 with my main RAID-Z3 exported on `/mnt/raid1`


