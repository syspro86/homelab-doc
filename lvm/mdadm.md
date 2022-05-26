# mdadm

### raid5 생성

디스크들에 단일 파티션을 만든 상태로 구성하는 경우

```
mdadm --create /dev/md0 --level=5 --raid-devices=4 /dev/sd[abcd]1
```

파티션 없이 디스크를 통으로 RAID 구성하는 경우 (파티션번호1을 빼준다)

```
mdadm --create /dev/md0 --level=5 --raid-devices=4 /dev/sd[abcd]
```

디스크가 현재 3개 있지만, 곧 디스크를 추가할 예정인 경우

```
mdadm --create /dev/md0 --level=5 --raid-devices=4  /dev/sd[abc]1 missing
# 디스크 추가 후
mdadm /dev/md0 -a /dev/sdd1
```


### 시놀로지 SHR 방식으로 구성하기

10T, 10T, 14T, 14T 디스크로 SHR 를 구성하려는 경우

- /dev/sda1 (10T)
- /dev/sdb1 (10T)
- /dev/sdc1 (10T) /dev/sdc2 (4T)
- /dev/sdd1 (10T) /dev/sdd2 (4T)

와 같이 파티션을 구성한 후

```
mdadm --create /dev/md0 --level=5 --raid-devices=4 /dev/sd[abcd]1
mdadm --create /dev/md1 --level=1 --raid-devices=2 /dev/sd[cd]2
```

10T 짜리 raid5 장치와, 4T 짜리 raid1 장치를 만든다. 만약 14T가 3개 였다면 raid1이 아닌 raid5로 /dev/md1 를 생성하면 된다.

```
pvcreate /dev/md0
pvcreate /dev/md1
vgcreate shr-vg /dev/md0 /dev/md1
# 최대 용량으로 생성
lvcreate -L+100%FREE -n shr-lv shr-vg
# ext4 파티션을 생성한다
mkfs.ext4 /dev/shr-vg/shr-lv
```

두개 레이드 장치를 pvcreate로 초기화한 후, 하나의 volume group에 추가하여 하나의 lv 를 만들어 사용하면 SHR와 같은 구성이 된다.
