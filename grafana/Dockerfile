FROM grafana/grafana:4.2.0

RUN apt-get update && \
    apt-get -y --no-install-recommends install curl ca-certificates && \
    apt-get clean
    
COPY start.sh /start.sh
COPY grafana.ini /etc/grafana/grafana.ini 
COPY dashboards/* /var/lib/grafana/dashboards/

ENTRYPOINT ["/start.sh"]
