# [Data Retention](./README.md)

Data retention di Elastic di atur  di ILM ( Index Lifecycle Management), di mana index yang di create akan di maintain datanya sesuai policy yang sudah di tentukan.

Step dalam pembuatan policy ILM sbb:
- Create ILM Policy
- Crate Index template dengan menambahkan configurasi ILM 



### [Create Simple ILM Policy with delete index after 90 days]()
```bash
PUT _ilm/policy/90-days-retentions-data
{
  "policy": {
    "phases": {
      "hot": {
        "actions": {
          "set_priority": {
            "priority": 100
          }
        }
      },
      "delete": {
        "min_age": "90d",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}

```

### [ILM Policy with reduce replica after 45 days and delete after 90 days]()
```bash
PUT _ilm/policy/90-days-retentions-data-with-reduce-replica
{
  "policy": {
    "phases": {
      "hot": {
        "min_age": "0ms",
        "actions": {
          "set_priority": {
            "priority": 100
          }
        }
      },
      "warm": {
        "min_age": "45d",
        "actions": {
          "allocate": {
            "number_of_replicas": 0
          },
          "set_priority": {
            "priority": null
          }
        }
      },
      "delete": {
        "min_age": "90d",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}
```

### [Create index template]()
```bash
PUT _index_template/myapps
{
  "priority": 70,
  "template": {
    "settings": {
      "index": {
        "lifecycle": {
          "name": "90-days-retentions-data"
        },
        "number_of_replicas": "1"
      }
    }
  },
  "index_patterns": [
    "myapps-*"
  ],
  "composed_of": []
}
```

