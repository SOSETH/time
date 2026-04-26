# Role: time

Configuration of timezone and ntp settings

**Compatibility tested with:**
 * Debian 8
 * Debian 9
 * Fedora 25 Server

## Configuration
| Name                | Default value                           | Description                                                                                                                               |
| ------------------- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `timezone_area`     | `Europe`                                | Timezone area                                                                                                                             |
| `timezone`          | `Zurich`                                | Exact timezone (area needs to match)                                                                                                      |
| `ntp_servers`       | `["time.ethz.ch", "swisstime.ethz.ch"]` | Timeservers. Note that these are only guaranteed to be accessible from inside ETH's network, so you might need to specify different ones. |
| `ntp_sources_dir`   | `/etc/chrony/sources.d`                 | Directory for optional Chrony source drop-ins.                                                                                            |
| `ntp_sources_files` | `[]`                                    | Optional list of drop-in files and server lists to render as `server ... iburst` lines.                                                   |

### Proxmox 9 / Debian Trixie drop-in example

Keep existing role behavior (`ntp_servers` still drives `chrony.conf`) and add source drop-ins:

```yaml
time_client_override: chrony
ntp_sources_files:
  - name: ethz.sources
    servers: [time.ethz.ch, time6.ethz.ch]
  - name: swiss.sources
    servers: [0.ch.pool.ntp.org, 1.ch.pool.ntp.org, 2.ch.pool.ntp.org, 3.ch.pool.ntp.org]
```

## License
GPLv3
