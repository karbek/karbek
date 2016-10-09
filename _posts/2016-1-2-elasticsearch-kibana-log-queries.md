---
layout: post
title: Analyze elasticsearch queries with logstash
---

Logstash can be used to analyze various kinds of logs.
We want to analyze elasticsearch slow query logs, especially the queries contained in this file.

There is no existing logstash-plugin for the elasticsearch slow query log. The mapping can be realized with grok. We define a grok-pattern to match the lines of the log file:

```javascript
[2016-10-09 10:41:07,460][DEBUG][index.search.slowlog.query] took[304.3micros], took_millis[0], types[test_type], stats[], search_type[QUERY_THEN_FETCH], total_shards[5], source[], extra_source[{"query":{"query_string":{"query":"text=logstash-test","lowercase_expanded_terms":true,"analyze_wildcard":false}}}],
```

Here is the grok pattern:

```javascript
\[%{TIMESTAMP_ISO8601:timestamp},%{INT:timestamp_ms}\]\[%{LOGLEVEL:loglevel}\]\[%{DATA:logtype}\] took\[%{BASE10NUM:duration_ms}ms\], took_millis\[%{BASE10NUM:duration}\], types\[%{DATA:elasticsearch_type}\], stats\[\], search_type\[%{DATA:elasticsearch_search_type}\], total_shards\[%{INT:shards_total}\], source\[\], extra_source\[\{\"query\":\{\"query_string\":\{\"query\":\"text=%{DATA:query}\",\"lowercase_expanded_terms\":true,\"analyze_wildcard\":false\}\}\}\],
```
