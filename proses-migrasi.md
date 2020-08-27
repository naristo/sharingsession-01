# [Data Migration](./README.md)

## Logstash Reindex

### general
```yaml
input {
      elasticsearch {
        hosts => "ip:9200"
        index => "ap-xka.20.01.01"
        query => '{ "query": { "query_string": { "query": "*" } } }'
        size => 1000
        scroll => "5m"
      }
    }
filter {

}

output {
  elasticsearch {
    hosts    => [ "IP:9200" ]
    index => "%{[@metadata][_index]}"
    document_id => "%{[@metadata][_id]}"
  }
  file {
    path => "./ap-xka.20.01.01.txt"
  }
  
}

```

### Spesifik untuk index ap-xka

```bash
#GET template dari cluster origin
GET ap-xka-yyyy.mm.dd

Copy `mappings: {...}`

#Create Template ap-xka baru
PUT _template/ap-xka
{
  "index_patterns": [
    "ap-xka*"
  ],
  "mappings": {
    "properties": {
      "original_date": {
        "type": "date",
        "format": "yyMMddHHmmssSSSZ"
      },
      "ACQ": {
        "type": "short"
      },
      "AVAILBAL": {
        "type": "float"
      },
      "CFIID": {
        "type": "keyword"
      },
      "DES": {
        "type": "short"
      },
      "ENTMDE": {
        "type": "short"
      },
      "FROMACCT": {
        "type": "keyword"
      },
      "ISS": {
        "type": "short"
      },
      "KARTU": {
        "type": "keyword"
      },
      "MPUT": {
        "type": "text"
      },
      "MTI": {
        "type": "short"
      },
      "NOPLG": {
        "type": "keyword"
      },
      "NORCD": {
        "type": "text"
      },
      "REGCDE": {
        "type": "keyword"
      },
      "RESN": {
        "type": "byte"
      },
      "RESPCDE": {
        "type": "keyword"
      },
      "RSPDR": {
        "type": "byte"
      },
      "TCDER": {
        "type": "keyword"
      },
      "TERMCAP": {
        "type": "text"
      },
      "TFIID": {
        "type": "keyword"
      },
      "TID": {
        "type": "keyword"
      },
      "TIM": {
        "type": "text"
      },
      "TOACCT": {
        "type": "keyword"
      },
      "TRANAMT": {
        "type": "long"
      },
      "UTLCDE": {
        "type": "keyword"
      }
    }
  },
  "settings": {
    "number_of_shards": "1",
    "number_of_replicas": "1"
  }
}
```
### Gunakan Config Logstash ini
```bash
input { 
      elasticsearch {
        hosts => "ip:9200"
        index => "ap-xka.20.01.01"
        query => '{ "query": { "query_string": { "query": "*" } } }'
        size => 1000
        scroll => "5m"
      }
}

filter {
  if "_dateparsefailure" in [tags] or "_timeparsefailure" in [tags] {
    mutate {
         copy => { "_@timestamp" => "original_date" }
         remove_field => [ "@timestamp","_@timestamp" ]
    }
    date {
      match => [ "original_date", "yyMMddHHmmssSSSZ" ]
    }
  }
}

output {
  elasticsearch {
    hosts    => [ "IP_CLUSTER_BARU:9200" ]
    index => "%{[@metadata][_index]}"
    document_id => "%{[@metadata][_id]}"
  }
}
    
```
#### [top](#data-migration)
