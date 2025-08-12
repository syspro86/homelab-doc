`lsblk` 통해 기존 파티션 확인

lvm 으로 구성되어있었다면

```
vgscan
vgchange -ay
ls /dev/mapper/  # 장치가 생겼는지 확인
```

디스크에 설치된 파티션 마운트
```
mount /dev/sda1 /mnt
mount /dev/mapper/vg-lv /mnt   # LVM 의 경우

mount /dev/sda2 /mnt/boot   # grub 설정을 수정해야 하면
mount /dev/sda3 /mnt/boot/efi   # EFI 파티션이 있는 경우
```

디스크 파티션으로 전환
```
mount --bind /dev /mnt/dev
mount --bind /proc /dev/proc
mount --bind /sys /dev/sys
chroot /mnt
```

작업 완료 후
```
exit
umount -R /mnt
reboot
```
