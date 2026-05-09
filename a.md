# Quick Start

Download the required files from [GitHub Releases](https://github.com/kulikov0/whitelist-bypass/releases).

## Contents

- [Requirements](#requirements)
- [Creator (desktop)](#creator-desktop)
- [Creator (headless, server)](#creator-headless-server)
- [VK Bot](#vk-bot)
- [VK Bot (headless, server)](#vk-bot-headless-server)
- [Joiner (Android)](#joiner-android)
- [Joiner (iOS)](#joiner-ios)
- [Joiner (Linux, headless)](#joiner-linux-headless)

## Requirements

- **Creator** (side with unrestricted internet) — desktop or headless on a server
- **Joiner** (side with censorship) — Android or iOS

Two tunnel modes: **DC** (DataChannel) and **Video** (VP8). Headless creator automatically adapts to the mode chosen by the joiner on each new connection. This is not possible with the legacy browser path — you can't create DC in VK and connect via video.

> **Headless on both sides is recommended** — it features traffic obfuscation, configurable VP8 pacing and WB Stream. The old browser path (Android `WebView` vs Electron-creator with JS hooks) still works, but is slower, lacks obfuscation, and is gradually being phased out.

## Creator (desktop)

![Interface](res/desktop_interface.png)

1. Download and launch the application
2. Press **+** to create a tab
3. Select the service: **VK** or **Telemost**
4. Select connection type: **DC** (VK only) or **Video**
5. Authorize in the opened window
6. Create a call
7. Copy the call link and send it to Joiner

**Headless mode** — creating a call without a browser. First authorize through normal mode (VK or Telemost), then use **Headless VK** / **Headless TM**.

If you need to run headless on a server without GUI — export cookies using the **VK Cookies** / **Yandex Cookies** buttons and use them with headless binaries (see README).

> Running desktop Creator on a VPS without a graphical environment via XPRA — see [docs/vps/SETUP.md](vps/SETUP.md).

## Creator (headless, server)

For running Creator on a server without GUI.

### Preparing cookies

Cookies are needed for platform authorization. Export them from desktop Creator:

1. Open Creator on desktop
2. Authorize in VK or Telemost through normal mode
3. Press **VK Cookies** or **Yandex Cookies**
4. Copy the resulting file to the server

### Launch

```sh
# VK
./headless-vk-creator --cookies cookies.json

# Telemost
./headless-telemost-creator --cookies cookies-yandex.json

# WB Stream (anonymous guest token, no cookies needed)
./headless-wbstream-creator
```

After launch, Creator will create a call and output the link to the log. The link must be passed to Joiner.

### Connecting to an existing call

To avoid recreating the call on restart, pass an existing link:

```sh
./headless-vk-creator       --cookies cookies.json        --vk-link https://vk.com/call/join/<token>
./headless-telemost-creator --cookies cookies-yandex.json --tm-link https://telemost.yandex.ru/j/<id>
./headless-wbstream-creator --room wbstream://<uuid>
```

### Flags

| Flag | VK | TM | WB | Description |
|---|---|---|---|---|
| `--cookies <path>` | yes | yes | - | Path to cookies file (JSON) |
| `--cookie-string <str>` | yes | yes | - | Cookies as a string (`name=val; name=val`) |
| `--peer-id <id>` | yes | - | - | VK peer_id for a new call |
| `--vk-link <link>` | yes | - | - | Connect to an existing VK call |
| `--tm-link <uri>` | - | yes | - | Connect to an existing Telemost conference |
| `--room <id>` | - | - | yes | Connect to an existing WB Stream room |
| `--resources <mode>` | yes | yes | yes | `default` / `moderate` / `unlimited` / `custom` |
| `--read-buf <bytes>` | yes | yes | yes | Read buffer size, only with `--resources custom` |
| `--max-dc-buf <bytes>` | yes | - | - | DC `BufferedAmountLowThreshold` threshold, only with `--resources custom` |
| `--mem-limit <bytes>` | yes | yes | yes | Go runtime soft memory limit, only with `--resources custom` |
| `--write-file <path>` | yes | yes | yes | File where the active call link is written |

### Resource modes

| Mode | `read-buf` | `max-dc-buf` (VK) | `mem-limit` | When to use |
|---|---|---|---|---|
| `moderate` | 16 KB | 1 MB | 64 MB | Low-RAM VPS |
| `default`  | 32 KB | 4 MB | 128 MB | Normal usage |
| `unlimited`| 64 KB | 8 MB | 256 MB | Maximum throughput (may throttle due to congestion control) |
| `custom`   | from `--read-buf` | from `--max-dc-buf` | from `--mem-limit` | Fine-tuning |

In `custom` mode, any unspecified flag uses values from `unlimited`. Example with explicit configuration of all buffers:

```sh
./headless-vk-creator \
  --cookies cookies.json \
  --vk-link https://vk.com/call/join/<token> \
  --write-file /var/run/whitelist-bypass/call.txt \
  --resources custom \
  --read-buf 65536 \
  --max-dc-buf 8388608 \
  --mem-limit 268435456
```

## VK Bot

The bot allows creating calls via VKontakte messages without direct access to Creator.

### Setup

1. Create a VKontakte community (can be private)

![Create community](res/create_community.png)

2. Go to "Management" of the community

![Management](res/management.png)

3. Section "Advanced" -> "API" -> "Create key", check all boxes

![API](res/api_section.png)
![Create key](res/create_key.png)
![Create key](res/create_key1.png)

4. Confirm SMS and copy the key (re-copying will require SMS)

![Copy key](res/copy_key.png)

5. Enable Long Poll API, in "Event types" -> "Messages" check all boxes

![Long Poll API](res/long_poll_api1.png)

6. "Messages" -> "Bot settings" -> enable "Bot capabilities"

![Bot settings](res/bot_settings.png)

7. Copy the community ID (digits only, without "club")

![Community ID](res/community_id.png)

8. Find out your VK ID (profile -> "Manage VK ID account" -> "My data")

9. Fill in bot settings in Creator: Token, Group ID, User ID. Save and enable the bot.

![Bot settings in Creator](res/bot_config.png)

### Commands

- `/vk dc` — VK call, DC mode
- `/vk video` — VK call, Video mode
- `/vk headless` — VK call, headless mode
- `/tm video` — Telemost call, Video mode
- `/tm headless` — Telemost call, headless mode
- `/wb headless` — WB Stream room
- `/list` — list of active tabs
- `/close <id>` — close tab by ID

![Bot commands](res/bot_commands.jpeg)

## VK Bot (headless, server)

Standalone Go binary `headless-vk-bot` — the same as the bot in desktop Creator, but without Electron. Listens to community Long Poll, launches the appropriate headless-creator on command, and sends the join-link to the chat. Suitable for VPS: one process holds Long Poll and spawns creators on demand.

### Preparation

1. Create a VKontakte community and get a community access token, group ID and your VK ID — the procedure is described in the [VK Bot](#vk-bot) section (steps 1-8).
2. Download the binaries from [GitHub Releases](https://github.com/kulikov0/whitelist-bypass/releases) and place them in one folder — for example `/usr/local/bin/`:
   - `headless-vk-bot-linux-x64`
   - `headless-vk-creator-linux-x64`
   - `headless-telemost-creator-linux-x64`
   - `headless-wbstream-creator-linux-x64`

   Don't forget to grant execute permissions: `chmod +x /usr/local/bin/headless-*`.
3. Prepare cookies:
   - VK: export via desktop Creator using the **VK Cookies** button.
   - Telemost: using the **Yandex Cookies** button.
   - WB Stream does not require cookies (anonymous guest token).
4. Copy cookies to the server.

### Launch

```sh
./headless-vk-bot \
  --token <community_access_token> \
  --group-id <community_id> \
  --user-id <your_vk_id> \
  --bins-dir /usr/local/bin \
  --vk-cookies /etc/whitelist-bypass/cookies-vk.json \
  --tm-cookies /etc/whitelist-bypass/cookies-yandex.json
```

### Flags

| Flag | Description |
|---|---|
| `--token <str>` | Community access token (required) |
| `--group-id <id>` | Community ID, digits only (required) |
| `--user-id <id>` | VK ID of the user allowed to send commands. Empty = allowed to everyone (NOT recommended) |
| `--bins-dir <dir>` | Folder containing `headless-vk-creator` / `headless-telemost-creator` / `headless-wbstream-creator` (required) |
| `--vk-cookies <path>` | Path to VK cookies (needed for `/vk`) |
| `--tm-cookies <path>` | Path to Yandex cookies (needed for `/tm`) |
| `--sessions-dir <dir>` | Folder for logs of spawned creators. Optional — without the flag logs are not written, stdout/stderr of creators are discarded |

Upon receiving a command, the bot spawns the corresponding creator with `--write-file <tmp>`, waits for the link to appear in the file (up to 60 seconds) and sends it to the chat. If you specify `--user-id`, commands from other users will be ignored.

### Running as a systemd service

`/etc/systemd/system/wlb-vk-bot.service`:

```ini
[Unit]
Description=Headless VK bot (whitelist-bypass)
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/local/bin/headless-vk-bot \
  --token <community_access_token> \
  --group-id <community_id> \
  --user-id <your_vk_id> \
  --bins-dir /usr/local/bin \
  --vk-cookies /etc/whitelist-bypass/cookies-vk.json \
  --tm-cookies /etc/whitelist-bypass/cookies-yandex.json
Restart=always
RestartSec=5
User=wlb

[Install]
WantedBy=multi-user.target
```

Activate:

```sh
sudo systemctl daemon-reload
sudo systemctl enable --now wlb-vk-bot
sudo journalctl -u wlb-vk-bot -f
```

When the service stops, the bot sends `SIGTERM` to all spawned creator processes to avoid leaving hanging sessions.

### Commands

- `/vk` — launch `headless-vk-creator`
- `/tm` — launch `headless-telemost-creator`
- `/wb` — launch `headless-wbstream-creator`
- `/list` — list of active sessions
- `/close <id>` — close session by short ID
- `/start` — show main menu

All commands are also available via inline keyboard in the chat.

## Joiner (Android)

![Interface](res/android_interface.png)

1. Download and install `whitelist-bypass.apk`
2. On first launch, allow the VPN connection in the system dialog
3. Open settings (button to the right of GO) and select the tunnel mode (**DC** or **Video**) matching the Creator
4. Paste the call link into the input field
5. Press **GO**
6. Wait for the "Tunnel active" status — all device traffic now goes through the call

### Settings

- **Tunnel** — tunnel mode (DC / Video)
- **Headless** — connect without WebView (recommended, enabled by default)
- **Split tunneling** — choose which apps go through the tunnel
- **Proxy settings** — SOCKS5 port, authorization. "Proxy only" mode — no VPN, proxy only
- **DNS settings** — system or custom DNS
- **VP8 pacing** — override VP8 pacing parameters (see below)
- **Autoclick settings** — auto-join the call with a specified name
- **Reconnect on app start** — auto-connect to the last link on launch
- **Show logs** — show logs for debugging

### VP8 pacing

Controls how often the joiner sends VP8 frames through the SFU. Configured only on the joiner; the creator adapts to the values sent by the joiner at the start of the session.

- **Override VP8 pacing** — disabled by default. When unchecked, defaults `fps=24 batch=30` are used (≈6.5 Mbps theoretical ceiling). When enabled, two fields become available.
- **FPS** — nominal VP8 frame rate. Range 1..240. Typically 24-30.
- **Batch** — tick density multiplier. Actual send rate ≈ `fps × batch` frames/sec. Range 1..256.

Throughput ≈ `fps × batch × 1126 bytes/frame`. Examples:

| fps | batch | ceiling |
|----:|------:|--------:|
| 24  | 1     | ~27 KB/s |
| 24  | 8     | ~216 KB/s |
| 24  | 30    | ~810 KB/s (≈6.5 Mbps) |

Higher batch means more load on the phone's CPU and the SFU. If packet drops appear in logs or the connection is unstable — reduce batch.

> **Important:** obfuscation and VP8 pacing only work in **headless-headless** mode — both creator and joiner must be in headless mode.

### If it doesn't work

Try changing DNS: in app settings **DNS settings** select Custom (default `8.8.8.8` / `8.8.4.4`). Also check that Android DNS system settings are set to "Automatic" (without Private DNS).

## Joiner (iOS)

On iOS, only SOCKS5 proxy mode is available (no VPN). To proxy all device traffic, use a VPN app with SOCKS5 support (e.g., Shadowrocket or Streisand).

1. Download `whitelist-bypass-proxy.ipa` (unsigned) from [GitHub Releases](https://github.com/kulikov0/whitelist-bypass/releases) and install it (via AltStore, Sideloadly, or sign with your own account). Or build from source (see README).
2. Install any VPN app with SOCKS5 support (Shadowrocket, Streisand, etc.).
3. Open whitelist-bypass, select tunnel mode (**DC** or **Video**), paste the call link and tap **Go**.
4. Wait for "Tunnel Active" status. The app will display the SOCKS5 proxy address (e.g. `socks5://user:pass@127.0.0.1:1080`).
5. Copy SOCKS5 parameters from whitelist-bypass.
6. Paste them into the VPN app and connect — all device traffic will go through the tunnel.

Alternatively, the proxy can be set directly in individual apps:
- **Telegram**: Settings -> Data and Storage -> Proxy -> Add Proxy -> SOCKS5
- Or tap the **"Open in Telegram"** button in whitelist-bypass for automatic setup

### Settings

- **Tunnel mode** — tunnel mode (DC / Video)
- **Auth mode** — proxy authorization (Auto — random credentials, Manual — custom)
- **Display name** — name when joining the call
- **VP8 Pacing** — override VP8 parameters (see VP8 pacing section above)
- **Show logs** — show logs for debugging

> SOCKS5 proxy mode was chosen due to Apple restrictions: using NetworkExtension (VPN) requires a paid Apple Developer account and doesn't work via sideload. If someone from the community implements a full VPN based on these sources — that would be great.

## Joiner (Linux, headless)

Headless joiner for Linux servers and desktops. Sets up a local SOCKS5 proxy through which any traffic can be routed (e.g., `curl --socks5`, Telegram, system-wide via `redsocks`/`tun2socks`).

Download the binary from [GitHub Releases](https://github.com/kulikov0/whitelist-bypass/releases):

- `headless-wbstream-joiner-linux-x64` / `-ia32` — for WB Stream
- `headless-telemost-joiner-linux-x64` / `-ia32` — for Telemost

> VK joiner is not suitable for the headless approach: joining a call requires solving a captcha, so there is no Linux binary for VK. Use the Android/iOS client.

### Launch

```sh
# WB Stream
./headless-wbstream-joiner --room wbstream://<uuid> --socks-port 1080

# Telemost
./headless-telemost-joiner --tm-link https://telemost.yandex.ru/j/<id> --socks-port 1080
```

After the `TUNNEL CONNECTED` line, SOCKS5 is up on `127.0.0.1:<socks-port>`. Check:

```sh
curl --socks5 127.0.0.1:1080 https://api.ipify.org
```

### Flags

| Flag | WB | TM | Description |
|---|---|---|---|
| `--room <link>` | yes | - | `wbstream://<uuid>` or just room UUID |
| `--tm-link <uri>` | - | yes | `https://telemost.yandex.ru/j/<id>` |
| `--name <name>` | yes | yes | name in the call (default `Joiner`) |
| `--socks-port <port>` | yes | yes | SOCKS5 port (default `1080`) |
| `--socks-user <user>` | yes | yes | SOCKS5 login (optional) |
| `--socks-pass <pass>` | yes | yes | SOCKS5 password (optional) |
| `--resources <mode>` | yes | yes | `default` / `moderate` / `unlimited` |
| `--tunnel-mode <mode>` | yes | - | `video` or `dc` (WB only) |
| `--vp8-fps <fps>` | yes | yes | VP8 frame rate (default `24`) |
| `--vp8-batch <n>` | yes | yes | batch multiplier (default `30`) |

When specifying `--socks-user`/`--socks-pass`, SOCKS5 requires authentication. Without them, the proxy is open to everyone on `127.0.0.1`.

### System-wide tunnel (like Android VPN)

To proxy all host traffic, use `tun2socks` or `redsocks` on top of the local SOCKS5. Example with `tun2socks`:

```sh
sudo tun2socks -device tun://wb0 -proxy socks5://127.0.0.1:1080
```

---

[Author's blog](https://t.me/markovdrankthechains)

### Thank the author

- `0xd986b7576340d8d7b04f806dfd38a182b19edf50` - USDC (ERC20)
- `TTEo4XXTB6CqhEiKpyoncfk3skEvoq3bCP` - USDT (TRC20)
```
