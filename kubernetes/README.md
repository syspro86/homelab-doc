# kubernetes

POD 강제 종료
```
kubectl delete pod <PODNAME> --grace-period=0 --force --namespace <NAMESPACE>
```
