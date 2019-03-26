<!-- CH Repo Tracking Data version 1.0.2 -->
|     Category     |         Value         |
| ---------------: | --------------------- |
| **Repo Status**  | deprecated |
| **Language**     | Ruby          |
| **Repo Owner**   |   |
<!-- If you use any of the following fields, please delete "Repo Owner", above,
     as it then becomes ambiguous.  If you add fields, please update the script
     at walker-github-ranger. -->
<!--
| **Repo PoC**     | <One or more humans>  |
| **Repo Oncall**  | <Link to PagerDuty>   |
| **Repo Team Owner** | <Name of owning team> |
-->


[fluent-plugin-riak2](https://github.com/collectivehealth/fluent-plugin-riak2g), a plugin for [Fluentd](http://fluentd.org)
==================


`fluent-plugin-riak2` is a fluentd output plugin designed to stuff log messages into a riak cluster.

This version is based on the work of [Kota UENISHI's fluent-plugin-riak](https://github.com/kuenishi/fluent-plugin-riak).  We are very thankful for his effort and his decision to release the work under the Apache 2 license.

`fluent-plugin-riak2` is designed to be used with Riak 2.x clusters and it's yokozuna/solr based search engine.  Support for secondary indicies is limited and should be considered deprecated.

Riak ( http://github.com/basho/riak ) is an open-source distributed KVS focused on availability.

Current status is still proof-of-concept: index setting and its configuration are to be decided. Also performance optimization is required. Another idea is in_tail_riak by using riak post-commit.

installation
------------

```bash
$ sudo gem install fluent-plugin-riak2
```

Notice: you need Riak configured using eleveldb as backend.


fluent.conf example
-------------------

```
<match riak2.**>
  type riak2

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

License
=======

Apache 2.0

Copyright Kota UENISHI, Collective Health, Inc.

Kota UENISHI's original message:

Many Thanks to fluent-plugin-mongo

Collective Health, Inc. would like to thank Kota UENISHI for his effort and his decision to release the work under the Apache 2 license.
