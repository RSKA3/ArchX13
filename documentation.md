# Config for my Thinkpad X13 Gen 1 running Arch Linux 

## Filesystem
Type: btrfs

Compression: zstd:1 

Additional flags: noatime

Structure:

    /boot
    /               @
    /home           @home
    /var/log        @var_logs
    /var/cache      @var_cache
    /.snapshots     @snapshots

Root directory is encrypted with LUKS2 using cryptsetup.

systemd-cryptenroll --wipe-slot=tpm2 --tpm2-device=auto \
  --tpm2-pcrs=7+15:sha256=0000000000000000000000000000000000000000000000000000000000000000 \
  /dev/encrypted-device

## RAM

Type: ZRAM

Amount: 8 GiB which equals half of my RAM.

## Security

TPM2 and Secure boot are both enabled, the UEFI settings are also password locked.

The key is signed with the Microsoft key.



## Bootloader & Snapshots

GRUB2 is used as the bootloader.

Snapshots are configured with Snapper and snap-pac for the root directory.


## Programs

### Ly

Login manager.

Source: Extra ly

### Niri

Scrolling compositor.

Source: Extra niri

"~/.config/niri/config.kdl"


### Noctalia

Desktop shell.

Source: Extra noctalia 

"~/.config/noctalia/settings.toml"

"~/.local/state/noctalia/settings.toml"

### Helium Browser

Web browser.

Source: AUR helium-browser-bin

NOTE: To enable DRM for helium browser you need to:
    
    1. Download the latest [Chrome.deb package](https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb)
    2. Extract it: "ar x google-chrome-stable_current_amd64.deb"
    3. Extract the tar "tar -xf data.tar.xz -C extracted"
    4. Find the Widevine version: "cat extracted/opt/google/chrome/WidevineCdm/manifest.json | grep version"
    5. Make the directory: "mkdir -p ~/.config/net.imput.helium/WidevineCdm/<Widevine version>"
    6. Move the contents of WidevineCdm to the directory: "mv extracted/opt/google/chrome/WidevineCdm ~/.config/net.imput.helium/WidevineCdm/<Widevine version>"

### Ghostty

Terminal emulator.

"~/.config/ghostty/config.ghostty"

### xremap

Key remapper.

Source: AUR xremap-niri-bin

"~/.config/xremap/config.yml"

I have set up xremap to output tab switch keybinds for helium browser due to the fact that neither Firefox nor Chromium browsers allow me to set non-latin keys (like ctrl+ö) to switch tabs.

To configure xremap it requires the user to:

    1. add themselves to the input group: sudo gpasswd -a YOUR_USER input
    2. Give members of the input group permission to make output devices: echo 'KERNEL=="uinput", GROUP="input", TAG+="uaccess", MODE:="0660", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-input.rules
    3. Reboot

### localsend

Source: AUR localsend-bin

To configure localsend with firewalld it requires that you enable the port 53317.

	1. sudo firewall-cmd --permanent --add-port=53317/tcp
	2. sudo firewall-cmd --permanent --add-port=53317/udp
	3. sudo firewall-cmd --reload
