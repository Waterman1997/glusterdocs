
<a name="tuning-options"></a>

You can tune volume options, as needed, while the cluster is online and
available.

> **Note**
>
> It is recommended to set server.allow-insecure option to ON if
> there are too many bricks in each volume or if there are too many
> services which have already utilized all the privileged ports in the
> system. Turning this option ON allows ports to accept/reject messages
> from insecure ports. So, use this option only if your deployment
> requires it.

Tune volume options using the following command:

`# gluster volume set <VOLNAME> <OPT-NAME> <OPT-VALUE>`

For example, to specify the performance cache size for test-volume:

    # gluster volume set test-volume performance.cache-size 256MB
    Set volume successful

You can view the changed volume options using command:

`# gluster volume info`

The following table lists the Volume options along with its
description and default value:

> **Note**
>
> The default options given here are subject to modification at any
> given time and may not be the same for all versions.

Type | Option | Description | Default Value | Available Options
--- | --- | --- | --- | ---
 | auth.allow | IP addresses of the clients which should be allowed to access the volume. | \* (allow all) | Valid IP address which includes wild card patterns including \*, such as 192.168.1.\*
 | auth.reject | IP addresses of the clients which should be denied to access the volume. | NONE (reject none)  | Valid IP address which includes wild card patterns including \*, such as 192.168.2.\*
Cluster | cluster.self-heal-window-size | Specifies the maximum number of blocks per file on which self-heal would happen simultaneously. | 1 | 0 - 1024 blocks
 | cluster.data-self-heal-algorithm | Specifies the type of self-heal. If you set the option as "full", the entire file is copied from source to destinations. If the option is set to "diff" the file blocks that are not in sync are copied to destinations. Reset uses a heuristic model. If the file does not exist on one of the subvolumes, or a zero-byte file exists (created by entry self-heal) the entire content has to be copied anyway, so there is no benefit from using the "diff" algorithm. If the file size is about the same as page size, the entire file can be read and written with a few operations, which will be faster than "diff" which has to read checksums and then read and write. | reset | full/diff/reset
 | cluster.min-free-disk | Specifies the percentage of disk space that must be kept free. Might be useful for non-uniform bricks | 10% | Percentage of required minimum free disk space
 | cluster.min-free-inodes | Specifies when system has only N% of inodes remaining, warnings starts to appear in log files | 10% | Percentage of required minimum free inodes
 | cluster.stripe-block-size | Specifies the size of the stripe unit that will be read from or written to. | 128 KB (for all files) | size in bytes
 | cluster.self-heal-daemon | Allows you to turn-off proactive self-heal on replicated | On | On/Off
 | cluster.ensure-durability | This option makes sure the data/metadata is durable across abrupt shutdown of the brick. | On | On/Off
 | cluster.lookup-unhashed | This option does a lookup through all the sub-volumes, in case a lookup didn’t return any result from the hashed subvolume. If set to OFF, it does not do a lookup on the remaining subvolumes. | on | auto, yes/no, enable/disable, 1/0, on/off
 | cluster.lookup-optimize | This option enables the optimization of -ve lookups, by not doing a lookup on non-hashed subvolumes for files, in case the hashed subvolume does not return any result. This option disregards the lookup-unhashed setting, when enabled. | on | on/off
 | cluster.randomize-hash-range-by-gfid | Allows to use gfid of directory to determine the subvolume from which hash ranges are allocated starting with 0. Note that we still use a directory/file’s name to determine the subvolume to which it hashes | off | on/off
 | cluster.rebal-throttle | Sets the maximum number of parallel file migrations allowed on a node during the rebalance operation. The default value is normal and allows a max of [(((processingunits) − 4) / 2), 2] files to be migrated at a time. Lazy will allow only one file to be migrated at a time and aggressive will allow maxof[(((processing units) - 4) / 2), 4] | normal | lazy/normal/aggressive
 | cluster.background-self-heal-count | Specifies the number of per client self-heal jobs that can perform parallel heals in the background. | 8 | 0-256
 | cluster.heal-timeout | Time interval for checking the need to self-heal in self-heal-daemon | 600 | 5-(signed-int)
 | cluster.eager-lock | If eager-lock is off, locks release immediately after file operations complete, improving performance for some operations, but reducing access efficiency | on | on/off
 | cluster.quorum-type | If value is “fixed” only allow writes if quorum-count bricks are present. If value is “auto” only allow writes if more than half of bricks, or exactly half including the first brick, are present | none | none/auto/fixed
 | cluster.quorum-count | If quorum-type is “fixed” only allow writes if this many bricks are present. Other quorum types will OVERWRITE this value | null | 1-(signed-int)
 | cluster.heal-wait-queue-length | Specifies the number of heals that can be queued for the parallel background self heal jobs. | 128 | 0-10000
 | cluster.favorite-child-policy | Specifies which policy can be used to automatically resolve split-brains without user intervention. “size” picks the file with the biggest size as the source. “ctime” and “mtime” pick the file with the latest ctime and mtime respectively as the source. “majority” picks a file with identical mtime and size in more than half the number of bricks in the replica. | none | none/size/ctime/mtime/majority
 | cluster.use-anonymous-inode | Setting this option heals directory renames efficiently | no | no/yes
