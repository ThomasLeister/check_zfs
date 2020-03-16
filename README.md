# check_zfs

**Icinga / Nagios compatible script for checking any ZFS storage pool for free space.**

*(Original by [zlacelle](https://github.com/zlacelle/nagios_check_zfs_linux)) - Removed fragmentation value support, changed CLI interface and added Python3 support.*


## Example configuration for Icinga2

`/etc/icinga2/conf.d/commands.conf`

```
object CheckCommand "zfs" {
    import "plugin-check-command"
    command = [ "/usr/local/nagios-plugins/check_zfs" ]
    arguments = {
        "--pool" = {
            value = "$zfs_pool$"
            description = "ZFS pool to check"
            required = true
        }
        "--warn" = {
            value = "$zfs_warning$"
            description = "Free space in percent for warning alert"
            required = true
        }
        "--crit" = {
            value = "$zfs_critical$"
            description = "Free space in percent for critical alert"
            required = true
        }
    }
}
```

`/etc/icinga2/conf.d/services.conf`

```
apply Service "zfs_" for (pool in host.vars.zfs_pools) {
    import "generic-service"
    check_command = "zfs"
    vars.zfs_pool = pool
    vars.zfs_warning = "80"
    vars.zfs_critical = "90"
}
```

`/etc/icinga2/conf.d/hosts.conf`

```
object Host "myhost.tld" {  
    import "generic-host"
    [...]
    vars.zfs_pools = [ "mysuperstoragepool" ]
    [...]
}
```



## Run command manually for testing

e.g.:

```
./check_zfs --pool mysuperstoragepool --warn 80 --crit 90
```

