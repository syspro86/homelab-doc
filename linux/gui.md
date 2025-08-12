sleep 모드로 가는걸 막는 서비스 목록

```
systemd-inhibit --list --mode=block
```

vram 할당 용량 확인

```
glxinfo | grep -E -i "device|memory"
    Device: AMD Radeon Graphics (radeonsi, renoir, ACO, DRM 3.61, 6.14.0-27-generic) (0x15e7)
    Video memory: 512MB
...
Memory info (GL_NVX_gpu_memory_info):
    Dedicated video memory: 512 MB
```
