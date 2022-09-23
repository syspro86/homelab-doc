# 디스크 관련 명령어

디스크 SMART 정보

```
smartctl --all /dev/sdc

```

배드섹터 검사

```
badblocks -svw -b 4096 /dev/sdc

-s 진행사항을 표시한다
-v verbose
-w write를 수행하며 테스트한다. (데이터 덮어씀). 읽기만 테스트하려면 -n
-b 4096 블럭 사이즈를 지정한다. 기본 1024.
```
