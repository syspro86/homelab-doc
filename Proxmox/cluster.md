# Cluster

## Cluster 정보 초기화

Cluster 정보를 지우고, 가입 이전 상태로 초기화한다.

```
systemctl stop pve-cluster corosync
pmxcfs -l
rm -rf /etc/corosync/*
rm /etc/pve/corosync.conf
killall pmxcfs
systemctl start pve-cluster
```

