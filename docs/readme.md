# require

## 構成
- OS: ubuntu 24.10 # GUI付き
- machine: lll-legacy

## ファイル
- autoinstall.yml
  - パスワードを置き換える
  - リポジトリを適応させる
- dm.key
  - `/`に設置する
- fstab
  - `/etc/fstab`に付け加える
- crypttab
  - `/etc/crypttab`に付け加える

# 手順

1. `https://raw.githubusercontent.com/flll/ubuntu-install/refs/heads/main/autoinstall.yml`
  - ![QR_864521](https://github.com/user-attachments/assets/194d4e40-e340-41b1-9ca5-df9f15676950)
1. インストール完了する
1. dmキーを`password`から変更する
1. `sudo tailscale up --accept-risk all --ssh --advertise-routes=10.0.0.0/24 --accept-routes --advertise-exit-node` をする


# ディスク構成
```
物理ディスク (WWN: 0x5002538d700a5342)
┌─────────────────────────────────────────┐
│ GPTパーティションテーブル                 │
├──────────┬──────────┬───────────────────┤
│ EFI      │ /boot    │ 暗号化パーティション│
│ (1GB)    │ (2GB)    │ (108.7GB)         │
│ FAT32    │ ext4     │                   │
└──────────┴──────────┴───────────────────┘
                        │
                        ▼
                    dm-crypt
                        │
                        ▼
                    LVM PV
                        │
                        ▼
                    VG: ubuntu-vg
                        │
                        ▼
                    LV: ubuntu-lv
                    (108.7GB)
                    ext4

マウントポイント:
/boot/efi ← EFIパーティション (FAT32)
/boot     ← Bootパーティション (ext4)
/         ← LV: ubuntu-lv (ext4)

スワップ:
/swap.img (8GB)
```
