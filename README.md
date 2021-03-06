# kafka-topics

[![release](http://github-release-version.herokuapp.com/github/landoop/kafka-topics-ui/release.svg?style=flat)](https://github.com/landoop/kafka-topics-ui/releases/latest)
[![docker](https://img.shields.io/docker/pulls/landoop/kafka-topics-ui.svg?style=flat)](https://hub.docker.com/r/landoop/kafka-topics-ui/)
[![Join the chat at https://gitter.im/Landoop/support](https://img.shields.io/gitter/room/nwjs/nw.js.svg?maxAge=2592000)](https://gitter.im/Landoop/support?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Browse Kafka topics and understand what's happening on your cluster. Find topics / view topic metadata / browse topic data (kafka messages) / view topic configuration / download data. This is a web tool for the [confluentinc/kafka-rest proxy](https://github.com/confluentinc/kafka-rest).

## Live Demo
[kafka-topics-ui.landoop.com](http://kafka-topics-ui.landoop.com)

## Running it

```
    docker pull landoop/kafka-topics-ui
    docker run --rm -it -p 8000:8000 \
               -e "KAFKA_REST_PROXY_URL=http://kafka-rest-proxy-host:port" \
               -e "PROXY=true" \
               landoop/kafka-topics-ui
```

**Config:** If you don't use our docker image, keep in mind that `Kafka-REST-Proxy`
CORS support can be a bit buggy, so if you have trouble setting it up, you may need
to provide CORS headers through a proxy (i.e. nginx).

**Note:** The schema-registry is optional and topics are attempted to be read using Avro,
then fall back to JSON, and finally fall back to Binary.

## Build from source

```
    git clone https://github.com/Landoop/kafka-topics-ui.git
    cd kafka-topics-ui
    npm install
    http-server .
```
Web UI will be available at `http://localhost:8080`

### Nginx config

If you use `nginx` to serve this ui, let angular manage routing with
```
    location / {
      add_header 'Access-Control-Allow-Origin' "$http_origin" always;
      add_header 'Access-Control-Allow-Credentials' 'true' always;
      add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
      add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Mx-ReqToken,X-Requested-With' always;

      proxy_pass http://kafka-rest-server-url:8082;
      proxy_redirect off;

      proxy_set_header  X-Real-IP  $remote_addr;
      proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header  Host $http_host;
    }
```

### Setup Kafka Rest clusters

Use multiple Kafka Rest clusters in `env.js` :
```
var clusters = [
  {
    NAME:"prod",
    KAFKA_REST : "prod.url.com",// "The Kafka Rest url"
    MAX_BYTES: "?max_bytes=50000",
    COLOR: "#141414" // Optional
  },
  {
  NAME:"dev",
  KAFKA_REST : "dev.url.com", "The Kafka Rest url"
  MAX_BYTES: "?max_bytes=50000",

  COLOR: "red" // Optional
  }
]

```
* Use `MAX_BYTES` to set the default maximum amount of bytes to fetch from each topic.
* Use `COLOR` to set different header colors for each set up cluster.

## Changelog
[Here](https://github.com/Landoop/kafka-topics-ui/wiki/Changelog)

## License

The project is licensed under the [BSL](http://www.landoop.com/bsl) license.

## Relevant Projects

* [schema-registry-ui](https://github.com/Landoop/schema-registry-ui), View, create, evolve and manage your Avro Schemas for multiple Kafka clusters
* [kafka-connect-ui](https://github.com/Landoop/kafka-connect-ui), Set up and manage connectors for multiple connect clusters
* [fast-data-dev](https://github.com/Landoop/fast-data-dev), Docker for Kafka developers (schema-registry,kafka-rest,zoo,brokers,landoop) 
* [Landoop-On-Cloudera](https://github.com/Landoop/Landoop-On-Cloudera), Install and manage your kafka streaming-platform on you Cloudera CDH cluster



<img src="http://www.landoop.com/images/landoop-dark.svg" width="13" /> www.landoop.com
