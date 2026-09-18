# Deck

An interactive terminal console for checking the reputation of IP addresses and file hashes, built in the style of the Claude Code terminal.

AbuseIP queries **AbuseIPDB** and **VirusTotal** side by side, lets you submit abuse reports, and keeps a local history of everything you've checked or reported.

by Shahab Mohammadi · v 3

```
╭─────────────────────────────────────────────────────╮
│ ✻ Welcome to Deck!  v3.0 · by Shahab Mohammadi   │
│                                                     │
│   /help for help, /keys for your API key setup      │
│                                                     │
│   cwd: ~/Downloads/ip                               │
╰─────────────────────────────────────────────────────╯

> 185.220.101.1

⏺ AbuseIPDB(185.220.101.1)  MALICIOUS
  ⎿  Score           █████████████░░░░░░░ 64%
     Reports         212 from 48 reporters (last 90d)
     Last reported   2026-09-16 08:00 UTC (today)
     Country         Netherlands
     ISP             Example Hosting BV
     Flags           Tor exit

⏺ VirusTotal(185.220.101.1)
  ⎿  Detections      5 malicious, 1 suspicious / 94 engines
     Reputation      -12
     AS owner        EXAMPLE-AS

─────────────────────────────────────────────────────────────────────
> Try "8.8.8.8", a file hash, or /help
─────────────────────────────────────────────────────────────────────
  ? for shortcuts                         ● AbuseIPDB  ● VirusTotal
```

*(Example output with made-up data.)*

## Features

- **IP check:** queries AbuseIPDB and VirusTotal in parallel and shows the abuse score, report count, country, ISP, usage type, Tor/whitelist flags, engine detections and reputation.
- **Hash check:** looks up MD5, SHA1 or SHA256 file hashes on VirusTotal.
- **Abuse reports:** submits a report to AbuseIPDB with categories and a comment, after showing you a review screen to confirm.
- **History:** logs every check and report locally.
- **Usage:** shows today's checks and reports, the free-tier limits, and your live VirusTotal quota.
- **API key manager:** keys are validated before saving and stored with `600` permissions.
- **Claude Code–style interface:** a prompt between two rules, a `/` command menu, arrow-key pickers, an animated spinner, and `⏺` / `⎿` result blocks.
- **No dependencies:** uses only the Python standard library.

## Requirements

- Python **3.9+**
- macOS or Linux for the full interactive interface. On Windows, or when input is piped, AbuseIP falls back to simple typed prompts.
- Free API keys:
  - AbuseIPDB: <https://www.abuseipdb.com/account/api>
  - VirusTotal: <https://www.virustotal.com/gui/my-apikey>

## Getting started

```bash
cd ip
python3 deck.py
# or
./deck.py
```

The first time you run a lookup, AbuseIP asks for any API key it needs. Input is masked, and the key is checked against the service before it's saved. You can also add keys any time with `/keys`.

## Usage

Type an **IP address** or a **file hash** and press **Enter** to look it up, or use a command. Type `/` to open the command menu.

| Command | Description |
|---|---|
| `/check <ip>` | Check an IP on AbuseIPDB and VirusTotal |
| `/report <ip>` | Report an abusive IP to AbuseIPDB |
| `/hash <hash>` | Look up a file hash (MD5/SHA1/SHA256) on VirusTotal |
| `/history` | Show your last 25 checks and reports |
| `/usage` | Show today's usage and API quota |
| `/keys` | Add, update or view API keys |
| `/clear` | Clear the screen |
| `/help` | Show commands and shortcuts |
| `/exit` | Exit AbuseIP |

If you leave out the argument (for example, just `/check`), deck asks for it.

Aliases: `/ip` → `/check`, `/settings` → `/keys`, `/quit` and `/q` → `/exit`, `/?` → `/help`.

### Reporting an IP

1. Run `/report 203.0.113.7`.
2. Pick one or more categories. Use **↑↓** to move, **Space** to tick, or type a category number to jump to it (for example `22` for SSH).
3. Enter a comment describing the evidence. AbuseIPDB requires one.
4. Check the summary and choose **Yes, submit to AbuseIPDB**.

Press **Esc** at any step to cancel; nothing is sent.

### Keyboard shortcuts

| Key | Action |
|---|---|
| `/` | Open the command menu |
| `↑` `↓` | Move through the menu, or browse earlier inputs |
| `Tab` | Autocomplete the selected command |
| `?` | Show or hide the shortcuts panel |
| `Esc` `Esc` | Clear the input |
| `Ctrl+C` | Clear the input, stop a running request, or press twice to exit |
| `Ctrl+L` | Clear the screen |
| `Ctrl+D` | Exit |
| `Ctrl+A` / `Ctrl+E` | Jump to the start / end of the line |
| `Ctrl+U` / `Ctrl+K` | Delete to the start / end of the line |
| `Ctrl+W` | Delete the previous word |

## Configuration

### API keys

Keys are resolved in this order:

1. Environment variables `ABUSEIPDB_API_KEY` and `VIRUSTOTAL_API_KEY`
2. The saved config file
3. An interactive prompt (the key is then saved to the config file)

```bash
export ABUSEIPDB_API_KEY="your-key"
export VIRUSTOTAL_API_KEY="your-key"
```

### Files

All data is stored in the config directory:

| OS | Location |
|---|---|
| macOS / Linux | `$XDG_CONFIG_HOME/abuseip/`, or `~/.config/abuseip/` if `XDG_CONFIG_HOME` isn't set |
| Windows | `%APPDATA%\abuseip\` |

| File | Contents |
|---|---|
| `config.json` | Saved API keys (permissions `600`) |
| `history.jsonl` | Log of checks and reports |
| `prompt_history.json` | Your last 200 inputs, for ↑ recall |

Keys are only ever sent to the service they belong to.

### Colors

- Set `NO_COLOR=1` to turn off colors.
- The banner is drawn in black, so it reads best on a light terminal background.
- Accent colors use true color when `COLORTERM=truecolor` is set, and 256 colors otherwise.

## Free-tier limits

| Service | Limit |
|---|---|
| AbuseIPDB | 1,000 checks/day, 1,000 reports/day |
| VirusTotal | ~500 lookups/day, 4 requests/minute |

These are for reference only. Run `/usage` to see your live VirusTotal quota.

## Author

Shahab Mohammadi
