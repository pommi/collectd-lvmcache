# collectd-lvmcache

This script collects [lvmcache / dm-cache](https://www.kernel.org/doc/Documentation/device-mapper/cache.txt) SSD caching statistics and outputs it to STDOUT in a [Collectd Exec plugin](https://collectd.org/wiki/index.php/Plugin:Exec) compatible format.

## Usage

### Collectd

```
<Plugin exec>
    Exec "user:group" "/path/to/collectd-lvmcache"
</Plugin>
```
Note: you are recommended to run under the least privileged user like `nobody` and in that case you need to allow `user` to elevate privileges via `sudo` for `specific command`, see below.

### Command-line (for testing)
Under `root` (or other user having access to `dmesetup` command), simply run:
```
# ./collectd-lvmcache
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-dirty" interval=1 N:0
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-metadata_used" interval=1 N:9199616
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-metadata_free" interval=1 N:1064542208
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-cache_used" interval=1 N:663814144
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-cache_free" interval=1 N:36917149696
PUTVAL "hostname.tld/lvmcache-lv0/cache_result-read_hits" interval=1 N:81565
PUTVAL "hostname.tld/lvmcache-lv0/cache_result-read_misses" interval=1 N:166286
PUTVAL "hostname.tld/lvmcache-lv0/cache_result-write_hits" interval=1 N:3422930
PUTVAL "hostname.tld/lvmcache-lv0/cache_result-write_misses" interval=1 N:294842
PUTVAL "hostname.tld/lvmcache-lv0/cache_result-demotions" interval=1 N:0
PUTVAL "hostname.tld/lvmcache-lv0/cache_result-promotions" interval=1 N:1806
```

### Sudo setup
Example for Ubuntu 20.04, but all modern distros should be the same or very similar:
* assuming you have chosen `nobody` as user in collectd's config, then configure `/etc/sudoers.d/nobody_dmsetup_status`, with content 
```
# allow nobody user to query LVM CACHE stats without password
nobody ALL=(root:root) NOPASSWD: /sbin/dmsetup status --target cache
```
* checking by listing allowed commands and options for `nobody`:
```
root@server# sudo -U nobody -l
Matching Defaults entries for nobody on server:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User nobody may run the following commands on server:
    (root : root) NOPASSWD: /sbin/dmsetup status --target cache
```
* checking under `nobody` user itself (I've installed script into `/opt/scripts/monitoring/collectd/collectd-lvmcache`):
```
root@server:~# sudo -u nobody /bin/bash
nobody@server:/home/admin$ cd /tmp
nobody@server:/tmp$ /opt/scripts/monitoring/collectd/collectd-lvmcache
PUTVAL "hostname.tld/lvmcache-vg0-data/df_complex-dirty" interval=1 N:109051904
PUTVAL "hostname.tld/lvmcache-vg0-data/df_complex-metadata_used" interval=1 N:10108928
PUTVAL "hostname.tld/lvmcache-vg0-data/df_complex-metadata_free" interval=1 N:31834112
...
...
```
