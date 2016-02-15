---
layout: post
title: Logstash filter config to read all (un)known incoming fields!
---

Long time, no see folks! :) 

I will try hard to be punctual in writing blog posts from now!

#### Problem statement begin
We had a logstash consumer which would read data from multiple collectd agents and forward that to Kafka for further processing. 

#### Problem statement end

Thankfully, logstash has a `collectd` codec which makes our lives easy. In a nutshell, it reads events from the collectd binary protocol over the network via udp. If you want to find out more, go [here](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-collectd.html)

We follow strict `Schema on Read`. It essentially means that you read in the entire data without filtering or removing any of the fields and store it wherever (HDFS, S3 etc). 

But we hit a roadblock pretty quickly as different `collectd` plugins have different fields and we didn't want to edit logstash to include new fields as and when we enable the plugin at the agents.

So, how do we store all the (un)known fields which collectd agents send?

#### Solution

Read in the entire `event` hash object via the `ruby` filter and populate the `message` event with a json blob of all the fields.

Here's the code:

<script src="https://gist.github.com/91pavan/f38eac2286efcfe5f739.js"></script>

Hope it's useful.

Until next time!