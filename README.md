# mpen

ペンがタッチとしてしか OS に届かない機種向けの、指タッチ自動無視番人。
例: MateBook E + M-Pencil。

## 向く機種

- ペン先が画面のタッチとして来る (`libinput list-devices` でペンが `cap:t`)
- 近づき信号も筆圧も OS に来ない
- 指タッチとペンが別の入力機器として見える

普通の Wacom 等 (近づき信号あり) は OS 側で勝手に無視するから要らないよ。

## 入れ方

```bash
sudo pacman -Sy python-evdev
sudo usermod -aG input $USER
# 入り直す (グループ反映のため)
mkdir -p ~/.local/bin ~/.config/systemd/user
cp mpen ~/.local/bin/mpen
chmod +x ~/.local/bin/mpen
cp mpen.service ~/.config/systemd/user/
systemctl --user enable --now mpen
```

## 使い方

ペンが画面に触れている間だけ指タッチが止まるよ。ペンを離すと戻るよ。
記録は `journalctl --user -u mpen` で見られるよ。

機器名が違う時は引数で変えるよ。名前はこう調べるよ

```bash
mpen --list
mpen --pen "Stylus" --touch "12d1:10b8" --touch-exclude "Stylus"
```

決まったら service の ExecStart に足すよ

```
ExecStart=%h/.local/bin/mpen --pen "Stylus" --touch "12d1:10b8"
```

## やめ方

```bash
systemctl --user disable --now mpen
```

## 注意

- Wayland でも X11 でも動くよ (表示の仕組みによらないから)
- ペンのボタン等 Bluetooth 側は別扱いだよ
- 同時に2本指で触る取り回しには対応してないよ
