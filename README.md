fluent-plugin-riak, a plugin for [Fluentd](http://fluentd.org)
==================

fluent-plugin-riak is a alternative for people who are not sufficient with mongo or webhdfs. Riak ( http://github.com/basho/riak ) is an open-source distributed KVS focused on availability. It also has a strong query system with secondary index (2i): see docs ( http://docs.basho.com/riak/latest/tutorials/querying/ ) for details.

Current status is still proof-of-concept: index setting and its configuration are to be decided. Also performance optimization is required. Another idea is in_tail_riak by using riak post-commit.

installation
------------

```bash
$ sudo gem install fluent-plugin-riak
```

Notice: you need Riak configured using eleveldb as backend.


fluent.conf example
-------------------

```
<match riak.**>
  type riak

  buffer_type memory
  flush_interval 10s
  retry_limit 5
  retry_wait 1s
  buffer_chunk_limit 256m
  buffer_queue_limit 8096

  # pb port
  nodes 127.0.0.1:8087
  #for cluster, define multiple machines
  #nodes 192.168.100.128:10018 129.168.100.128:10028 
</match>

```

- key format -> 2013-02-<uuid>
- value format -> [records] in JSON
- index:

 - year_int -> year
 - month_bin -> <year>-<month>
 - tag_bin -> tags

easy querying log
-----------------

```bash
$ curl -X PUT http://localhost:8098/buckets/static/keys/browser.html -H 'Content-type: text/html' -d @browser.html
$ open http://localhost:8098/buckets/static/keys/browser.html
```

Pros
----

- easy operations
- high availability
- horizontal scalability (esp. write performance)
- good night sleep

Cons
----

- no capped table, TTL objects

TODOs
-----

- refine browser.html query interface with cool features
- rething index structures


License
=======

Apache 2.0

Copyright Kota UENISHI

Many Thanks to fluent-plugin-mongo
