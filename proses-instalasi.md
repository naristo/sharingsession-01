# [Instalation](./README.md)

## Elasticsearch Instalation

Untuk metode instalasi yang digunakan menggunakan package rpm 
```bash
yum -y localinstall elasticsearch-{versi}-x86_64.rpm
```

```bash
terdapat problem di awal instalasi dan ini di karenakan source rpm yang digunakan ternyata corrupt.
resolusi : download ulang package installer
```

## Pre-Req on every elasticseach nodes

```bash
## file limit
vim /etc/security/limits.conf
add line : elasticsearch  -  nofile  65535

## memory lock

vim /usr/lib/systemd/system/elasticsearch.service
tambahkan line : LimitMEMLOCK=infinity
dibawah line [Service]
di save
jalankan command : sudo systemctl daemon-reload

## disable swap
jalankan command  : sudo swapoff -a
vim /etc/ftab 
tambahkan # di awal baris yang ada kata2 swap

## virtual memory
vim /etc/sysctl.conf
tambahkan vm.max_map_count=262144
exit + save
jalankan command sysctl -p
```
