
## subvolume

```
btrfs subvolume create minio
```

snapshot
```
btrfs subvolume snapshot -r minio  minio-snapshot
```

remove
```
btrfs subvolume delete minio-snapshot
btrfs subvolume delete minio
```
