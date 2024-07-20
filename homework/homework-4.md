# 作业4：为e1000网卡驱动添加remove代码

TODO 尚未完成

加载模块

```bash
insmod r4l_e1000_demo.ko
```


```
[   42.565126] r4l_e1000_demo: loading out-of-tree module taints kernel.
[   42.571077] r4l_e1000_demo: Rust for linux e1000 driver demo (init)
[   42.571600] r4l_e1000_demo: Rust for linux e1000 driver demo (probe): None
[   42.718546] ACPI: \_SB_.LNKC: Enabled at IRQ 11
[   42.740231] r4l_e1000_demo: Rust for linux e1000 driver demo (net device get_stats64)
[   42.742179] insmod (80) used greatest stack depth: 11144 bytes left
```


卸载


```bash
rmmod r4l_e1000_demo.ko
```

```
[   83.427176] r4l_e1000_demo: Rust for linux e1000 driver demo (exit)
[   83.427645] r4l_e1000_demo: Rust for linux e1000 driver demo (remove)
[   83.427831] r4l_e1000_demo: Rust for linux e1000 driver demo (device_remove)
[   83.428010] r4l_e1000_demo: Rust for linux e1000 driver demo (device_remove)
[   83.430603] r4l_e1000_demo: Rust for linux e1000 driver demo (net device get_stats64)
```

再次加载

```
[  106.193959] r4l_e1000_demo: Rust for linux e1000 driver demo (init)
[  106.194393] r4l_e1000_demo: Rust for linux e1000 driver demo (probe): None
[  106.194764] r4l_e1000_demo 0000:00:03.0: BAR 0: can't reserve [mem 0xfebc0000-0xfebdffff]
[  106.195096] r4l_e1000_demo: probe of 0000:00:03.0 failed with error -16
```

报错了，


猜测是 `probe` 方法申请的资源没有释放

```rust
dev.request_selected_regions(bars, c_str!("e1000 reserved memory"))?;
```


