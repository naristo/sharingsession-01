# Securing Elasticsearch Cluster
* Secure communication between elasticsearch nodes
* secure communication from client to elasticsearch cluster

## Pre-requisite
  * set `xpack.security.enabled` setting to `true` in `elasticsearch.yml`
  * generate node certificate
  * config TLS on transport layer
  * config TLS on http layer


## Generate Certificate
* [Use Self-Sign]()
* [Use private CA](./create-certificate-private-ca.md)


## Generate Certificate with Self-Sign

* create list of instance in yaml format
```yml
instances:
  - name: "node1" 
    ip: 
      - "192.0.2.1"
    dns: 
      - "node1.mydomain.com"
  - name: "node2"
    ip:
      - "192.0.2.2"
      - "198.51.100.1"
  - name: "node3"
  - name: "node4"
    dns:
      - "node4.mydomain.com"
      - "node4.internal"
  - name: "coordinator1"
    ip: 
      - "192.0.2.10"
    dns: 
      - "cnode1.mydomain.com"
      - "elastic.mydomain.com"
  - name: "coordinator2"
    ip: 
      - "192.0.2.11"
    dns: 
      - "cnode2.mydomain.com"
      - "elastic.mydomain.com"
  - name: "load-balancer"
    ip: 
      - "192.0.2.12"
    dns: 
      - "f5vshostname.mydomain.com"
      - "elastic.mydomain.com"

```

```bash
bin/elasticsearch-certutil cert --silent --in instances.yml --out all-certificate.zip --pass testpassword --keep-ca-key

```
Nanti akan ada 1 compressed file dengan nama `all-certificate.zip` dimana terdapat CA, private key dan Certificate untuk setiap parameter `name ` yang ada di file `instance.yml`



