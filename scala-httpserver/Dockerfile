FROM centos:7


ENV JAVA_VERSION="1.8.0"

# --- --- ---  Update OS --- --- ---
RUN yum -y clean all \
  && yum -y install java-$JAVA_VERSION-openjdk wget tar curl unzip which git \
  && yum -y clean all 

ENV JAVA_HOME /usr/lib/jvm/java-$JAVA_VERSION-openjdk

RUN mkdir -p /opt/prometheus \
    && curl http://central.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.7/jmx_prometheus_javaagent-0.7.jar > /opt/prometheus/prometheus.jar

RUN curl -s https://raw.githubusercontent.com/paulp/sbt-extras/master/sbt > /usr/local/bin/sbt  \
    && chmod 0777 /usr/local/bin/sbt

# Clone and build simple http server 

RUN cd /opt \ 
  && git clone https://github.com/gianluca-sabena/boilerplates.git \
  && cd boilerplates/scala/akka-http-metrics \
  && sbt -v assembly


COPY prometheus.yaml /opt/prometheus/prometheus.yaml

CMD java -javaagent:/opt/prometheus/prometheus.jar=9999:/opt/prometheus/prometheus.yaml -jar /opt/boilerplates/scala/akka-http-metrics/target/scala-2.11/app.jar