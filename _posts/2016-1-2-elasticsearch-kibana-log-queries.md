---
layout: post
title: Analyze elasticsearch queries with logstash
---

Logstash can be used to analyze various kinds of logs.
We want to analyze elasticsearch slow query logs, especially the queries contained in this file.

There is no existing logstash-plugin for the elasticsearch slow query log. The mapping can be realized with grok. We define a grok-pattern to match the lines of the log file:

