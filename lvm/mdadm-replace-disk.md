# mdadm disk 교체

### disk 제거

```
root@pve:~# mdadm --manage /dev/md0 --fail /dev/sda
mdadm: set /dev/sda faulty in /dev/md0

root@pve:~# cat /proc/mdstat
Personalities : [raid6] [raid5] [raid4] [raid0] 
md0 : active raid5 sdd[4] sdc[2] sdb[1] sda[0](F)
      29298909696 blocks super 1.2 level 5, 512k chunk, algorithm 2 [4/3] [_UUU]
      bitmap: 0/73 pages [0KB], 65536KB chunk
unused devices: <none>

root@pve:~# mdadm --manage /dev/md0 --remove /dev/sda
mdadm: hot removed /dev/sda from /dev/md0

root@pve:~# cat /proc/mdstat
Personalities : [raid6] [raid5] [raid4] [raid0] 
md0 : active raid5 sdd[4] sdc[2] sdb[0]
      29298909696 blocks super 1.2 level 5, 512k chunk, algorithm 2 [4/3] [_UUU]
      bitmap: 2/73 pages [8KB], 65536KB chunk
unused devices: <none>
```

### 물리 디스크 교체

sda 디스크를 제거하고 새 디스크를 추가한다. 신규 디스크 -> /dev/sda

### disk 추가

```
root@pve:~# mdadm --manage /dev/md0 --add /dev/sda
mdadm: added /dev/sda

root@pve:~# cat /proc/mdstat
Personalities : [raid6] [raid5] [raid4] [raid0] 
md0 : active raid5 sda[5] sdd[4] sdc[2] sdb[1]
      29298909696 blocks super 1.2 level 5, 512k chunk, algorithm 2 [4/3] [_UUU]
      [>....................]  recovery =  0.0% (78080/9766303232) finish=2084.2min speed=78080K/sec
      bitmap: 4/73 pages [16KB], 65536KB chunk

unused devices: <none>

root@pve:~# mdadm --detail /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Wed Dec  8 12:24:16 2021
        Raid Level : raid5
        Array Size : ...
     Used Dev Size : ...
      Raid Devices : 4
     Total Devices : 4
       Persistence : Superblock is persistent

     Intent Bitmap : Internal

       Update Time : Thu Apr  7 19:25:08 2022
             State : clean, degraded, recovering 
    Active Devices : 3
   Working Devices : 4
    Failed Devices : 0
     Spare Devices : 1

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : bitmap

    Rebuild Status : 0% complete

              Name : pve:0  (local to host pve)
              UUID : 8afb359a:d644ce21:fff8276e:c6203ec8
            Events : 81433

    Number   Major   Minor   RaidDevice State
       5       8        1        0      spare rebuilding   /dev/sda
       1       8       17        1      active sync   /dev/sdb
       2       8       33        2      active sync   /dev/sdc
       4       8       49        3      active sync   /dev/sdd
```
