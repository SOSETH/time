# Role: time

Configuration of timezone and ntp settings

**Compatibility tested with:**
 * Debian 8
 * Debian 9
 * Fedora 25 Server

## Configuration
| Name                             | Default value                           | Description                                                                                                                               |
| -------------------------------- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `timezone_area`                  | `Europe`                                | Timezone area                                                                                                                             |
| `timezone`                       | `Zurich`                                | Exact timezone (area needs to match)                                                                                                      |
| `ntp_servers`                    | `["time.ethz.ch", "swisstime.ethz.ch"]` | Timeservers. Note that these are only guaranteed to be accessible from inside ETH's network, so you might need to specify different ones. |
| `chrony_conf_dropin_enabled`     | `false`                                 | Enable managed config drop-in mode (keep vendor main config).                                                                             |
| `chrony_conf_dropin_dir`         | `/etc/chrony/conf.d`                    | Directory for managed Chrony config drop-in file.                                                                                         |
| `chrony_conf_dropin_file`        | `custom.conf`                           | Filename for managed Chrony config drop-in file.                                                                                          |
| `chrony_sources_dropins_enabled` | `false`                                 | Enable Chrony `sources.d` drop-ins management.                                                                                            |
| `chrony_sources_dropins_dir`     | `/etc/chrony/sources.d`                 | Directory for Chrony source drop-in files.                                                                                                |
| `chrony_sources_dropins_files`   | `[]`                                    | List of source files with `name` and `servers`.                                                                                           |

### Proxmox 9 / Debian Trixie drop-in example

Keep vendor `chrony.conf`, manage our config via conf.d drop-in, and optional sources.d files:

```yaml
time_client_override: chrony

chrony_conf_dropin_enabled: true
chrony_conf_dropin_dir: /etc/chrony/conf.d
chrony_conf_dropin_file: vis.conf

chrony_sources_dropins_enabled: true
chrony_sources_dropins_dir: /etc/chrony/sources.d
chrony_sources_dropins_files:
    - name: ethz.sources
      servers: [time.ethz.ch, time6.ethz.ch]
    - name: swiss.sources
      servers: [0.ch.pool.ntp.org, 1.ch.pool.ntp.org, 2.ch.pool.ntp.org, 3.ch.pool.ntp.org]
```

When `chrony_conf_dropin_enabled` or `chrony_sources_dropins_enabled` is true,
the role performs independent preflight checks that `{{ time_chrony_conf_path }}`
contains the matching `confdir`/`sourcedir` directives for the configured directories.

## License
GPLv3
