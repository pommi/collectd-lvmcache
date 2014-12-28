# collectd-lvmcache

This script collects [lvmcache / dm-cache](https://www.kernel.org/doc/Documentation/device-mapper/cache.txt) SSD caching statistics and outputs it to STDOUT in a [Collectd Exec plugin](https://collectd.org/wiki/index.php/Plugin:Exec) compatible format.

## Usage

### Collectd

```
<Plugin exec>
    Exec "user:group" "/path/to/collectd-lvmcache"
</Plugin>
```

### Command-line (for testing)

```
$ ./collectd-lvmcache
PUTVAL "hostname.tld/lvmcache-lv0/bytes-dirty" interval=1 N:0
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-metadata_used" interval=1 N:3846144
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-metadata_free" interval=1 N:205869056
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-cache_used" interval=1 N:1252261888
PUTVAL "hostname.tld/lvmcache-lv0/df_complex-cache_free" interval=1 N:17001349120
PUTVAL "hostname.tld/lvmcache-lv0/requests-read_hits" interval=1 N:42325
PUTVAL "hostname.tld/lvmcache-lv0/requests-read_misses" interval=1 N:145193
PUTVAL "hostname.tld/lvmcache-lv0/requests-write_hits" interval=1 N:212759
PUTVAL "hostname.tld/lvmcache-lv0/requests-write_misses" interval=1 N:1221586
PUTVAL "hostname.tld/lvmcache-lv0/requests-demotions" interval=1 N:0
PUTVAL "hostname.tld/lvmcache-lv0/requests-promotions" interval=1 N:15897
```
