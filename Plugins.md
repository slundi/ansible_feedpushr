# Feedpushr plugins

## Available plugins

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

## Output plugin configuration

### Kafka

Send new articles to Kafka.

| Property | Description |
|----------|-------------|
| `feedpushr_kafka_brokers` | Comma separated list of broker host |
| `feedpushr_kafka_topic` | Target topic |
| `feedpushr_kafka_format` | Payload [format](https://github.com/ncarlier/feedpushr#output-format) (internal JSON format if not provided) |

### Mastodon

Send new articles as a "Toot" to a [Mastodon](https://joinmastodon.org/) instance.

| Property | Description |
|----------|-------------|
| `feedpushr_mastodon_url` | Mastodon instance URL (by default: <https://mastodon.social>) |
| `feedpushr_mastodon_token` | Access token |
| `feedpushr_mastodon_visibility` | Toot visibility ("direct", "private", "unlisted" or by default "public") |
| `feedpushr_mastodon_format` | Toot [format](https://github.com/ncarlier/feedpushr#output-format) (by default: `{{.Title}}\n{{.Link}}`) |

### RDBMS

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

### Twitter

Send new articles to a Twitter timeline.

| Property | Description |
|----------|-------------|
| `feedpushr_twitter_consumerKey` | Consumer key |
| `feedpushr_twitter_consumerSecret` | Consumer secret |
| `feedpushr_twitter_accessToken` | Access token |
| `feedpushr_twitter_accessTokenSecret` | Access token secret |
| `feedpushr_twitter_format` | Tweet [format](https://github.com/ncarlier/feedpushr#output-format) (by default: `{{.Title}}\n{{.Link}}`) |

### Twitter-Selenium

Send new articles to a Twitter timeline with Selenium.

| Property | Description |
|----------|-------------|
| `feedpushr_twitter_selenium_browser` | browser driver (available: chrome, firefox) |
| `feedpushr_twitter_selenium_seleniumAddr` | selenium address (default: 127.0.0.1:4444) |
| `feedpushr_twitter_selenium_username` | username/account |
| `feedpushr_twitter_selenium_password` | password |
| `feedpushr_twitter_selenium_format` | Tweet [format](https://github.com/ncarlier/feedpushr#output-format) (by default: `{{.Title}}\n{{.Link}}`) |

### Wallabag

Send new articles to a Wallabag instance.

| Property | Description |
|----------|-------------|
| `feedpushr_wallabag_url` | Base URL of the Wallabag instance (<https://app.wallabag.it> unless self-hosting)
| `feedpushr_wallabag_username` | Username
| `feedpushr_wallabag_password` | Password
| `feedpushr_wallabag_clientId` | Client ID for the Wallabag API client
| `feedpushr_wallabag_clientSecret` | Client secret
| `feedpushr_wallabag_includeContent` | Whether to send the include the article content in the request to the API. If not, wallabag will use its own fetcher to get the content from the article URL.

## Filter plugin configuration

### Prose

Extract Named Entity from text and convert them to hashtags.

| Property | Default | Description |
|----------|---------|-------------|
| `feedpushr_prose_filter` | `all` | Filter entity by label (available: all,person,gpe) |
| `feedpushr_prose_format`      | `hashtag`  | Format each named entity in a specific type (available: hashtag,keyword,none |
| `feedpushr_prose_separator`   | `space`    | Separate each named entity with a defined chararcter (available: space,tab,comma,semi-colon,pipe) |
| `feedpushr_prose_minCharLength`      | `1`  | Each entity has at least N characters |
| `feedpushr_prose_maxCharLength`      | `15` | Each entity has a maxium of N characters |

### RAKE

Extract keywords of an article by using Rapid Automatic Keyword Extraction algorithm.

| Property | Default | Description |
|----------|---------|-------------|
| `feedpushr_rake_minCharLength`       | `4` | Each word has at least N characters |
| `feedpushr_rake_maxWordsLength`      | `3` | Each phrase has at most N words |
| `feedpushr_rake_minKeywordFrequency` | `4` | Each keyword appears in the text at least N times |
