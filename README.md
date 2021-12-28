# Elastic stack (ELK) on Docker for monitoring SAP Commerce (Hybris)

Based on [docker-elk](https://github.com/deviantony/docker-elk) which based on the official Docker images from Elastic:

* [Elasticsearch](https://github.com/elastic/elasticsearch/tree/master/distribution/docker)
* [Logstash](https://github.com/elastic/logstash/tree/master/docker)
* [Kibana](https://github.com/elastic/kibana/tree/master/src/dev/build/tasks/os_packages/docker_generator)

## Requirements

### Host setup

* [Docker Engine](https://docs.docker.com/install/) version **17.05** or newer
* [Docker Compose](https://docs.docker.com/compose/install/) version **1.20.0** or newer
* 1.5 GB of RAM

*:information_source: Especially on Linux, make sure your user has the [required permissions][linux-postinstall] to
interact with the Docker daemon.*

By default, the stack exposes the following ports:

* 5044: Logstash Beats input
* 5000: Logstash TCP input
* 9600: Logstash monitoring API
* 9200: Elasticsearch HTTP
* 9300: Elasticsearch TCP transport
* 5601: Kibana

## Usage

1. Clone this repository
2. Set your *hybris_log* path in docker-compose.yml in the section logstash.volumes:

```yaml
volumes:
      - ./logstash/config/logstash.yml:/usr/share/logstash/config/logstash.yml:ro,z
      - ./logstash/pipeline:/usr/share/logstash/pipeline:ro,z
      - <CHANGE_ME>:/var/log/hybris/:ro,z
```
For exmaple it could be:
```yaml
volumes:
      - ./logstash/config/logstash.yml:/usr/share/logstash/config/logstash.yml:ro,z
      - ./logstash/pipeline:/usr/share/logstash/pipeline:ro,z
      - /opt/hybris/log/tomcat/:/var/log/hybris/:ro,z:/var/log/hybris/:ro,z
```
3. Run containers:

```console
$ docker-compose up
```

You can also run all services in the background (detached mode) by adding the `-d` flag to the above command.

### Cleanup

Elasticsearch data is persisted inside a volume by default.

In order to entirely shutdown the stack and remove all persisted data, use the following Docker Compose command:

```console
$ docker-compose down -v
```

