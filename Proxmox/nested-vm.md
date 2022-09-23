# Nested VM

VM안에서 가상화 기능 사용

옵션 확인

```
cat /sys/module/kvm_intel/parameters/nested<br></br>cat /sys/module/kvm_amd/parameters/nested
```

1 이나 Y 이면 활성화 됨

```
# intel
echo "options kvm-intel nested=Y" > /etc/modprobe.d/kvm-intel.conf

# amd
echo "options kvm-amd nested=1" > /etc/modprobe.d/kvm-amd.conf
```

vm 의 cpu 타입을 host 로 변경 후

/etc/pve/qemu-server/xxx.conf 내용 추가

```
args: -cpu host,-hypervisor,-mpx
```

https://pve.proxmox.com/wiki/Nested\_Virtualization

https://forum.proxmox.com/threads/hyper-v-in-windows-guest-not-working-for-wsl2.87791/
