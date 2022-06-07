# Feedpushr

Role to install [feedpushr](https://github.com/ncarlier/feedpushr).

A simple feed aggregator service with sugar on top.

## Notice

I did not test all vars. You can encouter some bug, feel free to fix it on my GitHub or by creating an issue.

## Roadmap

* Build from source instead of using binaries: plugins are not in the ARM archive

## Requirements

Nothing, the author of feedpusher is providing all binaries for common Linux arch.

## Role Variables

| Variable | Default value | Description |
|----------|---------------|-------------|
| `feedpushr_install_dir` | `/opt/feedpushr` | Where to install the binaries
| `feedpushr_user` | `www-data` | User that run the Feedpushr service
| `feedpushr_group` | `www-data` | Group that run the Feedpushr service
| `feedpushr_admin_user` | `admin` | Admin user, it'll run `htpasswd -B -c .htpasswd admin`
| `feedpushr_admin_user` | `Change-Me!` | Default password so it is obvious to change it
| `feedpushr_db` | `boltdb://data.db` | Database location, default is "boltdb://data.db"
| `feedpushr_listen` | `:8080` | HTTP listen address, default is ":8080" (examples: `localhost:8080` or `:8080` for all interfaces)
| `feedpushr_log_level` | `info` | Logging level, default is "info". Available levels are: `debug`, `info`, `warn` and `error`.
| `feedpushr_log_file` | Log output, STDOUT if not set (example: `/var/log/feedpushr.log`)
| `feedpushr_log_pretty` | `false` |  Writes log using plain text format, default is "false"
| `feedpushr_authn` | `.htpasswd` | Authentication method, default is ".htpasswd" (Password file for basic HTTP authentication). Issuer base URL for OpenID Connect authentication. `/etc/feedpushr.htpasswd`
| `feedpushr_authorized_username` | `*` | Authorized username, default is "*" for all (examples `foo`)
| `feedpushr_retention_duration` | `72h` | Cache retention duration, default is "72h"
| `feedpushr_clear_cache` | `false` | Clear cache at service startup, default is "false". **Should not be activated for production**.
| `feedpushr_clear_config` | `false` | Clear the configuration at service startup, default is "false". **Should not be activated for production**.
| `feedpushr_delay` | `1m` | Delay between aggregations, default is "1m" (examples: `30s`, `15m`, `1h`)
| `feedpushr_fan_out_delay` | `0s` | Delay between deployment of each aggregator, default is "0s" (examples: `300ms`, `5s`)
| `feedpushr_explore_provider` | `default` | Provider used to search new RSS feeds, default is "rsssearchhub"
| `feedpushr_import` | `FP_IMPORT` | Import an OPML file at bootstrap, disabled by default (example: "./my_subscriptions.opml")
| `feedpushr_max_nb_feeds` | `0` | Maximum number of feeds, default is 0 for unlimited
| `feedpushr_max_nb_output` | `0` | Maximum number of outputs, default is 0 for unlimited
| `feedpushr_public_url` |  | Public URL used for PSHB subscriptions, disabled by default (example: `https://example.org/feedpushr`)
| `feedpushr_sentry_dsn` | Sentry DSN URL, disabled by default (example: `https://<id>:<key>@sentry.io/<project>`)
| `feedpushr_timeout` | `5s` | Aggregation timeout, default is 5s
| `feedpushr_plugins` | Comma separated list of plugins to load, default is empty (example: `./feedpushr-twiter.so,./feedpushr-rake.so`).

### Available plugins

* Output plugins
  * [Kafka](https://github.com/ncarlier/feedpushr/tree/master/contrib/kafka): `feedpushr-kafka.so`
  * [Mastodon](https://mastodon.social): `feedpushr-mastodon.so`
  * RDBMS: `feedpushr-rdbms.so`
  * Twitter: `feedpushr-twitter.so`
  * Twitter Selenium: `feedpushr-twitter-selenium.so`
  * [Wallabag](https://www.wallabag.it): `feedpushr-wallabag.so`
* Filter plugins
  * Prose: `feedpushr-prose.so`
  * RAKE: `feedpushr-rake.so`

Output and Filter's configuration are stored inside the Feedpushr DB (Bolt). The configuration is managed by the API or the UI.

## Dependencies

None but with some plugins you may need to install some stuffs.

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: all
  gather_facts: true
  remote_user: root
  vars:
    # Feedpushr
    feedpushr_user: me
    feedpushr_group: me
    feedpushr_admin_psw: "Change-me!"
    feedpushr_db: "boltdb:///home/me/feedpushr.bolt_db"
    feedpushr_listen: "localhost:8330"
    feedpushr_retention_duration: 72h
    feedpushr_delay: 10m
    feedpushr_fan_out_delay: 256ms
    #feedpushr_plugins: "feedpushr-rake.so,feedpushr-prose.so,feedpushr-rdbms.so"
    feedpushr_public_url: https://192.168.1.2/feedpushr
  roles:
    - slundi.feedpushr
```

## License

MIT

## Author Information

I made this Ansible role because I want to try feedpushr in order to replace my personnal use of Feedly: I like Feedly UI but I can't filter without paying or save articles.
