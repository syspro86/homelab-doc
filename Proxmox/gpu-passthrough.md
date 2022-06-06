# GPU Passthrough

https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/

## PVE 에서 설정

/etc/default/grub 파일 수정
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt pcie_acs_override=downstream,multifunction nofb video=efifb:off vga=off"
```

grub 업데이트
```
update-grub
```

vfio 를 읽어오도록 /etc/modules 파일 수정

```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

host 에서 gpu를 인식하지 않도록 blacklist 등록

```
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
```

vfio 정보를 등록

```
lspci -v # 결과에서 GPU 이름앞 01:00 형태 숫자를 찾는다.
lspci -n -s 01:00 # 결과에서 aaaa:bbbb 형태 값을 찾는다.

# ids 부분을 위 명령 결과값으로 바꾼다.
echo "options vfio-pci ids=10de:1b81,10de:10f0 disable_vga=1"> /etc/modprobe.d/vfio.conf
```

initramfs 를 업데이트하고 재부팅
```
update-initramfs -u
reset
```

이후 GPU 를 사용할 VM 설정
- machine 유형을 q35로 설정
- Hardware - Add - PCI Device - GPU 선택
  - All Function: 체크
  - Primary GPU: X
  - Rom-BAR: 체크
  - PCI-Express: 체크

## 오류 해결

### vfio-pci 0000:08:00.0: BAR 3: can't reserve [mem 0xf0000000-0xf1ffffff 64bit pref]

장치를 제거한 후 다시 검색하여 추가한다.

```
echo 1 > /sys/bus/pci/devices/0000\:08\:00.0/remove
echo 1 > /sys/bus/pci/rescan
```

필요 시 crontab에 @reboot 로 추가하여 실행.