Logging | diagnostics.brick-log-level | Changes the log-level of the bricks | INFO | DEBUG/WARNING/ERROR/CRITICAL/NONE/TRACE
 | diagnostics.client-log-level | Changes the log-level of the clients. | INFO | DEBUG/WARNING/ERROR/CRITICAL/NONE/TRACE
 | diagnostics.latency-measurement | Statistics related to the latency of each operation would be tracked. | Off | On/Off
 | diagnostics.dump-fd-stats | Statistics related to file-operations would be tracked. | Off | On/Off
Performance | *features.trash | Enable/disable trash translator | off | on/off
 | *performance.readdir-ahead | Enable/disable readdir-ahead translator in the volume |	off | on/off
 | *performance.read-ahead | Enable/disable read-ahead translator in the volume | off | on/off
 | *performance.io-cache | Enable/disable io-cache translator in the volume | off | on/off
 | features.read-only | Enables you to mount the entire volume as read-only for all the clients (including NFS clients) accessing it. | Off | On/Off
 | performance.write-behind-window-size | Size of the per-file write-behind buffer. | 1MB | Write-behind cache size
 | performance.io-thread-count | The number of threads in IO threads translator. | 16 | 1-64
 | performance.flush-behind | If this option is set ON, instructs write-behind translator to perform flush in background, by returning success (or any errors, if any of previous writes were failed) to application even before flush is sent to backend filesystem. | On | On/Off
 | performance.cache-max-file-size | Sets the maximum file size cached by the io-cache translator. Can use the normal size descriptors of KB, MB, GB,TB or PB (for example, 6GB). Maximum size uint64. | 2 ^ 64 -1 bytes | size in bytes
 | performance.cache-min-file-size | Sets the minimum file size cached by the io-cache translator. Values same as "max" above | 0B | size in bytes
 | performance.cache-refresh-timeout | The cached data for a file will be retained till 'cache-refresh-timeout' seconds, after which data re-validation is performed. |	1s | 0-61
 | performance.cache-size | Size of the read cache. | 32 MB | size in bytes
 | geo-replication.indexing | Use this option to automatically sync the changes in the filesystem from Primary to Secondary. | Off | On/Off
 | network.frame-timeout | The time frame after which the operation has to be declared as dead, if the server does not respond for a particular operation. | 1800 (30 mins) | 1800 secs
 | network.ping-timeout | The time duration for which the client waits to check if the server is responsive. When a ping timeout happens, there is a network disconnect between the client and server. All resources held by server on behalf of the client get cleaned up. When a reconnection happens, all resources will need to be re-acquired before the client can resume its operations on the server. Additionally, the locks will be acquired and the lock tables updated. This reconnect is a very expensive operation and should be avoided. | 42 Secs | 42 Secs
