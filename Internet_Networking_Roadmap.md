# 🗺️ Unified Internet & Networking Roadmap (Week 1)

*A 7-day, beginner-friendly cybersecurity & networking bootcamp*

## Before Day 1 — Setup Checklist (send this out in advance)

* **VMware/VirtualBox**: with Kali Linux or Ubuntu installed
* **Wireshark** (with Npcap on Windows) — needed from Day 4 onward
* **CyberChef**: bookmark [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/), no install needed
* A terminal ready for `curl`, `dig`/`nslookup`, and `ping` (WSL for Windows users)

Also share 2–3 references for anyone who wants to go deeper on their own: **PortSwigger Web Security Academy** (free, beginner labs), **Cloudflare Learning Center** (short, clear explainers), **CryptoHack** (crypto practice sets).

---

## Week Overview

| Day | Theme | Core (40–60 min) | Stretch (optional, 20–30 min) |
| :--- | :--- | :--- | :--- |
| **1** | How the Web Actually Works | Client/server model, HTTP anatomy (methods, status codes, headers, cookies, sessions), statelessness. Browser DevTools (Network/Elements). | Run `curl -v` and match it to DevTools. Intercept a form submission and tamper with a cookie/value. |
| **2** | Web App Security Basics | Why apps trust input they shouldn't. Guided SQL-injection login bypass (`' OR true--`) on Juice Shop. What stops it (parameterized queries). Reflected XSS + IDOR walkthroughs. | Dump credentials via `UNION SELECT`, find the hidden scoreboard, or try a reflected XSS `alert(1)`. |
| **3** | Cryptography & the XOR Atom | Encoding vs. hashing vs. encryption. Symmetric vs. asymmetric. Why tiny keyspaces die — brute-force a single-byte XOR cipher. | Repeating-key XOR (Vigenère-style), or a CryptoHack "General" challenge. |
| **4** | Taste: Forensics | Open a `.pcap` in Wireshark. `Statistics → Conversations`, `Follow → TCP/HTTP Stream`. Pull a cleartext credential out of captured traffic. | Export an object from an HTTP stream and hash it; try a picoGym forensics challenge. |
| **5** | Networking & DNS Fundamentals | DNS as the internet's phonebook. TCP vs. UDP. Firewalls, VPNs, proxies (analogies). `dig`, `ping`, `nslookup -type=MX`. | Connect to a legit free/test VPN and compare your public IP before/after. |
| **6** | A Peek at Recon & Enumeration | Why recon comes before testing. Nmap basics (port scans, "open port"). Common ports (80/443/22). Passive recon via `crt.sh`. | Write a one-page "Intel Report" on the scan results. |
| **7** | Capstone | Guided walkthrough: recon → web vuln → crack the clue → forensics → flag. Hints every 10–15 min. | Solve with minimal hints, or pair up to help a stuck peer. |

---

## 📅 Detailed Daily Breakdown

### Day 1 — How the Web Actually Works

**Core Concepts**
* Client–server model: what happens between hitting Enter on a URL and pixels on screen
* HTTP request/response anatomy: methods (GET/POST), status codes (2xx/4xx/5xx), headers
* Cookies, sessions, and statelessness — how a site "remembers" you're logged in

**Hands-on**
* Open any website, open DevTools, and find: one request, its headers, and a cookie it sets
* View page source (`Ctrl+U`) vs. DevTools' Elements tab — discuss why they can differ
* Run `curl -v https://example.com` and match the output to the DevTools view
* Use DevTools (or Burp/ZAP) to resend a request with a modified header (e.g. `User-Agent: NJACK`)

**Mini-Challenge — "Scavenger Hunt"**
* On OWASP Juice Shop, find 3 things using only DevTools: a cookie name, a request that fails with an error, and one comment left in the page source. Submit via Google Form. No exploitation yet, just looking around.

