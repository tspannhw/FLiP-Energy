# FLiP-Energy

FLiP for Energy with websockets

# References

* https://pulsar.apache.org/docs/en/client-libraries-websocket/


# Start the server and then the websockets

```

bin/pulsar standalone

bin/pulsar-daemon start websocket

```

# Send to SOLR (https://github.com/tspannhw/FLiP-SOLR/blob/main/README.md)

```

docker exec 03ecc2709715 solr create -c energy

bin/pulsar-admin sink stop --name solr-sink-energy --namespace default --tenant public
bin/pulsar-admin sinks delete --tenant public --namespace default --name solr-sink-energy
bin/pulsar-admin sinks create --tenant public --namespace default --name solr-sink-energy --sink-type solr --sink-config-file conf/solr-sink-energy.yml --inputs energy
bin/pulsar-admin sinks get --tenant public --namespace default --name solr-sink-energy
bin/pulsar-admin sinks status --tenant public --namespace default --name solr-sink-energy

curl "http://localhost:8983/solr/energy/select?q=*"


bin/pulsar-admin schemas upload energy -f energy-schema.json

bin/pulsar-admin schemas get energy



```


# Reference

* https://toolslick.com/generation/metadata/avro-schema-from-json
* https://pulsar.apache.org/docs/en/schema-manage/
* https://github.com/tspannhw/CloudDemo2021/blob/main/schemas/energy.avsc
