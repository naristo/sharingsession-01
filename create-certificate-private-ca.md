# Generate Certificate using Internal/Company/Private CA

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
bin/elasticsearch-certutil csr --silent --in instances.yml --out all-certificate.zip --pass testpassword --keep-ca-key

```
Nanti akan ada 1 compressed file dengan nama `all-certificate.zip` dimana terdapat hanya certificate signing request (.csr) dan private key (*.key) parameter `name ` yang ada di file `instance.yml`

Dimana proses berikutnya generate certificate dari file (*.csr) ke CA