**Resources**
* MDN: [An overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
* Chrome DevTools: [Network panel reference](https://developer.chrome.com/docs/devtools/network/)
* [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) — the practice site used all week
* PortSwigger Web Security Academy (Burp tutorial)

**Team Prep**
* Save one clean request/response dump for a 10-min "read the exchange" mini-eval
* Ensure Windows users have WSL or a terminal ready for `curl`

---

### Day 2 — Web App Security Basics

**Core Concepts**
* Why concatenating user input into a query is fatal — the app trusts input it shouldn't
* **SQLi**: a login form builds a query from your input without checking it. The classic bypass makes the `WHERE` clause always true and comments out the password check
* **XSS**: a page shows text you typed without checking it, so you can sneak in a script
* **Broken Access Control (IDOR)**: the app shows *your* data because you asked for `id=5`, but never checks whether `id=6` is also yours to see

**Hands-on**
* Run OWASP Juice Shop (`docker run --rm -p 3000:3000 bkimminich/juice-shop`)
* Try a normal wrong login, then send `'` to trigger an error, then `' OR true--` as the email to log in as admin — explain the resulting query line-by-line
* Walk through one classic XSS example with exact steps given (type a harmless script into a search box, see it "pop up")
* Try changing a number in a URL (like an order ID) to see if you can view something that shouldn't be yours

**Mini-Challenge**
* Complete the 3 guided exercises above and submit a screenshot of each result. This is about recognizing the pattern, not finding new vulnerabilities unaided.

**Resources**
* [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
* [OWASP Top 10](https://owasp.org/www-project-top-ten/) (just skim the summaries)
* [PortSwigger Web Security Academy](https://portswigger.net/web-security) — SQLi topic, great for later
* DVWA (for transparent source code viewing), Hacksplaining

**Team Prep**
* Stand up Juice Shop on the exact setup freshers will use and solve it start-to-finish yourself
* Have a hosted instance ready as a fallback for anyone who can't run Docker

**📝 Quiz — Day 1 & 2**: simple MCQs — "which of these is an XSS attempt?", "what does a 404 mean?", "what's a cookie used for?"

---

### Day 3 — Cryptography & the XOR Atom

**Core Concepts**
* Encoding (Base64/hex — reversible, no key) vs. hashing (MD5/SHA — one-way fingerprint) vs. encryption (AES/RSA — requires a key)
* Symmetric (same key both ways) vs. asymmetric (public/private key pair)
* Why websites never store your actual password, just a hash of it
* Why tiny keyspaces die: a 256-key XOR cipher is trivial to brute-force, while 256-bit AES is not

**Hands-on (CyberChef, no command line needed)**
* Base64-encode a word, then hash it — observe that Base64 reverses but the hash doesn't
* Hash the same word a few times (identical output); change one letter (hash changes completely)
* **XOR Break**: given a hex string XOR'd with a single byte, loop through all 256 keys (CyberChef's XOR brute-force recipe, or a 5-line Python script) to find the key that reveals `NJACK{...}`

**Mini-Challenge**
* Given 3 short hashed passwords (deliberately weak/common ones), recover the original word using CyberChef or a quick lookup, and explain in one line why it was easy

**Resources**
* [CyberChef](https://gchq.github.io/CyberChef/)
* [Cloudflare: What is encryption?](https://www.cloudflare.com/learning/ssl/what-is-encryption/)
* [Cloudflare: What is hashing?](https://www.cloudflare.com/learning/ssl/what-is-hashing/)
* CryptoHack (Intro/General sets), Computerphile hashing videos

**Team Prep**
* Run a script to generate the single-byte XOR challenge and save the secret key/answer
* Verify the solver script works on a clean machine

---

### Day 4 — Taste: Forensics

**Core Concepts**
* A `.pcap` is a saved recording of network traffic
* The forensics workflow: narrow first (`Statistics → Conversations`), then reconstruct (`Follow → TCP/HTTP Stream`)
* Cleartext protocols leak everything

**Hands-on**
* Open a provided, small `.pcap` file in Wireshark
* Filter by `http`, find a `POST` request (the login)
* Right-click → `Follow → HTTP Stream` to read the cleartext username/password

**Mini-Challenge**
* Export an object from the HTTP stream (`File → Export Objects → HTTP`) and hash it, or try a picoGym forensics challenge

**Resources**
* Wireshark Sample Captures, Wireshark User's Guide
* picoGym Forensics, CyberDefenders

**Team Prep**
* Curate a small, clean sample pcap containing an HTTP cleartext login (downloading a curated sample takes ~5 minutes vs. ~1 hour to record your own)
* Warn Windows users about Npcap installation in advance

---

### Day 5 — Networking & DNS Fundamentals ("The Phonebook")

**Core Concepts**
* DNS as the internet's phonebook: turns a domain name into an IP address, and roughly how that lookup travels through a chain of servers
* TCP vs. UDP: "reliable, ordered delivery" vs. "fire and forget"
* Firewalls, VPNs, and proxies — one analogy each: a firewall is a bouncer checking IDs at the door; a VPN is an armored, encrypted tunnel your traffic travels through; a proxy is a stand-in who fetches things on your behalf
* Why DNS answers aren't verified by default, and what that means conceptually (DNS spoofing, told as a story, not performed)

**Hands-on**
* Terminal: `dig example.com`, `ping example.com`, `traceroute example.com` for 3 favorite sites — note the IP returned and how long the round trip took
* Run `nslookup -type=MX <domain>` on one of them to see a different kind of phonebook entry — the mail server, not the website
* Study a provided, pre-made packet capture that shows a spoofed DNS response next to a normal one (a ready-made "evidence exhibit" — nobody generates this live). Circle the field that gives the fake one away

**Mini-Challenge**
* Connect to a legitimate free/test VPN of your choice and compare your public IP before and after on an IP-lookup site — a small, safe, immediately visible "proof it works" moment

**Resources**
* Cloudflare: [What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)
* Cloudflare: [What is a firewall?](https://www.cloudflare.com/learning/security/what-is-a-firewall/)
* `man dig`, `man ping`

**Team Prep**
* Prepare the "spoofed vs. normal" DNS pcap exhibit
* No heavy infrastructure needed — this is all terminal/browser-based

**📝 Quiz — Day 3, 4 & 5**: recall-focused MCQs — "what does hashing protect?", "what does a pcap capture?", "what does DNS do?"

---

### Day 6 — A Peek at Recon & Enumeration ("Mission Briefing")

**Core Concepts**
* Why professionals look before they touch anything: recon comes before any deeper testing
* A gentle intro to Nmap: what a port scan is, and what "open port" means
* **crt.sh**, a public, passive log of issued certificates — safe to browse because it only shows public records, never touches the target directly
* Common ports recap (80/443 web, 22 SSH, 21 FTP) — just enough to recognize them in output

**Hands-on**
* Run one simple Nmap scan against your provided practice VM only: `nmap -sV <practice-VM-IP>`. Read through the output line by line
* Cross-reference what you found against the "common ports" cheat sheet — guess what each open port is probably for before checking
* Look up the certificate history of a domain you already own (or that was provided for the course) on `crt.sh`

**Mini-Challenge — "File the Intel Report" (self-scored)**
* Turn your Nmap output into a one-page report: for each open port, name the service, guess what it's used for, and flag anything worth a closer look on Day 7. A model answer sheet is provided to self-grade. Badge: 100% correctly identified = "Field Analyst," partial = "Recruit"

**Resources**
* [Nmap Getting Started Guide](https://nmap.org/book/man.html)
* [crt.sh](https://crt.sh/) — only look up domains you own or that are provided for the course
* Cloudflare: Common Ports Cheat Sheet

**Team Prep**
* Spin up a safe, isolated practice VM with 2–3 obvious open ports (e.g. 80, 22, 8080)
* Provide a model "Intel Report" answer sheet for self-grading

---

### Day 7 — Capstone: Putting It All Together

**The Challenge** — a guided, connected walkthrough (not an unassisted attack chain) revisiting each layer of the week in order:

1. **Recon** the provided practice server using Nmap (Day 6) to see what's running
2. **Web**: visit its web app and recognize a deliberately-placed, simple vulnerability (Day 2 — e.g. an obvious XSS or an ID you can change in the URL) to find a clue
3. **Crypto**: decode/crack a weak hash or XOR cipher found via that clue using CyberChef/Python (Day 3)
4. **Forensics**: analyze a provided `.pcap` of the server's traffic to find a hidden secondary credential (Day 4)
5. **Victory**: submit the final flag via the Google Form/Discord bot for leaderboard points

Hints are available at every stage — the goal is everyone finishing and seeing how the pieces connect, not everyone finishing unaided.

**Tips for running it**
* Provide a hint after every 10–15 minutes of being stuck — this day is a victory lap, not a wall
* Keep the server low-stakes (an isolated VM, not production infra)
* Consider running it in pairs so beginners can talk through it together

**Resources**
* Cumulative links from Days 1–6, plus a "Where Next" handout: PortSwigger for web, CryptoHack for crypto, CyberDefenders for forensics

**Team Prep**
* Test the entire capstone chain end-to-end on a fresh machine
* Ensure hints are pre-written and ready to be drip-fed

**📝 Final Quiz (Day 1–7, cumulative, ~15–20 questions)**: a relaxed, timed Kahoot/Quizizz session mixing recall questions from each day — a fun send-off rather than a hard test

---

## Making It Interesting — General Tips

* **Daily recall quiz**: 5 quick questions at the *start* of each day testing yesterday's content, kept light
* **Running leaderboard**: quiz points + challenge points, tracked via a shared spreadsheet or bot — keep the tone playful

---

## Quick Reference: Skills Covered by End of Week

By Day 7, a participant should be able to:
* Explain how a webpage loads and read basic requests/cookies in DevTools
* Recognize XSS, SQL injection, and broken access control when they see an example of each
* Explain the difference between encoding, hashing, and encryption — and break a weak (single-byte XOR) cipher
* Open a `.pcap` in Wireshark and pull a cleartext credential out of captured traffic
* Explain what DNS, TCP/UDP, firewalls, VPNs, and proxies each do
* Run a basic Nmap scan and look up a domain's certificate history
* Follow a guided path connecting an app vulnerability to a network-level flag, understanding how each day's topic linked to the next
