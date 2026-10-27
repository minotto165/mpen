# mpen

ペンがタッチとしてしか OS に届かない機種で、指タッチを自動で無効にするツールです。
例: MateBook E + M-Pencil。

## 対応機種

- ペン先が画面のタッチとして認識される (`libinput list-devices` でペンが `cap:t` と出ます)
- 近づき信号も筆圧も OS に届かない
- 指タッチとペンが別の入力機器として見える

近づき信号がある通常の Wacom ペンなどは OS 側で自動で無視されるため、このツールは不要です。

## インストール

```bash
sudo pacman -Sy python-evdev
sudo usermod -aG input $USER
# 一度入り直します (グループの反映のため)
mkdir -p ~/.local/bin ~/.config/systemd/user
cp mpen ~/.local/bin/mpen
chmod +x ~/.local/bin/mpen
cp mpen.service ~/.config/systemd/user/
systemctl --user enable --now mpen
```

## 使い方

ペンが画面に触れている間、指タッチが無効になります。ペンを離すと元に戻ります。
動作の記録は `journalctl --user -u mpen` で確認できます。

機器名が異なる場合は引数で指定します。名前は次のコマンドで調べられます。

```bash
mpen --list
mpen --pen "Stylus" --touch "12d1:10b8" --touch-exclude "Stylus"
```

引数が決まったら service ファイルの ExecStart に追記します。

```
ExecStart=%h/.local/bin/mpen --pen "Stylus" --touch "12d1:10b8"
```

## 削除

```bash
systemctl --user disable --now mpen
```

## 注意

- Wayland と X11 のどちらでも動作します (表示の仕組みに依存しないため)
- ペンのボタンなど Bluetooth 側の機能は対象外です
- 2本指での同時操作には対応していません
