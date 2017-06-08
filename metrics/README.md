# Metrics stack

This stack is based on:

- [Grafana](https://grafana.com/) to query and display metrics 
- [Prometheus](http://prometheus.io) to poll, store and query metrics
- [Alertmanager](https://prometheus.io/docs/alerting/alertmanager/) to send notifications
- [Telegraf](https://docs.influxdata.com/telegraf) to expose physical host metrics in http prometheus format
- [Mtail](https://github.com/google/mtail) to parse log files and expose prometheus metrics

## Prerequisites

This project requires:

- [Docker engine](https://docs.docker.com/engine/installation/) 
- [Docker compose](https://docs.docker.com/compose/)

You can install from <https://docs.docker.com>

## Install

Clone this project `git clone https://github.com/...`

Enter folder `cd monitoring-stack`

(Optional) Update smtp server and user emails in [alertmanager config](./alertmanager/config.yml#l6)

Build docker images `docker-compose build`

Run docker images `docker-compose up`

Check if services are running:
- Grafana (login with: admin/admin) <http://127.0.0.1:3000> 
- Prometheus <http://127.0.0.1:9090>
- Alertmanager <http://127.0.0.1:9093>
- Telegraf <http://127.0.0.1:9126>
- Mtail <http://127.0.0.1:3903>

You may need to replace host 127.0.0.1 with the host/ip where docker engine is running 

Clean up `docker-compose rm`

## Http proxy

To run docker-compose with http proxy 

- configure docker-engine to support http proxy, this step is specific for the OS (linux, centos, ubuntu, docker for mac, ...) search the relative docs
- Edit `docker-compose` and enable proxy variables (see inline comments) 
- set this env variables in your terminal before run docker-compose 
```
HTTP_PROXY=...
http_proxy=...
HTTPS_PROXY=..
https_proxy=...
NO_PROXY='127.0.0.1, localhost, /var/run/docker.sock'
no_proxy='127.0.0.1, localhost, /var/run/docker.sock'
```

Please note that `NO_PROXY` variable must match running environment. Docker client call docker engine with http requests, add hostname or ip of docker engine host to no_proxy env var.


## Usage

In order to learn how the stack works look at configuration file in application folders 

### Dashboards

Dashboards are created in grafana <http://docs.grafana.org/guides/getting_started/>

Try it yourself, login with user admin and password admin to <http://127.0.0.1:3000>

Check some example dashboards [dashboards](./grafana/dashboards)

Remember to export dashboard json file and store in a folder

### Alerts

Prometheus has an internal alerting system <https://prometheus.io/docs/alerting/rules/>

There is a [cpu usage alert rule](./prometheus/alert.rules) to fire when cpu usage is over 15%

Generate cpu cycle with `docker run -ti alpine sh -c "while true; do true; done"`

Wait one minutes, when the alert fire a vertical red dotted lines will appear in grafana telegraf cpu dashboard <http://127.0.0.1:3000/dashboard/file/telegraf.json>

### Alertmanager

**This section needs further works - Notification rules may not work!**

Documentation: <https://prometheus.io/docs/alerting/alertmanager/>

Alertmanager receives fired alerts from prometheus, processes a notification rules (filter based on labels, group by multiple alerts, multiple groups notifications,..) and sends out notification by mail on other channels 

To see alerts in you inbox remember to update smtp server and user emails in [alertmanager config](./alertmanager/config.yml#l6)



### Mtail for logs

In case some metrics or events are only available in a log file, the approach is to use [mtail](https://github.com/google/mtail) to constantly
parse log files and convert it to a numeric metrics (eg. number of matched string, extract a value,..)

See an example [mtail parser](./mtail/progs)


## Additional notes

Telegraf supports multiple output, so it is possible to expose metrics in prometheus and to send them to a remote kafka  

Prometheus can scale <https://prometheus.io/blog/2016/07/23/pull-does-not-scale-or-does-it>

Prometheus new backbends <https://prometheus.io/blog/2017/04/10/promehteus-20-sneak-peak>


# Further reading

- [1] How to write good alerts <https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit>

