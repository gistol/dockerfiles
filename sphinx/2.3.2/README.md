## Sphinx 2.3.2

### Usage

Create data dir and your configuration file with SQL source and index:
```
$ mkdir -p /data/sphinx/data
$ vim /data/sphinx/localities.conf

source db
{
  type = mysql
  sql_host = ${SQL_HOST}
  sql_port = ${SQL_PORT}
  sql_user = ${SQL_USER}
  sql_pass = ${SQL_PASS}
  sql_db = ${SQL_DB}
}

source localities : db
{
  ...
}
```

Run container:

```
$ docker run -d \
    --name "sphinx" \
    --restart="on-failure:5" \
    -v "/data/sphinx/localities.conf:/etc/sphinx/conf.d/localities.conf" \
    -v "/data/sphinx/data:/var/lib/sphinx" \
    -p "9312:9312"
    -p "9306:9306" \
    -p "9307:9307" \
    -e "SQL_HOST=127.0.0.1" \
    -e "SQL_PORT=3306" \
    -e "SQL_USER=test-user" \
    -e "SQL_PASS=test-pass" \
    -e "SQL_DB=test-db" \
    bborysenko/sphinx:2.3.2
  docker logs -f rz-localities
```

### HTTP protocol

Starting with 2.3.2-beta, Sphinx supports [HTTP protocol](http://sphinxsearch.com/docs/devel.html#http-rest) and can be accessed with regular HTTP clients.

### Exposed ports

- 9312
- 9306
- 9307

### Environment variables

#### Configuration options for `indexer`

| Environment variable | Option | Default | Description |
| --- | --- | --- | --- |
| `SPHINX_INDEXER_MEM_LIMIT` | [`mem_limit`](http://sphinxsearch.com/docs/devel.html#conf-mem-limit) | `128M` | Indexing RAM usage limit |

#### Configuration options for `searchd`

| Environment variable | Option | Default | Description |
| --- | --- | --- | --- |
| `SPHINX_SEARCHD_READ_TIMEOUT` | [`read_timeout`](http://sphinxsearch.com/docs/devel.html#conf-read-timeout) | `5` | Network client request read timeout, in seconds. |
| `SPHINX_SEARCHD_CLIENT_TIMEOUT` | [`client_timeout`](http://sphinxsearch.com/docs/devel.html#conf-client-timeout) | `300` | Maximum time to wait between requests when using persistent connections, in seconds. |
| `SPHINX_SEARCHD_MAX_CHILDREN` | [`max_children`](http://sphinxsearch.com/docs/devel.html#conf-max-children) | `30` | Maximum amount of worker threads. |
| `SPHINX_SEARCHD_SEAMLESS_ROTATE` | [`seamless_rotate`](http://sphinxsearch.com/docs/devel.html#conf-seamless-rotate) | `1` | Prevents searchd stalls while rotating indexes with huge amounts of data to precache. |
| `SPHINX_SEARCHD_PREOPEN_INDEXES` | [`preopen_indexes`](http://sphinxsearch.com/docs/devel.html#conf-preopen-indexes) | `1` | Whether to forcibly preopen all indexes on startup. |
| `SPHINX_SEARCHD_UNLINK_OLD` | [`unlink_old`](http://sphinxsearch.com/docs/devel.html#conf-unlink-old) | `1` | Whether to unlink `.old` index copies on successful rotation. |

#### Configuration options for SQL data source

| Environment variable | Option | Default | Description |
| --- | --- | --- | --- |
| `SQL_HOST` | [`sql_host`](http://sphinxsearch.com/docs/devel.html#conf-sql-host) | --- | SQL server host. |
| `SQL_PORT` | [`sql_port`](http://sphinxsearch.com/docs/devel.html#conf-sql-port) | --- | SQL server IP port. |
| `SQL_USER` | [`sql_user`](http://sphinxsearch.com/docs/devel.html#conf-sql-user) | --- | SQL user to use. |
| `SQL_PASS` | [`sql_pass`](http://sphinxsearch.com/docs/devel.html#conf-sql-pass) | --- | SQL user password to use |
| `SQL_DB` | [`sql_db`](http://sphinxsearch.com/docs/devel.html#conf-sql-db) | --- | SQL database  |

The values of this environment variables are substituted by [`envsubst`](https://www.gnu.org/software/gettext/manual/html_node/envsubst-Invocation.html) in your configuration file located in `/etc/sphinx/conf.d/`.
