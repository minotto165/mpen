# mpen

ペンがタッチとしてしか OS に届かない機種で、ペン使用中に指タッチを無効にします。
例: MateBook E + M-Pencil。

## 条件

- `libinput list-devices` でペンが `cap:t` と出る
- 近づき信号も筆圧も OS に届かない
- 指タッチとペンが別の機器として見える

近づき信号があるペンは OS 側で無視されるため不要です。

## インストール

```bash
sudo pacman -Sy python-evdev
sudo usermod -aG input $USER
# 一度入り直します
mkdir -p ~/.local/bin ~/.config/systemd/user
cp mpen ~/.local/bin && chmod +x ~/.local/bin/mpen
cp mpen.service ~/.config/systemd/user/
systemctl --user enable --now mpen
```

## 使い方

ペンが画面に触れている間だけ指タッチが無効になります。
記録は `journalctl --user -u mpen` で確認できます。
機器名が異なる場合は引数で指定し、service の ExecStart に追記します。

```bash
mpen --list
mpen --pen "Stylus" --touch "12d1:10b8" --touch-exclude "Stylus"
```

## 削除

```bash
systemctl --user disable --now mpen
```

## 注意

- Wayland と X11 のどちらでも動作します
- ペンのボタンなど Bluetooth 側は対象外です
- 2本指での同時操作には対応していません
