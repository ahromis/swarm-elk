# ELK Stack in Swarm Mode

This repository uses the new Docker Compose v3 format to create an ELK stack using swarm mode.

## Prerequisites:

1. Docker Engine 1.13+
2. Docker Compose 1.10+
3. Set the following on nodes that will be running Elasticsearch:
    1. `sysctl -w vm.max_map_count=262144`
    2. `echo 'vm.max_map_count=262144' >> /etc/sysctl.conf` (to persist reboots)

## Usage

1. Clone this repository
2. On a machine that is communicating with the swarm cluster:
    1. `docker stack deploy -c $(pwd)/docker-compose.yml elk`

That will bring up the ELK stack.

> **Note:** You may want to remove the stdout output using the `rubydebug` codec after confirming everything works as you expect. By leaving the stdout output enabled it would be too much output for most environments. Also you would want to increase the Elasticsearch heap size and memory reservations for most deployments.

If using UCP then you can access Kibana via the HRM label, which should be set to the URL you want to use for access to Kibana. Otherwise access Kibana via the service's published port, which is 5601 in this compose file.

## HA Elasticsearch

Elasticsearch can be run in an HA configuration after the initial stack comes up. The first node needs to register as healthy before scaling it out. After the initial Elasticsearch member is healthy, then it can be scaled.

1. Find the Elasticsearch service ID:
  1. `docker service ls`
1. Scale out the service to include more replicas:
  1. `docker service update --replicas=3 <replica_id>`

### Testing

Run the following container:

`docker run --rm -it --log-driver=gelf --log-opt gelf-address=udp://<logstash-host>:12201 alpine ping 8.8.8.8`

Login to Kibana and you should see traffic coming into Elasticsearch.

If you do a `docker logs` on the logstash container and you should see some messages coming through as well.

You can set logs at the engine level as well by specifying `--log-opt gelf-address=udp://host:port` in your daemon arguments. Or you can use syslog as well as TLS if you wish to add in your own certs.

### TODO

- Add an input buffer like redis, rabbitmq, or kafka
