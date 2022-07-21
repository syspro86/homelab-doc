# LVM

## 오류 해결

- [Thin Pool metadata read only 오류](metadata-read-only.md)

## RAID

- [mdadm](mdadm.md)

- [disk 교체하기](mdadm-replace-disk.md)

# WD HDD

## TLER

TLER 활성화

```
smartctl -l scterc,70,70 -d sat /dev/sda
```

TLER 활성화 확인

```
smartctl -l scterc /dev/sda
```
