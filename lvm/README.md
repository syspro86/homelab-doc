# LVM

## 오류 해결

- [Thin Pool activated read only 오류](activated-read-only.md)


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
