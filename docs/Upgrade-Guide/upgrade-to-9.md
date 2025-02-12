# Upgrade procedure to Gluster 9, from Gluster 8.x, 7.x and 6.x

We recommend reading the [release notes for 9.0](../release-notes/9.0.md) to be
aware of the features and fixes provided with the release.

> **NOTE:** Before following the generic upgrade procedure checkout the "**Major Issues**" section given below.

Refer, to the [generic upgrade procedure](./generic-upgrade-procedure.md) guide and follow documented instructions.

## Major issues

### The following options are removed from the code base and require to be unset

before an upgrade from releases older than release 4.1.0,

    - features.lock-heal
    - features.grace-timeout

To check if these options are set use,

```console
gluster volume info
```

and ensure that the above options are not part of the `Options Reconfigured:`
section in the output of all volumes in the cluster.

If these are set, then unset them using the following commands,

```{ .console .no-copy }
# gluster volume reset <volname> <option>
```

### Make sure you are not using any of the following deprecated features :

    - Block device (bd) xlator
    - Decompounder feature
    - Crypt xlator
    - Symlink-cache xlator
    - Stripe feature
    - Tiering support (tier xlator and changetimerecorder)
    - Glupy

**NOTE:** Failure to do the above may result in failure during online upgrades,
and the reset of these options to their defaults needs to be done **prior** to
upgrading the cluster.

### Deprecated translators and upgrade procedure for volumes using these features

[If you are upgrading from a release prior to release-6 be aware of deprecated xlators and functionality](https://docs.gluster.org/en/latest/Upgrade-Guide/upgrade_to_6/#deprecated-translators-and-upgrade-procedure-for-volumes-using-these-features).
