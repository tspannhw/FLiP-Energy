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

docker exec 03ecc2709715 solr delete -c energy

docker exec 03ecc2709715 solr create -c energy

docker exec 03ecc2709715 solr delete -c docs

docker exec 03ecc2709715 solr create -c docs


# example build schema
# curl http://localhost:8983/solr/films/schema \\
#-X POST -H 'Content-type:application/json' --data-binary '{
#    "add-field" : {
#        "name":"name",
#        "type":"text_general",
#        "multiValued":false,
#        "stored":true
#    },
#    "add-field" : {
#        "name":"initial_release_date",
#        "type":"tdate",
#        "stored":true
#    }
#}'


bin/pulsar-admin topics list public/default

bin/pulsar-admin topics subscriptions persistent://public/default/energy
  
bin/pulsar-admin topics unsubscribe --subscription energy-checker persistent://public/default/energy
bin/pulsar-admin topics unsubscribe --subscription nifi-pulsar-energy persistent://public/default/energy
  
bin/pulsar-admin topics unload persistent://public/default/energy

bin/pulsar-admin topics delete persistent://public/default/energy
bin/pulsar-admin topics delete persistent://public/default/energy-partition-0

bin/pulsar-admin schemas delete energy

bin/pulsar-admin schemas upload energy -f energy-schema.json

bin/pulsar-admin schemas get energy

bin/pulsar-client consume energy -s "energy-checker"

bin/pulsar-admin sink stop --name solr-sink-energy --namespace default --tenant public
bin/pulsar-admin sinks delete --tenant public --namespace default --name solr-sink-energy
bin/pulsar-admin sinks create --tenant public --namespace default --name solr-sink-energy --sink-type solr --sink-config-file conf/solr-sink-energy.yml --inputs energy
bin/pulsar-admin sinks get --tenant public --namespace default --name solr-sink-energy
bin/pulsar-admin sinks status --tenant public --namespace default --name solr-sink-energy

curl "http://localhost:8983/solr/energy/select?q=*"

docker exec 03ecc2709715 solr config -c energy -p 8983 -action set-user-property -property update.autoCreateFields -value false


```


# Reference

* https://toolslick.com/generation/metadata/avro-schema-from-json
* https://pulsar.apache.org/docs/en/schema-manage/
* https://github.com/tspannhw/CloudDemo2021/blob/main/schemas/energy.avsc
