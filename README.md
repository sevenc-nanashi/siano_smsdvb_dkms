# siano_smsdvb_dkms
This repository contains a copy of the siano smsdvb driver for the Linux kernel, with DKMS configuration.

# about
siano smsdvbドライバにdkms.confを付属させたものです。
linux kernel 6.8.12-15-pveにて大量のtraceが出るため、当該コードを落としてビルドするために作成しました。
https://x.com/GenericRead/status/1982216755440177194

CONFIG_SMS_SIANO_DEBUGFS=y だとしても prt_isdb_stats_exが出ないようになっています。

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