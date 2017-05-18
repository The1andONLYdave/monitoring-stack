# Monitoring microservice

An opinionated monitoring stack for physical hosts, clusters and microservices 

# Background

Test and learn how to monitor distributed microservices architecture

## Logs OR Metrics OR both

This is a long story. Logs and metrics are totally different. Logs use a lot of bandwidth, storage, and cpu
For example 10 servers logging 1KB for each of 1000 requests per second will fill most of a 100Mbit. [[1](https://grafana.com/blog/2016/01/05/logs-and-metrics-and-graphs-oh-my/)]

In my opinion metrics are the bast data to monitoring a complex infrastructure and relay on 
logs to investigate problems.


## Stack

This stack is based on:

- [Grafana](https://grafana.com/) to query and display metrics 
- [Prometheus](http://prometheus.io) to poll, store and query metrics
- [Alertmanager](https://prometheus.io/docs/alerting/alertmanager/) to send notifications
- [Telegraf](https://docs.influxdata.com/telegraf) to expose physical host metrics in http prometheus format
- [Mtail](https://github.com/google/mtail) to parse log files and expose prometheus metrics

# Prerequisites

This project requires:

- [Docker engine](https://docs.docker.com/engine/installation/) 
- [Docker compose](https://docs.docker.com/compose/)

You can install from <https://docs.docker.com>

# Install

Clone this project `git clone https://github.com/gianluca-sabena/monitoring-stack.git`

Enter folder `cd monitoring-stack`

Build docker images `docker-compose build`

Run docker images `docker-compose up`

Check if services are running:
- Grafana (login with: admin/admin) <http://127.0.0.1:3000> 
- Prometheus <http://127.0.0.1:9090>
- Telegraf <http://127.0.0.1:9126>
- Mtail <http://127.0.0.1:3903>

You may need to replace host 127.0.0.1 with the host/ip where docker engine is running 

Clean up `docker-compose rm`

# Usage

In order to learn how the stack works look at configuration file in application folders 

## Dashboards

Dashboards are created in grafana <http://docs.grafana.org/guides/getting_started/>

Try it yourself, login with user admin and password admin to <http://127.0.0.1:3000>

Check some example dashboards [dashboards](./grafana/dashboards)

Remember to export dashboard json file and store in a folder

## Alerts

Prometheus has an internal alerting system <https://prometheus.io/docs/alerting/rules/>

There is a [cpu usage alert rule](./prometheus/alert.rules) to fire when cpu usage is over 15%

Generate cpu cycle with `docker run -ti alpine sh -c "while true; do true; done"`

Wait one minutes, when the alert fire a vertical red dotted lines will appear in grafana telegraf cpu dashboard <http://127.0.0.1:3000/dashboard/file/telegraf.json>



## Logs

In case some metrics or events are only available in a log file, the approach is to use [mtail](https://github.com/google/mtail) to constantly
parse log files and convert it to a numeric metrics (eg. number of matched string, extract a value,..)

See an example [mtail parser](./mtail/progs)


# Additional notes

Telegraf supports multiple output, so it is possible to expose metrics in prometheus and to send them to a remote kafka  

Prometheus can scale <https://prometheus.io/blog/2016/07/23/pull-does-not-scale-or-does-it>

Prometheus new backbends <https://prometheus.io/blog/2017/04/10/promehteus-20-sneak-peak>


# Further reading

- [1] <https://grafana.com/blog/2016/01/05/logs-and-metrics-and-graphs-oh-my>
- [2] How to write good alerts <https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit>

