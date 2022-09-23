# SWAP 설정

```lang-bash
# 현재 값 확인
cat /proc/sys/vm/swappiness

# 최대한 swap을 활용하지 않도록 한다. 끄는것이 아님.
sysctl vm.swappiness=0

# swap을 껐다켜서 적용한다.
swapoff -a
swapon -a

# 바뀐 값 확인
cat /proc/sys/vm/swappiness
```
