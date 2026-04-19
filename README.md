# 🚀 Discord Vanity Checker V2
*Created by [@_m13]*

A high-performance, aesthetically pleasing, and robust Python script to automatically check the availability of Discord vanity URLs (`discord.gg/vanity`). Built to bypass API rate limits utilizing smart proxy rotation and keep you updated instantly via Discord Webhooks!

---

## ✨ Features
* **Beautiful CLI Dashboard:** Uses custom color-coded logs, ASCII art, and clean formatting to display check statuses in real time.
* **Multi-Threading:** Run up to 500+ simultaneous checks. You choose the thread count at startup — no config editing needed.
* **Vanity Generator:** Generate vanity lists automatically without typing them manually. Five generation modes available (see below).
* **Auto Proxy Rotation:** Provide a list of SOCKS4/SOCKS5, HTTP, or HTTPS proxies. The script automatically rotates proxies on rate limits with exponential backoff on failures.
* **Discord Webhook Alerts:** Automatically sends a formatted Rich Embed message to your Discord server the moment an available vanity is found.

---

## 🛠️ Installation

1. **Ensure you have Python installed** (Python 3.7+ recommended).
2. **Install the required packages:**
   Open a terminal in this folder and run:
   ```bash
   pip install -r requirements.txt
   ```

---

## ⚙️ Configuration

### 1. Set up Proxies (Optional, but mandatory for high volume)
Without proxies, Discord will IP ban you after ~50 rapid requests.
Open `proxies.txt` and paste in your proxies. Rotating proxies (e.g. DataImpulse) are strongly recommended.
```text
# Examples:
1.2.3.4:8080
user:password@1.2.3.4:8080
socks5://user:password@1.2.3.4:1080
http://gw.dataimpulse.com:823
```

### 2. Set up Discord Webhooks (Optional)
To get notified when a vanity is found:
1. Go to your Discord Server Settings > Integrations > Webhooks.
2. Create a new webhook and copy the URL.
3. Open `config.json` and paste it inside the quotes:
```json
{
  "webhook_url": "https://discord.com/api/webhooks/YOUR/WEBHOOK/URL"
}
```

---

## 🚀 Running the Checker

```bash
python checker.py
```

Or double-click `checker.exe` if you're using the compiled release.

At startup you will be presented with three options:

| Option | Description |
|--------|-------------|
| `[1]` Check Vanities | Load `vanities.txt` and start checking |
| `[2]` Generate Vanities | Auto-generate a vanity list (see below) |
| `[3]` Join Discord | Opens the support server |

---

## ⚡ Multi-Threading

When starting a check, you will be asked for a thread count:
```
> Threads (e.g. 50, 200, 500):
```
* **Recommended with rotating proxies:** 100–200 threads
* **Without proxies:** keep it at 1–5
* More threads = faster checks, but too many will overwhelm your proxy gateway and produce errors

---

## 🔧 Vanity Generator

Select `[2] Generate Vanities` from the main menu. Five modes are available:

### [1] Random
Generates N random vanities of a chosen length (or length range like `3-6`).
Choose a charset: `a-z`, `a-z + 0-9`, or custom.

### [2] Brute Force
Generates every possible combination for a given length.
Example: length `3` + `a-z` = all 17,576 three-letter combos.
The tool will warn you of the total count before generating.

### [3] Word Combinator
Mix a list of words together as pairs, or attach prefixes/suffixes.
You can type words inline (comma-separated) or load from a `words.txt` file.

| Sub-mode | Example output |
|----------|---------------|
| Word pairs | `darkwolf`, `voidking` |
| Word + suffix | `ghost99`, `null-x` |
| Prefix + word | `the-void`, `xo-faded` |

### [4] Rare Only
Generates only vanities that match high-value patterns:

| Pattern | Examples |
|---------|---------|
| Repeating | `aaa`, `zzz`, `111` |
| Palindromes | `aba`, `121`, `abba` |
| Sequential | `abc`, `xyz`, `123`, `987` |
| Alternating | `abab`, `xyxy`, `1212` |
| Repeat pairs (even length) | `aabb`, `ccdd`, `1122` |

### [5] Preset Lists
Curated word lists — no typing required. Pick one or load all at once:

| Preset | Description | Example |
|--------|-------------|---------|
| Hacker & Technical | High-authority tech terms | `kernel`, `payload`, `shellcode`, `exploit` |
| Rare 3-Char Combos | Aesthetic alphanumeric strings | `z7x`, `h4x`, `pwn`, `xor` |
| Aesthetic & Dictionary | Prestige short words | `abyss`, `void`, `phantom`, `nebula` |
| All of the above | Everything merged & deduped | — |

After generating, you will be asked:
```
> Start checking now? (y/n):
```
Answer `y` to jump straight into checking without going back to the menu.

---

## 📄 Output

* Available vanities are printed to the console in **green**
* All available vanities are saved to `available.txt` automatically
* Webhook notification is fired for each available vanity (if configured)

---

## ⚠️ Restricted Vanities

Some vanities will show as **AVAILABLE** in the checker but **cannot actually be claimed**. Discord silently reserves a large number of codes — common words, brand names, short strings, and others — and returns a `404` response for them just like a genuinely free vanity would.

When you try to set one of these on your server, Discord will reject it with an error like:
> *"This vanity URL is not available."*

There is no way to detect restricted vanities through the API — Discord does not distinguish them from truly available ones. Treat every `AVAILABLE` result as a **candidate**, not a guarantee. Always attempt to claim it manually to confirm.

---

**Disclaimer:** Use responsibly. Pinging Discord's API excessively may result in IP or account bans. Always use a strong rotating proxy network.
