# kubernetes

POD 강제 종료
```
kubectl delete pod <PODNAME> --grace-period=0 --force --namespace <NAMESPACE>
```

### 객체가 지워지지 않는 경우

다른 자원에 의해 대기중이거나, 실수를 방지하기 위해 막힌 상태일 수 있다.
객체 스펙의 finalizers 를 null이나 [] 로 변경해준다.

### prometheus 에서 등록

metric 정보를 주는 연결 정보를 넣으면 자동 수집된다.

```
annotations:
  prometheus.io/path: /metrics
  prometheus.io/port: "5000"
  prometheus.io/scrape: "true"
```

### livenessProbe

pod가 정상적으로 실행 중인지 판단하는 정보를 입력할 수 있다. (http 호출, port 오픈 등). 아래는 명령 실행하여 판단하는 방법.

```
livenessProbe:
   exec:
     command:
       - /bin/sh
       - '-c'
       - /usr/src/liveness_node.sh
   initialDelaySeconds: 60
```


