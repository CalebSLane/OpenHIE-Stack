# image based on fluentd v1.16-1
FROM docker.elastic.co/beats/filebeat:7.13.0

COPY filebeat.yml /usr/share/filebeat/filebeat.yml

USER root

# Create a directory to map volume with all docker log files RUN mkdir /usr/share/filebeat/dockerlogs

RUN chown -R root /usr/share/filebeat/

RUN chmod -R go-w /usr/share/filebeat/

# Use root account to use apk
USER root