# Feedpushr

Role to install [feedpushr](https://github.com/ncarlier/feedpushr).

A simple feed aggregator service with sugar on top.

## Requirements

Nothing, the author of feedpusher is providing all binaries for common Linux arch.

## Role Variables

| Variable | Feedpushr variable | Default value | Description |
|----------|:-------------:|--------------------|-------------|
| `feedpushr_install_dir` | | `/opt/feedpushr` | Where to install the binaries
| `feedpushr_db` | `FP_DB` | `boltdb://data.db` | Database location, default is "boltdb://data.db"
| `feedpushr_listen` | `FP_LISTEN_ADDR` | `:8080` | HTTP listen address, default is ":8080" (examples: `localhost:8080` or `:8080` for all interfaces)
| `feedpushr_log_level` | `FP_LOG_LEVEL` | `info` | Logging level, default is "info". Available levels are: `debug`, `info`, `warn` and `error`.
| `feedpushr_log_file` | `FP_LOG_OUTPUT` | | Log output, STDOUT if not set (example: `/var/log/feedpushr.log`)
| `feedpushr_log_pretty` | `FP_LOG_PRETTY` | `false` |  Writes log using plain text format, default is "false"
| `feedpushr_authn` | `FP_AUTHN` | `.htpasswd` | Authentication method, default is ".htpasswd" (Password file for basic HTTP authentication). Issuer base URL for OpenID Connect authentication. `/etc/feedpushr.htpasswd`
| `feedpushr_authorized_username` | `FP_AUTHORIZED_USERNAME` | `*` | Authorized username, default is "*" for all (examples `foo`)
| `feedpushr_retention_duration` | `FP_CACHE_RETENTION` | `72h` | Cache retention duration, default is "72h"
| `feedpushr_clear_cache` | `FP_CLEAR_CACHE` | `false` | Clear cache at service startup, default is "false". **Should not be activated for production**.
| `feedpushr_clear_config` | `FP_CLEAR_CONFIG` | `false` | Clear the configuration at service startup, default is "false". **Should not be activated for production**.
| `feedpushr_delay` | `FP_DELAY` | `1m` | Delay between aggregations, default is "1m" (examples: `30s`, `15m`, `1h`)
| `feedpushr_fan_out_delay` | `FP_FAN_OUT_DELAY` | `0s` | Delay between deployment of each aggregator, default is "0s" (examples: `300ms`, `5s`)
| `feedpushr_explore_provider` | `FP_EXPLORE_PROVIDER` | `default` | Provider used to search new RSS feeds, default is "rsssearchhub"
| `feedpushr_import` | `FP_IMPORT` | | Import an OPML file at bootstrap, disabled by default (example: "./my_subscriptions.opml")
| `feedpushr_max_nb_feeds` | `FP_MAX_NB_FEEDS` | `0` | Maximum number of feeds, default is 0 for unlimited
| `feedpushr_max_nb_output` | `FP_MAX_NB_OUTPUTS` | `0` | Maximum number of outputs, default is 0 for unlimited
| `feedpushr_public_url` | `FP_PUBLIC_URL=` |  | Public URL used for PSHB subscriptions, disabled by default (example: `https://example.org/feedpushr`)
| `feedpushr_sentry_dsn` | `FP_SENTRY_DSN=` | | Sentry DSN URL, disabled by default (example: `https://<id>:<key>@sentry.io/<project>`)
| `feedpushr_timeout` | `FP_TIMEOUT` | `5s` | Aggregation timeout, default is 5s
| `feedpushr_plugins` | `FP_PLUGINS` | | Comma separated list of plugins to load, default is empty (example: `./plugins/feedpushr-twiter.so,./plugins/feedpushr-rake.so`).

### Available plugins

* Output plugins
  * [Kafka](https://github.com/ncarlier/feedpushr/tree/master/contrib/kafka): feedpushr-kafka.so
  * [Mastodon](https://mastodon.social): feedpushr-mastodon.so
  * RDBMS: feedpushr-rdbms.so
  * Twitter: feedpushr-twitter.so
  * Twitter Selenium: feedpushr-twitter-selenium.so
  * [Wallabag](https://www.wallabag.it): feedpushr-wallabag.so
* Filter plugins
  * Prose: feedpushr-prose.so
  * RAKE: feedpushr-rake.so

### Output plugin configuration

#### Kafka

Send new articles to Kafka.

| Property | Description |
|----------|-------------|
| `feedpushr_kafka_brokers` | Comma separated list of broker host |
| `feedpushr_kafka_topic` | Target topic |
| `feedpushr_kafka_format` | Payload [format](https://github.com/ncarlier/feedpushr#output-format) (internal JSON format if not provided) |

#### Mastodon

Send new articles as a "Toot" to a [Mastodon](https://joinmastodon.org/) instance.

| Property | Description |
|----------|-------------|
| `feedpushr_mastodon_url` | Mastodon instance URL (by default: <https://mastodon.social>) |
| `feedpushr_mastodon_token` | Access token |
| `feedpushr_mastodon_visibility` | Toot visibility ("direct", "private", "unlisted" or by default "public") |
| `feedpushr_mastodon_format` | Toot [format](https://github.com/ncarlier/feedpushr#output-format) (by default: `{{.Title}}\n{{.Link}}`) |

#### RDBMS

Send new articles to a RDBMS (Mysql, PostgreSQL or sqlite3).

| Property | Description |
|----------|-------------|
| `feedpushr_rdbms_driver` | database driver (available: sqlite3, mysql, postgres) |
| `feedpushr_rdbms_database` | database name |
| `feedpushr_rdbms_host` | database host |
| `feedpushr_rdbms_port` | database port |
| `feedpushr_rdbms_username` | databse username |
| `feedpushr_rdbms_password` | database password |
| `feedpushr_rdbms_verbose` | verbose query activity |

#### Twitter

Send new articles to a Twitter timeline.

| Property | Description |
|----------|-------------|
| `feedpushr_twitter_consumerKey` | Consumer key |
| `feedpushr_twitter_consumerSecret` | Consumer secret |
| `feedpushr_twitter_accessToken` | Access token |
| `feedpushr_twitter_accessTokenSecret` | Access token secret |
| `feedpushr_twitter_format` | Tweet [format](https://github.com/ncarlier/feedpushr#output-format) (by default: `{{.Title}}\n{{.Link}}`) |

#### Twitter-Selenium

Send new articles to a Twitter timeline with Selenium.

| Property | Description |
|----------|-------------|
| `feedpushr_twitter_selenium_browser` | browser driver (available: chrome, firefox) |
| `feedpushr_twitter_selenium_seleniumAddr` | selenium address (default: 127.0.0.1:4444) |
| `feedpushr_twitter_selenium_username` | username/account |
| `feedpushr_twitter_selenium_password` | password |
| `feedpushr_twitter_selenium_format` | Tweet [format](https://github.com/ncarlier/feedpushr#output-format) (by default: `{{.Title}}\n{{.Link}}`) |

#### Wallabag

Send new articles to a Wallabag instance.

| Property | Description |
|----------|-------------|
| `feedpushr_wallabag_url` | Base URL of the Wallabag instance (<https://app.wallabag.it> unless self-hosting)
| `feedpushr_wallabag_username` | Username
| `feedpushr_wallabag_password` | Password
| `feedpushr_wallabag_clientId` | Client ID for the Wallabag API client
| `feedpushr_wallabag_clientSecret` | Client secret
| `feedpushr_wallabag_includeContent` | Whether to send the include the article content in the request to the API. If not, wallabag will use its own fetcher to get the content from the article URL.

### Filter plugin configuration

#### Prose

Extract Named Entity from text and convert them to hashtags.

| Property | Default | Description |
|----------|---------|-------------|
| `feedpushr_prose_filter` | `all` | Filter entity by label (available: all,person,gpe) |
| `feedpushr_prose_format`      | `hashtag`  | Format each named entity in a specific type (available: hashtag,keyword,none |
| `feedpushr_prose_separator`   | `space`    | Separate each named entity with a defined chararcter (available: space,tab,comma,semi-colon,pipe) |
| `feedpushr_prose_minCharLength`      | `1`  | Each entity has at least N characters |
| `feedpushr_prose_maxCharLength`      | `15` | Each entity has a maxium of N characters |

#### RAKE

Extract keywords of an article by using Rapid Automatic Keyword Extraction algorithm.

| Property | Default | Description |
|----------|---------|-------------|
| `feedpushr_rake_minCharLength`       | `4` | Each word has at least N characters |
| `feedpushr_rake_maxWordsLength`      | `3` | Each phrase has at most N words |
| `feedpushr_rake_minKeywordFrequency` | `4` | Each keyword appears in the text at least N times |

## Dependencies

None but with some plugins you may need to install some stuffs.

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

## License

MIT

## Author Information

I made this Ansible role because I want to try feedpushr in order to replace my personnal use of Feedly: I like Feedly UI but I can't filter without paying or save articles.
