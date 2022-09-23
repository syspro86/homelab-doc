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

## 절반 이상 노드가 down 되어 vm 시작/종료 등이 안될때 votes 수를 임시 조정한다.

```
pvecm expected 1
# 1 대신 온라인 노드 수를 입력한다.

```

## 클러스터 노드 지우기

```
pvecm delnode nodename
```
