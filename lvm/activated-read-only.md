# activated read only 오류

## 오류 증상 확인

thinpool이 (activated read only) 상태로 표시되며, write가 불가한 상태

```
root@pve:~# lvs
  LV                  VG     Attr       LSize   Pool          Origin Data%  Meta%
  raid_thinpool       hdd_vg twi-cotzM-  21.00t                      47.70  0.37                            
  vm-121-disk-0       hdd_vg Vwi-aotz--  20.00t raid_thinpool        50.09                                  
  data                pve    twi-aotz--   1.00t                      28.62  1.64                            
  root                pve    -wi-ao----  96.00g                                                             
  swap                pve    -wi-ao----   8.00g                                                             
  vm-121-disk-0       pve    Vwi-aotz--  20.00g data                 43.96                                  
  vm-122-disk-0       pve    Vwi-aotz--  32.00g data                 100.00                                 
```

thinpool의 attr 정보에 c (under construction), M (mirrored without initial sync) 값이 확인된다.

```
root@pve:~# lvdisplay
--- Logical volume ---
  LV Name                raid_thinpool
  VG Name                hdd_vg
  LV UUID                0Vlh3d-E5ng-o7ik-niOj-SgEf-gYu4-XwrSwp
  LV Write Access        read/write (activated read only)
  LV Creation host, time pve, 2021-12-06 21:10:56 +0900
  LV Pool metadata       raid_thinpool_tmeta
  LV Pool data           raid_thinpool_tdata
  LV Status              available
  # open                 0
  LV Size                21.00 TiB
  Allocated pool data    50.00%
  Allocated metadata     0.38%
  Current LE             5505024
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     32768
  Block device           253:15
```

thinpool 의 상태가 read/write 가 가능하지만 (activated read only) 상태가 추가되어 있음.

---
### lvs 의 결과에서 thinpool의 Meta% 사용도가 100%에 가깝다면 메타데이터 크기를 키워서 해결할 수 있다.

단 메타데이터 사이즈는 약 15.8G 를 초과할 수 없다. 증가 시키기전에 현재 사이즈를 확인해야 한다.
```
root@pve:~# lvs -a
  LV                    VG     Attr       LSize   Pool          Origin Data%  Meta%
  raid_thinpool         hdd_vg twi-cotzM-  21.00t                      47.70  100
  [raid_thinpool_tdata] hdd_vg Twi-ao----  21.00t                                                             
  [raid_thinpool_tmeta] hdd_vg ewi-ao----  15.50g                                  
```

raid_thinpool_tmeta 크기가 15.5g 이기 때문에 크기를 키울 수 없지만, 보통 더 작은 크기이며 아래 명령어를 키울 수 있다.

```
lvextend --poolmetadatasize +2G hdd_vg/raid_thinpool
```

실수로 메타데이터 크기를 16g 이상 키운 경우 thinpool 을 사용할 수 없게 된다.
신규 메타데이터 볼륨을 만들고 교환하여 해결할 수 있다.

실행 전 해당 thinpool의 볼륨을 활용하는 모든 vm 을 중지하고, pve 화면의 스토리지에서도 비활성화해야 한다.

```
# 기존 메타데이터의 블록매핑정보를 저장한다.
thin_dump /dev/mapper/hdd_vg-raid_thinpool_tmeta > meta  
# 원하는 크기의 교체용 메타데이터를 생성한다.
lvcreate -n raid_thinpool_meta1 -L 15G hdd_vg
# 신규 메타데이터 볼륨에 매핑 정보를 복원한다.
thin_restore -i meta -o /dev/mapper/hdd_vg-raid_thinpool_meta1
# 신규 메타데이터 볼륨으로 교체한다. (기존 메타데이터와 신규 볼륨이 서로 교체된다.)
lvconvert --thinpool hdd_vg/raid_thinpool --poolmetadata hdd_vg/raid_thinpool_meta1
# thinpool 을 활성화 한다.
lvchange -ay hdd_vg/raid_thinpool
# 이상이 없다면 교체된 이전 메타데이터 볼륨을 삭제한다.
lvremove hdd_vg/raid_thinpool_meta1
```

---
### lvconvert --repair 통한 복구 시도

LVM 기본 복구 기능을 통해 복구를 시도한다.

```
lvconvert --repair hdd_vg/raid_thinpool
```

실행 후에도 정상화 되지 않고 <code>lvs -a</code> 의 결과에서 tmeta 볼륨이 16G 이상으로 증가 되었다면 상단의 thin_dump~lvremove 명령어를 통해 16G 이하 볼륨을 생성하여 교체해준다.

---
### 오류 플래그 강제 수정하기

이 방법은 더 이상 시도해 볼 방법이 검색되지 않아 시도한 방법으로, 데이터 손실의 위험을 감수하고 수행해야 한다.

상단의 thin_dump 를 수행하여 생성된 meta 파일을 직접 수정 후 복원하여 정상 인식을 기대해 본다.

```
<superblock uuid="" time="0" transaction="1" flags="1" version="2" data_block_size="16384" nr_data_blocks="2621440">
```

flags="1" 로 되어있다면 1을 0으로 수정 후에 저장한다. (오류 플래그 정보로 보임)

그 후 lvcreate, thin_restore ~~ lvremove 까지 나머지 명령어를 수행해 준다.

