# [Troubleshoot](./README.md)

- [check cluster health](###check-cluster-health)
- [check nodes](###check-nodes)
- [check running task](###running-task-in-cluster)
- [resolve unassign shard](###resolve-unassign-shard)

### [Check Cluster Health](#troubleshoot)
```bash
curl -XGET IP:9200/_cluster/health?pretty
```
```json
#sample response
{
  "cluster_name" : "testcluster",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 1,
  "active_shards" : 1,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 1,
  "delayed_unassigned_shards": 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch": 0,
  "task_max_waiting_in_queue_millis": 0,
  "active_shards_percent_as_number": 50.0
}
```

### [Check Nodes](#troubleshoot)
```bash
# If no filters are given, the default is to select all nodes
curl -X GET "localhost:9200/_nodes?pretty"
# Explicitly select all nodes
curl -X GET "localhost:9200/_nodes/_all?pretty"
# Select just the local node
curl -X GET "localhost:9200/_nodes/_local?pretty"
# Select the elected master node
curl -X GET "localhost:9200/_nodes/_master?pretty"
# Select nodes by name, which can include wildcards
curl -X GET "localhost:9200/_nodes/node_name_goes_here?pretty"
curl -X GET "localhost:9200/_nodes/node_name_goes_*?pretty"
# Select nodes by address, which can include wildcards
curl -X GET "localhost:9200/_nodes/10.0.0.3,10.0.0.4?pretty"
curl -X GET "localhost:9200/_nodes/10.0.0.*?pretty"
# Select nodes by role
curl -X GET "localhost:9200/_nodes/_all,master:false?pretty"
curl -X GET "localhost:9200/_nodes/data:true,ingest:true?pretty"
curl -X GET "localhost:9200/_nodes/coordinating_only:true?pretty"
curl -X GET "localhost:9200/_nodes/master:true,voting_only:false?pretty"
# Select nodes by custom attribute (e.g. with something like `node.attr.rack: 2` in the configuration file)
curl -X GET "localhost:9200/_nodes/rack:2?pretty"
curl -X GET "localhost:9200/_nodes/ra*:2?pretty"
curl -X GET "localhost:9200/_nodes/ra*:2*?pretty"

```

### running task in cluster(#troubleshoot)
```bash
GET _cat/tasks?v
```
```bash
action                         task_id                      parent_task_id               type      start_time    timestamp running_time ip           node
cluster:monitor/tasks/lists    Q-U9h5g8RVOoH2IZXxWC2A:30539 -                            transport 1598460961855 16:56:01  280.6micros  10.46.104.25 instance-0000000001
cluster:monitor/tasks/lists[n] Q-U9h5g8RVOoH2IZXxWC2A:30540 Q-U9h5g8RVOoH2IZXxWC2A:30539 direct    1598460961855 16:56:01  96.1micros   10.46.104.25 instance-0000000001
cluster:monitor/tasks/lists[n] yllo_ap0TYm1AD1Vl9F42A:21853 Q-U9h5g8RVOoH2IZXxWC2A:30539 transport 1598460961856 16:56:01  271.1micros  10.46.104.18 instance-0000000000
cluster:monitor/tasks/lists[n] LoWhNcb_SsaiP50fL8Lfhg:9562  Q-U9h5g8RVOoH2IZXxWC2A:30539 transport 1598460961877 16:56:01  139.2micros  10.46.104.4  tiebreaker-0000000002

```

### [resolve unassign shard](#troubleshoot)

Check for unassigned shard and the reason
```bash
GET /_cluster/allocation/explain

```
normal response
```bash
{
  "error" : {
    "root_cause" : [
      {
        "type" : "illegal_argument_exception",
        "reason" : "unable to find any unassigned shards to explain [ClusterAllocationExplainRequest[useAnyUnassignedShard=true,includeYesDecisions?=false]"
      }
    ],
    "type" : "illegal_argument_exception",
    "reason" : "unable to find any unassigned shards to explain [ClusterAllocationExplainRequest[useAnyUnassignedShard=true,includeYesDecisions?=false]"
  },
  "status" : 400
}

```
```bash
POST /_cluster/reroute&retry_failed=true
```