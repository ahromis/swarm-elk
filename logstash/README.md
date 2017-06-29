# ELK stack with logbook

This was created to make a more pluggable logging solution with Docker.

## Usage

1. Follow the steps in the [logbook](https://github.com/ahromis/logbook) repo
2. Clone this repo
3. Set the following on nodes that will be running Elasticsearch:
    1. `sysctl -w vm.max_map_count=262144`
    2. `echo 'vm.max_map_count=262144' >> /etc/sysctl.conf` (to persist reboots)
4. `docker stack deploy -c ./logstash/docker-compose.yml elk`
5. Access Kibana by going to `http://<worker_node_ip>:5601`

## HA Elasticsearch

Elasticsearch can be run in an HA configuration after the initial stack comes up. The first node needs to register as healthy before scaling it out. After the initial Elasticsearch member is healthy, then it can be scaled.

1. Find the Elasticsearch service ID:
    1. `docker service ls`
2. Scale out the service to include more replicas:
    1. `docker service update --replicas=3 <replica_id>`

