# mpen

Disables finger touch while the pen is in use, for devices where the pen only reaches the OS as touch.
Example: MateBook E + M-Pencil.

## Requirements

- The pen shows up as `cap:t` in `libinput list-devices`
- No proximity or pressure events reach the OS
- Finger touch and pen appear as separate input devices

Pens with proximity are already ignored by the OS, so this tool is not needed for them.

## Install

```bash
sudo pacman -Sy python-evdev
sudo usermod -aG input $USER
# Log back in (to apply the group)
mkdir -p ~/.local/bin ~/.config/systemd/user
cp mpen ~/.local/bin && chmod +x ~/.local/bin/mpen
cp mpen.service ~/.config/systemd/user/
systemctl --user enable --now mpen
```

## Usage

Finger touch is disabled only while the pen touches the screen.
Check the log with `journalctl --user -u mpen`.
For different device names, pass arguments and append them to ExecStart in the service file.

```bash
mpen --list
mpen --pen "Stylus" --touch "12d1:10b8" --touch-exclude "Stylus"
```

## Removal

```bash
systemctl --user disable --now mpen
```

## Notes

- Works on both Wayland and X11
- Pen buttons and other Bluetooth features are out of scope
- Two-finger gestures while the pen is down are not supported
