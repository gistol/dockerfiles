# Sphinx 2.3.2




```
docker run -d \
  -p 9306:9306 \
  -p 9307:9307 \
  -v /data/sphinx/conf/localities.conf:/etc/sphinx/conf.d/localities.conf \
  -v /data/sphinx/data:/var/lib/sphinx \
  bborysenko/sphinx:2.3.2
```

## HTTP protocol

Starting with 2.3.2-beta, Sphinx supports [HTTP protocol](http://sphinxsearch.com/docs/devel.html#http-rest) and can be accessed with regular HTTP clients.

## Exposed ports

- 9312
- 9306
- 9307

## Environment variables

- `SPHINX_INDEXER_MEM_LIMIT`
- `SPHINX_SEARCHD_READ_TIMEOUT`
- `SPHINX_SEARCHD_CLIENT_TIMEOUT`
- `SPHINX_SEARCHD_MAX_CHILDREN`
- `SPHINX_SEARCHD_SEAMLESS_ROTATE`
- `SPHINX_SEARCHD_PREOPEN_INDEXES`
- `SPHINX_SEARCHD_UNLINK_OLD`
