# BTRFS Systemd Units #

This is a simple collection of `systemd` unit files for various tasks maintenance tasks for BTRFS file systems which are desirable to automate.

_Caution:_ I have not been able to find good documentation about what may occur if two btrfs operations, such as `balance` and `scrub`, overlap with each other. Until I can find a clear answer on this or have sufficient experimental evidence to show that it is _unsafe_ these unit files should be considered experimental and used with caution.

# Usage #

The provide units do the following,

* `btrfs-balance@.service`
    * A template unit which will perform the btrfs balance operation on the btrfs volume at the path given as the systemd unit template parameter.
    * Example: `systemctl start btrfs-balance@home.service` assuming `/home` is a btrfs volume.
    * Example: `systemctl start btrfs-balance@-.service` which will perform a balance on `/`. `-` is how `systemd` encodes the `/` path in template units.
* `btrfs-scrub@.service`
    * A template unit which will perform the btrfs scrub operation on the btrfs volume at the path given as the systemd unit template parameter.
    * Example: `systemctl start btrfs-scrub@home.service` assuming `/home` is a btrfs volume.
* `btrfs-scrub@.timer`
    * A template timer unit which will trigger the `btrfs-scrub@.service` at a monthly interval. You can customized the timer values using the standard `systemd` method of drop in units.
    * Example: `systemctl enable btrfs-scrub@home.timer`, this will trigger a scrub of `/home` once a month.
* `btrfs-snapshot@.service`
    * A template unit which will create a read-only snapshot of the btrfs subvolume specified by the `systemd` unit template value. These snapshots will be saved in `/var/lib/btrfs-snapshots` by default, but this value can be changed by overriding the `BTRFS_SNAPSHOT_PATH` environment variable for the unit. Each snapshot will be suffixed with [ISO 8601][iso8601] date time, with a resolution to the second.
    * Example: `systemctl start btrfs-snapshot@home.service` assuming `/home` is a btrfs subvolume.
* `btrfs-snapshot@.timer`
    * A template timer unit which runs the `btrfs-snapshot@.service` unit against the btrfs subvolume at the path given by the template parameter. By default this will run daily, but this can be override with a standard `systemd` drop in unit.
    * Example: `systemctl enable btrfs-snapshot@home.timer` will create a read-only snapshot of `/home` saved to `/var/lib/btrfs-snapshost`, assuming it is a btrfs subvolume, every day.

# Installation #

*You should probably use a package manager.*

```shell
# ./configure
# make install
```

This will install the units into the standard `systemd` folder for package provided units, e.g. `/usr/lib/systemd/system`.

[iso8601]: https://en.wikipedia.org/wiki/ISO_8601 "ISO 8601"
