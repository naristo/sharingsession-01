# [Configuration](./README.md)

## Coordinator
```bash
node.master: false 
node.voting_only: false 
node.data: false 
node.ingest: false 
node.ml: false 
xpack.ml.enabled: true 
node.transform: false 
xpack.transform.enabled: true 
node.remote_cluster_client: true 
```

## Master-Data
```bash
node.master: true 
node.data: true 
node.ingest: false 
node.ml: true 
xpack.ml.enabled: true 
node.remote_cluster_client: true
```

## Ingest
```bash
node.master: false 
node.voting_only: false 
node.data: false 
node.ingest: true 
node.ml: false 
node.transform: false 
node.remote_cluster_client: false 
```

### Generic 
```bash
cluster.name: my-cluster
node.name: my-node-hostname
path.data: /data/elasticsearch/data
path.logs: /data/elasticsearch/log
bootstrap.memory_lock: true
network.host: 0.0.0.0
http.port: 9200
cluster.initial_master_nodes: ["IP_MASTER_01","IP_MASTER_02","IP_MASTER_03","IP_MASTER_04","IP_MASTER_05"]
discovery.seed_hosts: ["IP_MASTER_1:9300","IP_MASTER_2:9300","IP_MASTER_3:9300"]
action.destructive_requires_name: true
action.auto_create_index: .security,.kibana*,.monitoring*,.watches,.triggered_watches,.watcher-history*,logstash*,packetbeat*,filebeat*,metricbeat*,*
indices.query.bool.max_clause_count: 10000
search.max_buckets: 20000

```