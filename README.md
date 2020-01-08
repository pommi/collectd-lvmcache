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

## Understanding the values

For the terminology refer to the [kernel's cache documentation](https://www.kernel.org/doc/html/latest/admin-guide/device-mapper/cache.html).

The values for `metadata_free`, `metadata_used`, `cache_free`, `cache_used` and `dirty` are multiplied with the blocksize, so the resulting unit is Bytes.

The `cache_result` values are counters, so expect to use a derivative on them (e.g. if you use Grafana).
