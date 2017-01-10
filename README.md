# ELK Stack in Swarm Mode

This repository uses the new Docker Compose v3 format to create an ELK stack using swarm mode.

## Prerequisites:

1. Docker Swarm 1.12+
2. Docker Compose 1.10+
3. Set the following on nodes that will be running Elasticsearch:
    1. `sysctl -w vm.max_map_count=262144`
    2. `echo 'vm.max_map_count=262144' >> /etc/sysctl.conf` (to persist reboots)

## Usage

1. Clone this repository
2. On a machine that is communicating with the swarm cluster:
    1. `docker stack deploy -c $(pwd)/docker-compose.yml logging`

That will bring up the ELK stack.

> **Note:** You may want to remove the stdout output using the `rubydebug` codec after confirming everything works as you expect. By leaving the stdout output enabled it would be too much output for most environments.

If using UCP then you can access Kibana via the HRM label, which should be set to the URL you want to use for access to Kibana. Otherwise access Kibana via the service's published port, which is 5601 in this compose file.

### Testing

Run the following container:

`docker run -dit --log-driver=gelf --log-opt gelf-address=udp://<logstash-host>:12201 alpine ping 8.8.8.8`

Login to Kibana and you should see traffic coming into Elasticsearch.

You can set logs at the engine level as well by specifying `--log-opt gelf-address=udp://host:port` in your daemon arguments. Or you can use syslog as well as TLS if you wish to add in your own certs.

### TODO

- Cluster Elasticsearch
- Add a buffer like redis, rabbitmq, or kafka
