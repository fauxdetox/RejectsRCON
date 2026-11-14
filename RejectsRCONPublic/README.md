# Rejects RCON

A lightweight BattlEye RCon admin tool for DayZ servers, built to replace DaRT.

- One exe, no installer, nothing to install on your server.
- The interface opens in your web browser but runs only on your own PC (`127.0.0.1`). Nothing is opened to the internet.
- All you need is your server's IP, RCon port and RCon password.

---

## Contents

1. [Getting started](#getting-started)
2. [Finding your RCon details](#finding-your-rcon-details)
3. [The screen at a glance](#the-screen-at-a-glance)
4. [Players](#players)
5. [Bans](#bans)
6. [Player DB and notes](#player-db-and-notes)
7. [Admins](#admins)
8. [Console](#console)
9. [Tools](#tools)
10. [Settings](#settings)
11. [Running more than one server](#running-more-than-one-server)
12. [Closing the tool](#closing-the-tool)
13. [Your data](#your-data)
14. [Troubleshooting](#troubleshooting)
15. [What RCon can't do](#what-rcon-cant-do)
16. [Building from source](#building-from-source)
17. [License and credits](#license-and-credits)

---

## Getting started

1. **Unzip** the download somewhere permanent, for example `Documents\RejectsRCON`. Don't run it from inside the zip: the tool saves its settings and history in a `data` folder next to the exe.
2. **Double-click `RejectsRCON.exe`.** The exe isn't code-signed, so Windows may show "Windows protected your PC". Click **More info**, then **Run anyway**.
3. Your browser opens to the tool. Click **Settings**, and under **Servers** fill in:

   | Field | What to enter |
   |---|---|
   | Name | Any label, e.g. *My DayZ Server Name* |
   | Host / IP | Your server's IP address |
   | RCon port | `RConPort` from your BattlEye config (see below) |
   | RCon password | `RConPassword` from the same file |

4. Click **Save server**, close Settings, pick the server from the dropdown at the top left and click **Connect**.

The status pill turns green and shows **Connected**. The player list, ban list and console fill in within a few seconds.

## Finding your RCon details

RCon settings live in your server's BattlEye config file:

```
<server folder>\<profiles folder>\BattlEye\BEServer_x64.cfg
```

It looks like this:

```
RConPassword yourpasswordhere
RConPort 2306
RestrictRCon 0
```

- If the file has no `RConPort` or `RConPassword` line, add both (e.g. `RConPort 2306`) and restart the server.
- On a rented server, your host's control panel usually shows the RCon port and password.
- The RCon port is **UDP**. If your server is behind a firewall, it has to allow that port.

## The screen at a glance

- **Top bar:** server picker, Connect/Disconnect, connection status, player count, time connected, restart countdown (when one is running), Settings and Quit.
- **Tabs:** Players, Bans, Player DB, Admins, Tools.
- **Console** (bottom): live server messages and an input box. Drag the bar above it to resize.

The browser tab title shows the server name and player count, so you can tell copies apart when you run more than one.

## Players

A live list of everyone on the server. It refreshes every 10 seconds by default, and right away when someone joins or leaves.

| Column | Meaning |
|---|---|
| # | Slot number on the server |
| Name | Player name, plus tags (below) |
| CC | Country, from a built-in offline lookup (no web requests) |
| Ping | Green under 100, yellow under 200, red above |
| IP / GUID | Click either to copy it |
| Visits | How many times this tool has seen them join |

**Tags next to names:**

- `new`: first time this tool has seen them
- `+N names`: they've joined under other names before
- `note`: you've left a note on them (hover to read it)
- `lobby`: still loading in
- `unverified`: BattlEye hasn't verified their GUID yet

**Actions:**

- **Msg:** send a private message to that player.
- **Kick:** kick them with a reason (the player sees it). Pick from your preset reasons or type one.
- **Ban:** ban by GUID for a set time or permanently. You can tick **Also ban their last known IP**.
- **Click the row** to open their full history panel.
- **Message all** sends a global message.
- **Filter** narrows the list by name, IP or GUID.

> Kicks and bans go by the player's GUID, which is re-checked right before the command is sent. If someone leaves and a new player takes their slot number, the wrong player doesn't get hit.

## Bans

Shows every GUID and IP ban on the server, with time left and reason. If the tool has seen the player before, it shows the name they were known by. Click that name to open their history.

- **Add ban:** ban a 32-character BE GUID or an IPv4 address, including players who aren't online.
- **Unban:** removes the ban and saves the ban file on the server.
- **loadBans:** reloads `bans.txt` from disk, for when you've edited it by hand.
- **Filter** searches GUID, IP, reason and name.

**Durations:** 1 hour, 1 day, 3 days, 7 days, 30 days or Permanent. You can also type any number of minutes, where `0` means permanent.

## Player DB and notes

The tool remembers every player it sees while connected: every name they've used, every IP, country, first and last seen dates, and number of visits.

- **Search** by any name they've ever used, a GUID, an IP (or the start of one) or text in a note.
- **Click a player** to open their history panel. From there you can message, kick or ban them if they're online, ban them offline, or unban them.
- **Notes** are private to you. Write anything and click **Save note**.

**Watchlist:** put the word `watch` anywhere in a note. When that player joins, you get a highlighted alert in the console, a pop-up in the tool, and a flashing browser tab title if the tab is in the background.

The join message in the console also tells you when someone has joined under a different name than before, and shows their note.

## Admins

Lists the RCon admins currently logged in to the server, by IP and port. It updates every couple of minutes and whenever an admin logs in.

## Console

The console shows server messages as they happen:

- player joins, GUIDs and disconnects
- kicks and bans
- chat that BattlEye sees
- messages from other RCon admins
- BattlEye messages
- your own commands and messages

**Filters:** click the chips (Joins, Chat, Admin, Kicks, BattlEye, You, System) to hide or show each type. **Find in console** searches what's on screen. Untick **autoscroll** to stop it jumping to the bottom while you read.

**Input box:** choose a mode on the left.

- **Say (global):** sends a message to all players.
- **RCon command:** sends any RCon command as-is, e.g. `players`, `bans`, `#lock`, `kick 3 reason`.

Press the **Up arrow** to bring back earlier entries.

Every console line is also saved to a daily log file (see [Your data](#your-data)).

## Tools

| Tool | What it does |
|---|---|
| **Restart countdown** | Sends global warnings at 60, 30, 15, 10, 5, 3, 2 and 1 minutes (plus the time you start at). Tick **#shutdown at 0** to shut the server down when it ends, so your restart script can bring it back. Cancel it here or by clicking the countdown badge at the top. |
| **Server lock** | `#lock` stops new players joining (handy before a restart). `#unlock` opens it back up. |
| **BattlEye reloads** | `loadBans`, `loadScripts`, `loadEvents` and `writeBans`, for after you've edited BattlEye files on the server. |
| **Max ping** | Has BattlEye kick players above the ping you set. |
| **Scheduled messages** | Shows your repeating messages. Edit them in Settings. |
| **Logs** | Lists your recent daily log files. |
| **Shutdown now** | Sends `#shutdown` right away, with no warning to players. You're asked to confirm first. |

The restart warning text can be changed in Settings. Use `{m}` where the number of minutes should go.

## Settings

- **Servers:** add, edit and delete server profiles. When editing, leave the password blank to keep the saved one.
- **Player list refresh:** how often the player list updates, in seconds (minimum 3).
- **Connect on startup:** connect to a chosen server as soon as the tool opens.
- **Console lines kept on screen:** how much history the console shows. Log files keep everything regardless.
- **Restart warning text:** the countdown message (`{m}` = minutes).
- **Auto-reconnect:** reconnects by itself when the connection drops, e.g. during a server restart.
- **Save console to daily log files:** on by default.
- **Open the browser when the tool starts:** turn this off if you'd rather open it yourself.
- **Keep running after the browser tab is closed:** see [Closing the tool](#closing-the-tool).
- **Presets:** your ban reasons, kick reasons and quick messages, one per line. They appear as one-click buttons in the kick, ban and message windows.
- **Scheduled global messages:** repeating messages, each with its own interval in minutes. They're sent only while you're connected. Each one waits a full interval before it's sent the first time.

## Running more than one server

Make a **separate copy of the whole folder** for each server and run each copy. Every copy has its own settings, player history and logs, and picks its own local port automatically. The browser tab title shows which server each tab belongs to.

Double-clicking the same copy again doesn't start a second instance. It just reopens the browser tab for the one already running.

## Closing the tool

- Close the browser tab, or click **Quit**. The program exits by itself about 12 seconds after the last tab closes. Reloading the page is fine.
- It **stays open while a restart countdown is running**, so the warnings and `#shutdown` still go out.
- Turn on **Settings → Keep running** to leave it running with no tab open (e.g. for scheduled messages). Reopen it by double-clicking the exe again.

## Your data

Everything is stored in the `data` folder next to the exe:

| File | Contents |
|---|---|
| `settings.json` | Your settings and presets |
| `hosts.json` | Server profiles, **including RCon passwords in plain text** |
| `players.json` | Player history and notes |
| `logs\YYYY-MM-DD.log` | Console log, one file per day |
| `app.log` | The tool's own diagnostics, for troubleshooting |

**Never share your `data` folder.** If you pass the tool on to someone, send them a fresh copy of the zip.

Your RCon password is never sent to the browser page. The page only accepts requests from your own PC, and only with a key that changes every time the tool starts.

**Advanced:** you can run the exe with these options:

- `-data "D:\somewhere\data"` uses a different data folder.
- `-nobrowser` starts it without opening a browser.

## Troubleshooting

**"login timed out: no response from server"**
- Check you're using the **RCon port**, not the game port or query port.
- Make sure BattlEye RCon is enabled: `BEServer_x64.cfg` needs an `RConPassword` line, and the server must have been restarted since.
- Make sure the server's firewall allows the RCon port over **UDP**.

**"login failed: wrong RCon password"**
- Re-enter the password exactly as it appears in `BEServer_x64.cfg`. It's case-sensitive.

**Player list requests are timing out**
- The server is busy, restarting, or dropping packets. The tool keeps trying and tells you when the list responds again.

**"no reply from server (the command may still have run)"**
- RCon runs over UDP, so a reply can get lost. Check the player or ban list before trying again. For kicks, bans and unbans, the tool already checks whether the action happened before retrying, so you won't get double bans.

**"bad token - reload the page" / "Tool not responding"**
- The tool was restarted or closed. Reload the page, or start `RejectsRCON.exe` again.

**The page won't open / another program uses the port**
- The tool tries the next free local port automatically (it starts at 8723). Look for "Rejects RCON running at …" in the console window, or check `data\app.log`.

**Country shows `--`**
- The IP isn't in the built-in table, or it's a private/LAN address.

## What RCon can't do

BattlEye RCon doesn't carry kills, deaths or player positions, and it doesn't see chat from mods that send chat their own way. Those need server-side logs or mods, which this tool deliberately doesn't require.

---

## Building from source

Requires **Go 1.24** or newer.

```
go.mod  main.go  app.go  web.go  store.go  parse.go  geo.go  geo.bin
os_windows.go  os_other.go
rcon/rcon.go        BattlEye RCon protocol client
ui/index.html       the whole web interface (embedded in the exe)
mockbe/main.go      fake BattlEye server for testing (not shipped)
```

Build the Windows exe:

```
GOOS=windows GOARCH=amd64 go build -trimpath -ldflags="-s -w" -o RejectsRCON.exe .
```

From a Windows command prompt, set `GOOS`/`GOARCH` with `set` first, or just run `go build -trimpath -ldflags="-s -w" -o RejectsRCON.exe .` on a Windows machine.

`-trimpath` keeps your own folder paths out of the exe.

### Testing without a real server

`mockbe` is a fake BattlEye RCon server. It simulates players joining, leaving and chatting, drops packets on purpose, and seeds a long ban list so multi-packet replies get exercised.

```
go run ./mockbe -addr 127.0.0.1:2306 -pw test -drop 0.1 -bans 120
```

| Flag | Default | Meaning |
|---|---|---|
| `-addr` | `127.0.0.1:2306` | Address to listen on |
| `-pw` | `test` | RCon password |
| `-drop` | `0.1` | Fraction of packets to drop (0 to 1) |
| `-bans` | `120` | Number of fake bans to seed |

Then add a server in Rejects RCON with host `127.0.0.1`, port `2306` and password `test`. All names, GUIDs and IPs in the mock are made up.

## License and credits

Rejects RCON is by **Taco Donkey** and released under the **WTFPL v2**. See `LICENSE.txt`. Do whatever you want with it.

The offline IP-to-country table (`geo.bin`) comes from [DaRT](https://github.com/DomiStyle/DaRT) by DomiStyle, under the MIT License. See `THIRD-PARTY-NOTICES.txt`.
