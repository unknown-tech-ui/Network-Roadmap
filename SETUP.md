# Setup Guide

Everything you need to install or account for, broken down by day so you're never stuck mid-session. Most tools only need to be installed once, before Day 1.

## Before Day 1 — Core Setup (do this in advance)

- **A virtualization app**: [VirtualBox](https://www.virtualbox.org/) (free, cross-platform) or [VMware Workstation Player](https://www.vmware.com/products/workstation-player.html) (free for personal use)
- **A Linux VM**: [Kali Linux](https://www.kali.org/get-kali/#kali-virtual-machines) (pre-loaded with most tools used this week) or plain [Ubuntu Desktop](https://ubuntu.com/download/desktop) if you'd rather install tools yourself
- **A modern browser** with DevTools: Chrome, Firefox, or Edge all work fine
- **Wireshark** (with Npcap on Windows) — needed from Day 4 onward, but worth installing now:
  ```bash
  sudo apt install wireshark -y
  # during install, say "yes" to let non-root users capture packets,
  # then: sudo usermod -aG wireshark $USER   (log out/in after)
  ```
- **CyberChef** — bookmark [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/), no install needed
- **A terminal** ready for `curl`, `dig`/`nslookup`, and `ping` (WSL for Windows users)
- **A Google account** (for submitting Mini-Challenge answers via Google Form) — or swap in Discord/whatever submission method you're running

Give your VM at least 2 CPU cores, 4GB RAM, and 25GB disk if possible — Nmap, Wireshark, and a browser running together want some headroom.

If using Kali, update it once after first boot:
```bash
sudo apt update && sudo apt full-upgrade -y
```

Also bookmark 2–3 references for anyone who wants to go deeper on their own: **PortSwigger Web Security Academy**, **Cloudflare Learning Center**, **CryptoHack**.

---

## Day 1 — How the Web Actually Works

No installs needed beyond the core setup. Just your browser and its built-in DevTools (`F12` or `Ctrl+Shift+I`).

- Target: [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) — either use the [public demo instance](https://demo.owasp-juice.shop/) or run it locally:
  ```bash
  docker run -d -p 3000:3000 bkimminich/juice-shop
  ```
  (requires [Docker](https://docs.docker.com/engine/install/) — install it during Before Day 1 setup if you'll self-host)

## Day 2 — Web App Security Basics

- Same Juice Shop instance from Day 1 — no new tools.
- Browser DevTools again (Network + Console tabs this time for the SQLi/XSS/IDOR walkthroughs).

## Day 3 — Cryptography & the XOR Atom

- **[CyberChef](https://gchq.github.io/CyberChef/)** — runs entirely in-browser, no install. Bookmark it.
- Optional: Python 3 (already on Kali/Ubuntu) if you'd rather brute-force the single-byte XOR with a 5-line script than CyberChef's recipe.
- No command line strictly required today.

## Day 4 — Taste: Forensics

- **Wireshark** — installed in core setup. If skipped:
  ```bash
  sudo apt install wireshark -y
  ```
- A small, provided `.pcap` file (organizer supplies this — see Organizer-only checklist below).
- Windows users: confirm Npcap installed alongside Wireshark, or capture will fail.

## Day 5 — Networking & DNS Fundamentals

- **`nslookup` / `dig`** — usually pre-installed on Kali/Ubuntu. If `dig` is missing:
  ```bash
  sudo apt install dnsutils -y
  ```
- **`traceroute`** — usually pre-installed; if missing: `sudo apt install traceroute -y`
- **A free/test VPN client** for the bonus IP-comparison exercise (e.g. [ProtonVPN free tier](https://protonvpn.com/free-vpn)) — optional, install ahead if you want to do the bonus.
- An IP-lookup site (e.g. [whatismyipaddress.com](https://whatismyipaddress.com/)) — browser only, no install.

## Day 6 — A Peek at Recon & Enumeration

- **Nmap** — comes pre-installed on Kali; on Ubuntu:
  ```bash
  sudo apt install nmap -y
  ```
- Browser access to [crt.sh](https://crt.sh/) — no install.

## Day 7 — Capstone

Combines everything above, plus:
- **SSH client** — pre-installed on Kali/Ubuntu (`ssh` command). Windows users not on WSL should install [PuTTY](https://www.putty.org/) or use Windows' built-in `ssh` (Windows 10/11 has it natively).
- **VPN client** — the same one from Day 5, now used to connect to the capstone network (organizer will provide connection details/config file).
- Everything from Days 1–6: browser + DevTools, CyberChef, Wireshark, `dig`/`nslookup`, Nmap, and your VM.

---

## Organizer-only checklist (not for participants)

If you're running this cohort, also prepare in advance:
- An isolated practice VM per participant/pair (Day 6–7 target) — never production infrastructure
- A VPN config/profile for the capstone network (Day 7)
- The curated sample `.pcap` file with an HTTP cleartext login (Day 4)
- The pre-made "spoofed vs. normal DNS" packet capture exhibit (Day 5)
- The single-byte XOR challenge script and saved secret key/answer, plus 3 pre-hashed weak passwords (Day 3)
- The model "Intel Report" answer sheet for the Day 6 Nmap exercise
- A hosted Juice Shop fallback instance for anyone who can't run Docker
- The Google Form / Discord bot wired up for challenge and quiz submissions
