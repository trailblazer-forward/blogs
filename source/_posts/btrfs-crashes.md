---
title: btrfs-crashes
date: 2026-10-18 21:48:28
tags: technology Life
---

# Btrfs Crashes

事发突然，我的 Btrfs 文件系统炸了。

## 环境

- OS: Arch Linux & Windows 双系统
- 硬盘分区：1x FAT32 boot 分区，1x SWAP 分区，1x btrfs 分区，其中下分若干子卷。

## 事发前

冻结 Arch Linux，重启后切换至 Windows，后又切换至 Arch Linux。此时需要写入磁盘的命令/应用程序均报错 read-only filesystem。重启后无法挂载 btrfs 分区，boot 和 SWAP 分区一切正常。

## 修复尝试

启动至 Arch Linux Live CD 环境，尝试挂载 btrfs 分区，报错。dmesg 日志：

```
[  150.144708] BTRFS info (device nvme0n1p3): first mount of filesystem fbf38717-3cb4-46a0-825c-acb254ae08c9
[  150.144713] BTRFS info (device nvme0n1p3): using crc32c checksum algorithm
[  150.150767] BTRFS info (device nvme0n1p3): start tree-log replay
[  150.162268] BTRFS error (device nvme0n1p3): bad tree block start, mirror 1 want 382872322048 have 0
[  150.163146] BTRFS error (device nvme0n1p3): bad tree block start, mirror 2 want 382872322048 have 0
[  150.163829] BTRFS error (device nvme0n1p3 state A): Transaction 63524 aborted (error -5)
[  150.164459] BTRFS: error (device nvme0n1p3 state A) in process_one_buffer:445: errno=-5 IO failure
[  150.165068] BTRFS: error (device nvme0n1p3 state EA) in btrfs_recover_log_trees:7807: errno=-5 IO failure
[  150.165670] BTRFS error (device nvme0n1p3 state EA): failed to recover log trees with error: -5
[  150.166880] BTRFS error (device nvme0n1p3 state EA): open_ctree failed: -5
```

这里由于缺乏经验，我没有尝试使用 `mount -o ro,rescue=all` 尝试进行挂载。不过因为重要的数据基本都在云端和 Windows 上留有备份了，所以数据并不是很重要。就当一次修复 btrfs 的尝试了吧。

由于无法挂载，尝试进行离线的 `btrfs check` 检查：

```
checksum verify failed on 383353208832 wanted 0x00000000 found 0xb6bde3e4
checksum verify failed on 383353208832 wanted 0x00000000 found 0xb6bde3e4
checksum verify failed on 383353208832 wanted 0x00000000 found 0xb6bde3e4
bad tree block 383353208832, bytenr mismatch, want=383353208832, have=0
ERROR: cannot open file system
```

一样，报错 bad tree block。

---

尝试 `btrfs rescue zero-log`：

```
checksum verify failed on 383353208832 wanted 0x00000000 found 0xb6bde3e4
checksum verify failed on 383353208832 wanted 0x00000000 found 0xb6bde3e4
ERROR: could not open ctree
```

Again...

---

尝试 `btrfs rescue chunk-recover`：

```
All Devices:
	Device: id = 1, name = /dev/nvme0n1p3

Scanning: 1012396969984 in dev0                
Scanning: DONE in dev0
DEVICE SCAN RESULT:
Filesystem Information:
	sectorsize: 4096
	nodesize: 16384
	tree root generation: 63523
	chunk root generation: 63324

All Devices:
	Device: id = 1, name = /dev/nvme0n1p3

All Block Groups:

All Chunks:
	Chunk: start = 13631488, len = 8388608, type = 1, num_stripes = 1
	   Stripes list:
	   [ 0] Stripe: devid = 1, offset = 13631488
...
  Chunk: start = 473584107520, len = 1073741824, type = 1, num_stripes = 1
      Stripes list:
      [ 0] Stripe: devid = 1, offset = 277064187904
      No block group.
      No device extent.

Total Chunks:		259
  Recoverable:		120
  Unrecoverable:	139

Orphan Block Groups:

Orphan Device Extents:

Chunk tree recovery failed
```

---

由于对 btrfs 的了解并不深入，我想不到有其他的修复方案了。看着 bad tree block 之类的报错一头雾水，不明白具体的故障是什么，还是学的太少了。这次就当是一个教训吧。

听说过 btrfs 不是很稳定，不过没想到回落到我自己头上。

事到如今，重装吧。
