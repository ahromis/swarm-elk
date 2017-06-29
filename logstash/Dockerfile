FROM logstash:5.4.2-alpine

RUN mkdir -p /log
COPY logstash.conf /etc/logstash.conf

CMD ["-f", "/etc/logstash.conf"]
