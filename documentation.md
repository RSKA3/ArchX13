# ArchX13
Documentation for my Thinkpad X13 Gen 1 running Arch Linux.

**Table of Contents**
- [Technical information](#technical-information)
  - [Filesystem](#filesystem)

## Technical information

### Hardware

* CPU: i7-10710U @ 1.10GHz
* RAM: 16 GB (2x 8 GB Micron DDR4 SODIMM @ 2667 MHz / rated 3200 MHz)
* MEMORY: 256GB NVMe (Intel SSDPEKKF256G8L)

### Filesystem
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

### RAM

Type: ZRAM

Amount: 8 GiB which equals half of my RAM.

### Security

TPM2 and Secure boot are both enabled, the UEFI settings are also password locked.

The key is signed with the Microsoft key.



### Bootloader & Snapshots

GRUB2 is used as the bootloader.

Snapshots are configured with Snapper and snap-pac for the root directory.



## Programs

### System

#### Ly
Login manager.

**Source:** `Extra/ly`

---

#### Niri
Scrolling compositor.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/niri` |
| **Config Path** | `~/.config/niri/config.kdl` |

---

#### Noctalia
Desktop shell.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/noctalia` |
| **Config Paths** | `~/.config/noctalia/settings.toml`<br>`~/.local/state/noctalia/settings.toml` |

---

### Default GUIs

#### Nautilus

File manager

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/nautilus` |

---

#### Loupe

Image viewer

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/loupe` |

---


#### Papers

Document viewer.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/papers` |

---

#### Foliate

Ebook reader.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/foliate` |

---

#### Logs

System event log viewer.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/gnome-logs` |

---

#### Authenticator

Generate and store 2FA codes.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/authenticator` |

---

#### Helium Browser

Chromium based web browser.

| Attribute | Details |
| :--- | :--- |
| **Source** | `AUR/helium-browser-bin` |

> **Note: Enabling DRM for Helium Browser**
> 1. Download the latest [Chrome `.deb` package](https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb).
> 2. Extract the package:
>    ```bash
>    ar x google-chrome-stable_current_amd64.deb
>    ```
> 3. Extract the tar archive:
>    ```bash
>    tar -xf data.tar.xz -C extracted
>    ```
> 4. Find the Widevine version:
>    ```bash
>    cat extracted/opt/google/chrome/WidevineCdm/manifest.json | grep version
>    ```
> 5. Create the destination directory:
>    ```bash
>    mkdir -p ~/.config/net.imput.helium/WidevineCdm/<Widevine version>
>    ```
> 6. Move the Widevine contents into the created directory:
>    ```bash
>    mv extracted/opt/google/chrome/WidevineCdm ~/.config/net.imput.helium/WidevineCdm/<Widevine version>
>    ```

---

#### Ghostty

Terminal emulator.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/ghostty` |
| **Config Path** | `~/.config/niri/config.ghostty` |

---

#### Btop++

Resource Manager.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/btop` |

---

#### ONLYOFFICE

Office Suite.

| Attribute | Details |
| :--- | :--- |
| **Source** | `AUR/onlyoffice-bin` |

---

#### LocalSend

Sharing files over local network.

| Attribute | Details |
| :--- | :--- |
| **Source** | `AUR/localsend-bin` |

To configure localsend with firewalld it requires that you enable the port 53317.

	1. sudo firewall-cmd --permanent --add-port=53317/tcp
	2. sudo firewall-cmd --permanent --add-port=53317/udp
	3. sudo firewall-cmd --reload

---

#### VSCodium

Code editor.

| Attribute | Details |
| :--- | :--- |
| **Source** | `AUR/vscodium-bin` |

Install NoctaliaTheme to enable custom theme.

---

#### Telegram

Instant messaging program.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/telegram-desktop` |

1. Open your file manager and find the noctalia.tdesktop-theme file in the hidden ~/.cache/telegram-theme-noctalia/ folder.
2. Drag and drop this file into Telegram (e.g., into "Saved Messages").
3. Click on the sent file directly in the Telegram chat.
4. A theme preview window will appear — click "Apply".

https://github.com/Alexandr-Sol/telegram-theme-noctalia#applying-the-theme-in-telegram

---

### CLI tools

#### Xremap

Key remapper.

| Attribute | Details |
| :--- | :--- |
| **Source** | `AUR/xremap-niri-bin` |
| **Config Path** | `~/.config/xremap/config.yml` |

I have set up xremap to output tab switch keybinds for helium browser due to the fact that neither Firefox nor Chromium browsers allow me to set non-latin keys (like ctrl+ö) to switch tabs.

To configure xremap it requires the user to:

    1. add themselves to the input group: sudo gpasswd -a YOUR_USER input
    2. Give members of the input group permission to make output devices: echo 'KERNEL=="uinput", GROUP="input", TAG+="uaccess", MODE:="0660", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-input.rules
    3. Reboot

---


#### GHGrab

Github Grab, select files to pull from repo.

| Attribute | Details |
| :--- | :--- |
| **Source** | `AUR/ghgrab-bin` |

---

#### fzf

Fuzzy find files.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/fzf` |

---

#### lazygit

Git TUI.

| Attribute | Details |
| :--- | :--- |
| **Source** | `Extra/lazygit` |

---


### Web apps

A selection of web apps created with helium browser.

* Spotify
* Youtube
* Gemini
* Moodist

## Shortcuts

`Mod` refers to windows key.

### Navigation

#### Keyboard

| Key Binding | Action |
| :--- | :--- |
| `Mod` + `J` | Left |
| `Mod` + `K` | Down |
| `Mod` + `L` | Up |
| `Mod` + `Ö` | Right |
| `Mod` + `1/2/3` | Workspace (1/2/3) |
| `Mod` + `R` | Resize window |
| `Mod` + `F` | Resize fullscreen |
| `Mod` + `Shift` + `F` | Fullscreen |

In addition these most programs can be navigated similarly with `Ctrl` in place of `Mod`.

You can also hold `Shift` down while using these keybinds to move around windows.

#### Keyboard

| Key Binding | Action |
| :--- | :--- |
| `Mod` + `ScrollUp` | Left |
| `Mod` + `ScrollDown` | Right |
| `Mod` + `Shift` + `ScrollUp` | Up |
| `Mod` + `Shift` + `ScrollDown` | Down |

### System 

| Key Binding | Action |
| :--- | :--- |
| `Mod` + `D` | Program launcher |
| `Mod` + `Esc` | Session manager |
| `Mod`/`Ctrl` + `Q` | Quit (Program / Window) |
| `Mod`/`Ctrl` + `T` | New (Terminal / Window) |
| `Mod` + `O` | Overview |
| `Mod` + `H` | Help |
| `PrtSc` | Screenshot |
| `Shift` + `PrtSc` | Screenshot screen |
| `Alt` + `PrtSc` | Screenshot window |

### Programs
 
| Key Binding | Action |
| :--- | :--- |
| `Mod` + `A` | Authenticator |
| `Mod` + `S` | Localsend (Share) |
| `Mod` + `C` | Calculator |
| `Mod` + `G` | Gemini (AI) |
| `Mod` + `B` | Helium (Browser) |
| `Mod` + `P` | Proton Pass (Passwords) |