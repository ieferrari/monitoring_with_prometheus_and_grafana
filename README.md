# Infrastucture for Prometheus & Grafana monitoring

Install Node_exporter in the servers you want to monitor. 
Install Prometheus & grafana in the central server that will monitor all the nodes.

------------------------------------------------
## Node_exporter

This will create a server in 0.0.0.0:9100/metrics with all the metrics of the machine.
Then, you pass the_ip_of_the_machine:9100 to prometheus to make it gather the data.

To create the service, run:

    docker-compose -f docker-compose.node_exporter.yml up

To test if the service is working, run:

    curl localhost:9100

To destroy the service, run:

    docker-compose -f docker-compose.node_exporter.yml down

------------------------------------------------
## Prometheus

This will create a service in 0.0.0.0:9090 to gather all the data from the target node_exporters

To create the service, run:

    docker-compose -d -f docker-compose.prometheus.yml up

To destroy the service, run:

    docker-compose -f docker-compose.prometheus.yml down


Edit the prometheus.yml file in the config folder to add ips of target machines with node_exporter running

    static_configs:
      - targets: ['server1_ip:9100','server2_ip:9100']
        labels:
          groups: 'my_servers'

------------------------------------------------
## Grafana

This will create a web graphical user interface to import and create dashboards for prometheus data

    docker-compose -d -f docker-compose.graphana.yml up

To destroy the service, run:

    docker-compose -f docker-compose.graphana.yml down

open a browser and go to:
* ip_of_this_machine:3000
* user: admin
* password: admin
* create new dashboard
  * configuration >> data sources >> add data source >> prometheus >> url: http://localhost:9090 >> save & test
  * create >> import >> 1860 >> load >> <my_dashboard_name> >> select prometheus >> create

