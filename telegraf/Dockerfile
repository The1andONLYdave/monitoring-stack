# https://github.com/influxdata/influxdata-docker/blob/master/telegraf/1.3/Dockerfile
FROM telegraf:1.3

COPY telegraf.conf /telegraf.conf

CMD telegraf --debug --config /telegraf.conf 