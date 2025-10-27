# siano_smsdvb_dkms
This repository contains a copy of the siano smsdvb driver from the Linux kernel and with DKMS configuration.

# about
siano smsdvbドライバにdkms.confを付属させたものです。

linux kernel 6.8.12-15-pveにて以下のような大量のtraceが出るため、問題のコードを落としてビルドするために作成しました。

```log
kernel: [253712.301329] ------------[ cut here ]------------
kernel: [253712.302351] invalid sysfs_emit_at: buf:00000000665237b4 at:0
kernel: [253712.303367] WARNING: CPU: 7 PID: 0 at fs/sysfs/file.c:777 sysfs_emit_at+0x64/0xd0
kernel: [253712.304405] Modules linked in: ...
kernel: [253712.314151] CPU: 7 PID: 0 Comm: swapper/7 Tainted: P        W  O       6.8.12-15-pve #1
kernel: [253712.315254] Hardware name: MSI MS-7971/H170A PC MATE (MS-7971), BIOS B.D0 12/11/2017
kernel: [253712.316326] RIP: 0010:sysfs_emit_at+0x64/0xd0
kernel: [253712.317396] Code: 00 00 00 48 c7 44 24 10 00 00 00 00 48 85 ff 74 08 f7 c7 ff 0f 00 00 74 39 89 f2 48 89 fe 48 c7 c7 40 ff dc 8b e8 6c 63 b7 ff <0f> 0b 31 c0 48 8b 54 24 18 65 48 2b 14 25 28 00 00 00 75 4e c9 31
kernel: [253712.318467] RSP: 0018:ffffb0ffc0298d60 EFLAGS: 00010246
kernel: [253712.319544] RAX: 0000000000000000 RBX: 0000000000000000 RCX: 0000000000000000
kernel: [253712.320627] RDX: 0000000000000000 RSI: 0000000000000000 RDI: 0000000000000000
kernel: [253712.321701] RBP: ffffb0ffc0298db0 R08: 0000000000000000 R09: 0000000000000000
kernel: [253712.322777] R10: 0000000000000000 R11: 0000000000000000 R12: ffff975a6035400c
kernel: [253712.323807] R13: ffff975a4149e008 R14: ffff975a4149e000 R15: ffff975a4149e004
kernel: [253712.324821] FS:  0000000000000000(0000) GS:ffff97618ed80000(0000) knlGS:0000000000000000
kernel: [253712.325833] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
kernel: [253712.326866] CR2: 00007eeef8d103d8 CR3: 000000050d236005 CR4: 00000000003706f0
kernel: [253712.327897] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
kernel: [253712.328887] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
kernel: [253712.329874] Call Trace:
kernel: [253712.330890]  <IRQ>
kernel: [253712.331878]  ? show_regs+0x6d/0x80
kernel: [253712.332864]  ? __warn+0x89/0x160
kernel: [253712.333847]  ? sysfs_emit_at+0x64/0xd0
kernel: [253712.334854]  ? report_bug+0x17e/0x1b0
kernel: [253712.335851]  ? irq_work_queue+0x2f/0x70
kernel: [253712.336857]  ? handle_bug+0x6e/0xb0
kernel: [253712.337868]  ? exc_invalid_op+0x18/0x80
kernel: [253712.338867]  ? asm_exc_invalid_op+0x1b/0x20
kernel: [253712.339861]  ? sysfs_emit_at+0x64/0xd0
kernel: [253712.340863]  smsdvb_print_isdb_stats_ex+0x80/0x3e0 [smsdvb]
kernel: [253712.341893]  smsdvb_onresponse+0x144/0xc50 [smsdvb]
kernel: [253712.342900]  smscore_onresponse+0x8d/0x4e0 [smsmdtv]
kernel: [253712.343886]  smsusb_onresponse+0x12d/0x230 [smsusb]
kernel: [253712.344888]  __usb_hcd_giveback_urb+0xa9/0x130
kernel: [253712.345890]  usb_giveback_urb_bh+0xa8/0x140
kernel: [253712.346876]  tasklet_action_common.constprop.0+0xd7/0x1e0
kernel: [253712.347889]  tasklet_action+0x22/0x30
kernel: [253712.348867]  handle_softirqs+0xd5/0x300
kernel: [253712.349850]  __irq_exit_rcu+0xd9/0x100
kernel: [253712.350887]  irq_exit_rcu+0xe/0x20
kernel: [253712.351898]  common_interrupt+0xa4/0xb0
kernel: [253712.352931]  </IRQ>
kernel: [253712.353911]  <TASK>
kernel: [253712.354886]  asm_common_interrupt+0x27/0x40
kernel: [253712.355872] RIP: 0010:cpuidle_enter_state+0xce/0x470
kernel: [253712.356855] Code: aa 01 ff e8 f4 ef ff ff 8b 53 04 49 89 c6 0f 1f 44 00 00 31 ff e8 02 93 00 ff 80 7d d7 00 0f 85 e7 01 00 00 fb 0f 1f 44 00 00 <45> 85 ff 0f 88 83 01 00 00 49 63 d7 4c 89 f1 48 8d 04 52 48 8d 04
kernel: [253712.357899] RSP: 0018:ffffb0ffc0123e50 EFLAGS: 00000246
kernel: [253712.358954] RAX: 0000000000000000 RBX: ffffd0ffbfd9eb00 RCX: 0000000000000000
kernel: [253712.359987] RDX: 0000000000000007 RSI: 0000000000000000 RDI: 0000000000000000
kernel: [253712.361022] RBP: ffffb0ffc0123e88 R08: 0000000000000000 R09: 0000000000000000
kernel: [253712.362084] R10: 0000000000000000 R11: 0000000000000000 R12: 0000000000000006
kernel: [253712.363149] R13: ffffffff8c870320 R14: 0000e6bfe38edaa4 R15: 0000000000000006
kernel: [253712.364152]  cpuidle_enter+0x2e/0x50
kernel: [253712.365164]  call_cpuidle+0x23/0x60
kernel: [253712.366183]  do_idle+0x207/0x260
kernel: [253712.367158]  cpu_startup_entry+0x2a/0x30
kernel: [253712.368125]  start_secondary+0x119/0x140
kernel: [253712.369103]  secondary_startup_64_no_verify+0x184/0x18b
kernel: [253712.370097]  </TASK>
kernel: [253712.371085] ---[ end trace 0000000000000000 ]---
```
https://x.com/GenericRead/status/1982216755440177194