nfs | nfs.enable-ino32 | For 32-bit nfs clients or applications that do not support 64-bit inode numbers or large files, use this option from the CLI to make Gluster NFS return 32-bit inode numbers instead of 64-bit inode numbers. | Off | On/Off
 | nfs.volume-access | Set the access type for the specified sub-volume. | read-write | read-write/read-only
 | nfs.trusted-write | If there is an UNSTABLE write from the client, STABLE flag will be returned to force the client to not send a COMMIT request. In some environments, combined with a replicated GlusterFS setup, this option can improve write performance. This flag allows users to trust Gluster replication logic to sync data to the disks and recover when required. COMMIT requests if received will be handled in a default manner by fsyncing. STABLE writes are still handled in a sync manner. | Off | On/Off
 | nfs.trusted-sync | All writes and COMMIT requests are treated as async. This implies that no write requests are guaranteed to be on server disks when the write reply is received at the NFS client. Trusted sync includes trusted-write behavior. | Off | On/Off
 | nfs.export-dir | This option can be used to export specified comma separated subdirectories in the volume. The path must be an absolute path. Along with path allowed list of IPs/hostname can be associated with each subdirectory. If provided connection will allowed only from these IPs. Format: \<dir\>[(hostspec[hostspec...])][,...]. Where hostspec can be an IP address, hostname or an IP range in CIDR notation. **Note**: Care must be taken while configuring this option as invalid entries and/or unreachable DNS servers can introduce unwanted delay in all the mount calls. | No sub directory exported. | Absolute path with allowed list of IP/hostname 
 | nfs.export-volumes | Enable/Disable exporting entire volumes, instead if used in conjunction with nfs3.export-dir, can allow setting up only subdirectories as exports. | On | On/Off
 | nfs.rpc-auth-unix | Enable/Disable the AUTH_UNIX authentication type. This option is enabled by default for better interoperability. However, you can disable it if required. | On | On/Off
 | nfs.rpc-auth-null | Enable/Disable the AUTH_NULL authentication type. It is not recommended to change the default value for this option. | On | On/Off
 | nfs.rpc-auth-allow\<IP- Addresses\> | Allow a comma separated list of addresses and/or hostnames to connect to the server. By default, all clients are disallowed. This allows you to define a general rule for all exported volumes. | Reject All | IP address or Host name
 | nfs.rpc-auth-reject\<IP- Addresses\> | Reject a comma separated list of addresses and/or hostnames from connecting to the server. By default, all connections are disallowed. This allows you to define a general rule for all exported volumes. | Reject All | IP address or Host name
 | nfs.ports-insecure | Allow client connections from unprivileged ports. By default only privileged ports are allowed. This is a global setting in case insecure ports are to be enabled for all exports using a single option. | Off | On/Off
 | nfs.addr-namelookup | Turn-off name lookup for incoming client connections using this option. In some setups, the name server can take too long to reply to DNS queries resulting in timeouts of mount requests. Use this option to turn off name lookups during address authentication. Note, turning this off will prevent you from using hostnames in rpc-auth.addr.* filters. | On | On/Off
 | nfs.register-with-portmap |For systems that need to run multiple NFS servers, you need to prevent more than one from registering with portmap service. Use this option to turn off portmap registration for Gluster NFS. | On | On/Off
 | nfs.port \<PORT- NUMBER\> | Use this option on systems that need Gluster NFS to be associated with a non-default port number. | NA | 38465-38467
 | nfs.disable | Turn-off volume being exported by NFS | Off | On/Off
Server | server.allow-insecure | Allow client connections from unprivileged ports. By default only privileged ports are allowed. This is a global setting in case insecure ports are to be enabled for all exports using a single option.| On |	On/Off
 | server.statedump-path | Location of the state dump file. | tmp directory of the brick | New directory path
Storage | storage.health-check-interval | Number of seconds between health-checks done on the filesystem that is used for the brick(s). Defaults to 30 seconds, set to 0 to disable. | tmp directory of the brick | New directory path

> **Note** 
>
> We've found few performance xlators, options marked with * in above table have been causing more performance regression than improving. These xlators should be turned off for volumes.