FROM logstash:5.4.2-alpine

RUN mkdir -p /log/volumes && \
    mkdir -p /log/docker
COPY logstash.conf /etc/logstash.conf

ENTRYPOINT ["logstash"]
CMD ["-f", "/etc/logstash.conf"]