暁雲小僧 様のサイトを参考に、CONFIG_SMS_SIANO_DEBUGFS=y だとしても prt_isdb_stats_exが出ないようになっています。  
https://www.gyoun.net/blog/solve_tunner_problem/

Linux Kernelからコピーしています。ライセンスはコピー元の通りGPLv2です。

かなりやっつけパッチなのでPRお待ちしています。

# usage
**利用は自己責任でお願いします。Kernel Driverにつき事故時の影響範囲は広範にわたります。**

野良カーネルモジュールの導入はセキュリティ的にどうなのか、という方はupstreamブランチとの差分を見るという用途でお使いください。

## install
```sh
git clone https://github.com/RGBA-CRT/siano_smsdvb_dkms.git
sudo cp -r siano_smsdvb_dkms /usr/src/siano_smsdvb_dkms-1.0
sudo dkms add siano_smsdvb_dkms/1.0
sudo dkms build siano_smsdvb_dkms/1.0
sudo dkms install siano_smsdvb_dkms/1.0
```

## uninstall
```sh
sudo dkms uninstall siano_smsdvb_dkms/1.0
sudo dkms remove siano_smsdvb_dkms/1.0
sudo rm -r /usr/src/siano_smsdvb_dkms-1.0
```

# tested on
- "Debian GNU/Linux 12 (bookworm) / kernel 6.8.12-15-pve / x86_64

# reference
以下参考にさせていただきました。ありがとうございます。
- https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/drivers/media/common/siano?h=v6.8
- https://www.gyoun.net/blog/solve_tunner_problem/
- https://gihyo.jp/admin/serial/01/ubuntu-recipe/0791

