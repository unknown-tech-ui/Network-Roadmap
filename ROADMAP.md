## Before Day 1 — Your Setup Checklist

You'll need **VMware or VirtualBox**, with Kali Linux or Ubuntu installed.

Keep a few references handy for whenever you want to go deeper on your own: **PortSwigger Web Security Academy** (free, beginner labs), **Cloudflare Learning Center** (short, clear explainers), and **badssl.com** (fun visual examples of broken TLS).

---

## Day 1 — How the Web Actually Works

**What you're learning today**

* The client–server model: what happens between hitting Enter on a URL and pixels showing up on your screen
* HTTP basics: what a request and response look like, common methods (GET/POST), and status codes (200, 404, 500)
* Cookies and sessions — how a website "remembers" that you're logged in
* Your first look at browser DevTools: the Network tab (to see requests) and the Elements tab (to see the page's HTML)

**What you're doing hands-on**

* Open any website, open DevTools, and follow along to find: one request, its headers, and a cookie it sets.
* View a page's source (`Ctrl+U`) and compare it to what's shown in DevTools' Elements tab, and think about why they can differ.

**Your mini-challenge — "Scavenger Hunt"**

On OWASP Juice Shop, find 3 specific things using only DevTools: a cookie name, a request that fails with an error, and one comment left in the page source. Submit your answers via the Google Form. No exploitation yet — just looking around.

**📚 Resources you can use**

* MDN: [An overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
* [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) — the practice site you'll use all week
* Chrome DevTools: [Network panel reference](https://developer.chrome.com/docs/devtools/network/)

---

## Day 2 — Introduction to Web App Security

**What you're learning today**

* What "web app security" means, in plain terms: the app trusts input it shouldn't
* Three common issues, starting with simple examples before you see a demo:

  * **XSS** — a page shows text you typed without checking it, so you can sneak in a script
  * **SQL Injection** — a login form builds a database query from your input without checking it
  * **Broken Access Control (IDOR)** — the app shows *your* data because you asked for `id=5`, but never checks if `id=6` is also yours to see
* Why input validation matters as the fix, in one sentence per issue

**What you're doing hands-on (guided, step-by-step)**

* Walk through one classic XSS example on Juice Shop with exact steps given: type a specific harmless script into a search box and watch it "pop up."
* Walk through one classic login-bypass SQLi example with exact steps given.
* Try changing a number in a URL (like an order ID) to see if you can view something that shouldn't be yours.

**Your mini-challenge**

Complete the 3 guided exercises above and submit a screenshot of each result. This is about recognizing the pattern, not finding new vulnerabilities unaided.

**📚 Resources you can use**

* [OWASP Top 10](https://owasp.org/www-project-top-ten/) (just skim the summaries for now)
* [PortSwigger Web Security Academy](https://portswigger.net/web-security) — beginner-level guided labs, great for later
* Juice Shop's own [hint system](https://pwning.owasp-juice.shop/) if you get stuck

**📝 Quiz — Day 1 & 2**

Simple MCQs: "which of these is an XSS attempt?", "what does a 404 mean?", "what's a cookie used for?"

---

## Day 3 — Cryptography Basics: Keeping Secrets and Proving Trust

**What you're learning today**

* Encryption vs. hashing, explained simply: encryption can be undone (with a key), hashing can't be undone
* Symmetric (same key both ways) vs. asymmetric (public/private key pair) — a simple analogy for each
* Why websites never store your actual password, just a hash of it
* What a "digital signature" proves, in one sentence: this came from who it says, and wasn't changed

**What you're doing hands-on (using CyberChef — no command line needed)**

* Encrypt then decrypt a short message using a symmetric cipher in CyberChef.
* Hash the same word a few times and see the hash is always identical; change one letter and see the hash change completely.
* Look up a couple of very weak, common passwords in an MD5 hash chart/CyberChef to see how quickly simple ones can be reversed — a reminder of why strong, unique passwords matter.

**Your mini-challenge**

Given 3 short hashed passwords (all very weak/common ones on purpose), figure out the original word using CyberChef or a quick online lookup, and explain in one line why it was easy.

**📚 Resources you can use**

* [Cloudflare: What is encryption?](https://www.cloudflare.com/learning/ssl/what-is-encryption/)
* [CyberChef](https://gchq.github.io/CyberChef/)
* [Cloudflare: What is hashing?](https://www.cloudflare.com/learning/ssl/what-is-hashing/)

---

## Day 4 — Web PKI & TLS: "The Case of the Fake Padlock" 🔍

**Today's framing**: you're a certificate detective. Every website's padlock is a claim — "trust me, I am who I say I am" — and your job today is learning to check that claim instead of blindly trusting it. This is fully self-paced; every exercise below is self-checkable.

**What you're learning today**

* What the padlock icon actually promises: this connection is encrypted, and you're really talking to who you think you are
* Certificates in plain terms: a trusted third party (a Certificate Authority) vouches that "this website's key really belongs to this domain"
* A simplified walk-through of the TLS handshake, as a "secret handshake between two spies" — they exchange a few signals, agree on a shared secret code, and *then* start whispering
* What can go wrong: expired certificates, self-signed certificates, mismatched domain names — and why your browser reacts loudly when it spots one

**What you're doing hands-on — "Build Your Own Cert Inspector Checklist"**

1. Pick any 3 real websites you use daily. Click the padlock icon on each and note: who issued the certificate, who it's issued to, and its expiry date.
2. Turn that into a personal 4-point checklist you can reuse forever: *Issuer trusted? Domain matches? Not expired? Browser shows no warning?*
3. Visit **badssl.com** — a site built specifically to safely demonstrate what broken certificates look like. Work through its "expired," "self-signed," and "wrong host" example pages and run your checklist against each one. Every page tells you exactly what's wrong with it, so you can self-check your answers as you go.

**Your mini-challenge — "Spot the Fake" (self-scored)**

Using badssl.com's example pages, log which ones your checklist correctly flagged as unsafe, and which real-world sites correctly passed. Score yourself: 4/4 checklist points caught = "Certificate Detective," 2–3 = "Apprentice," under 2 = worth a re-read of today's topics.

*Bonus round*: capture one HTTPS request from a real site in Wireshark and confirm the payload is unreadable, then compare it against an HTTP-only capture from Day 1/2 where everything was in plaintext. Screenshot both side by side as your "evidence board."

**📚 Resources you can use**

* [badssl.com](https://badssl.com/) — a safe, purpose-built practice ground
* Cloudflare: [What happens in a TLS handshake?](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)
* [SSL Labs test tool](https://www.ssllabs.com/ssltest/) — paste in any site and get an instant, plain-English grade of its TLS setup

---

## Day 5 — Networking & DNS: "The Phonebook Heist" 📞

**Today's framing**: every website visit starts with a lookup in the internet's phonebook — DNS. Today you follow that phonebook entry on its journey, and ask: what would it mean if someone tampered with a page in that phonebook before it reached you? Everything below uses only public, passive lookups on domains that are already public information — you don't intercept traffic or touch anyone else's system.

**What you're learning today**

* DNS in plain terms: the "phonebook" that turns a domain name into an IP address, and roughly how that lookup travels through a chain of servers
* Why DNS answers aren't checked/verified by default, and what that means at a conceptual level (DNS spoofing, as a story — you don't perform it)
* Firewalls, VPNs, and proxies — three different jobs, each with one clear analogy: a firewall is a bouncer checking IDs at the door; a VPN is an armored, encrypted tunnel your traffic travels through; a proxy is a stand-in who fetches things on your behalf
* How the whole week connects: an app (Day 1–2) lives at an address found via DNS (today), reached over a network (today), protected in transit by TLS/PKI (Day 4), built on crypto primitives (Day 3)

**What you're doing hands-on — "Trace the Lookup"**

1. Pick 3 of your favorite websites. Run `nslookup <domain>` (or `dig <domain>`) for each and note the IP address returned and how long the whole trip took.
2. Run `nslookup -type=MX <domain>` on one of them and see a *different* kind of phonebook entry — the mail server for that domain, not the website.
3. Study a provided, pre-made packet capture that shows what a spoofed DNS response looks like next to a normal one. Circle the field that gives the fake one away.

**Your mini-challenge — "Bouncer, Tunnel, or Stand-In?" (self-scored)**

Connect to a service (SMTP/SSH) that returns a flag once you've enumerated it and connected correctly.

*Bonus*: connect to a legitimate free/test VPN of your choice and compare your public IP before and after on an IP-lookup site — a small, safe, and immediately visible "proof it works" moment.

**📚 Resources you can use**

* Cloudflare: [What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)
* Cloudflare: [What is a firewall?](https://www.cloudflare.com/learning/security/what-is-a-firewall/)
* [VPN Explained — PowerCert Animated Videos (YouTube)](https://www.youtube.com/watch?v=R-JUOpCgTZc)

**📝 Quiz — Day 3, 4 & 5**

MCQs: "what does hashing protect?", "what's wrong with this certificate?", "what does DNS do?" — recall-focused, no scenario puzzles.

---

## Day 6 — A Peek at Recon: "Mission Briefing" 🕵️

**What you're learning today**

* Why professionals look before they touch anything: recon comes before any deeper testing
* A gentle intro to Nmap: what a port scan is, and what "open port" means in plain terms
* **crt.sh**, a public, passive log of issued certificates (tying back to Day 4) — safe to browse because it only shows public records, and you never touch the target directly
* A short recap of common ports (80/443 for web, 22 for SSH) — just enough to recognize them in output

**What you're doing hands-on — "Intel Report on Your Practice VM"**

1. Run one simple Nmap scan against your provided practice VM only: `nmap -sV <practice-VM-IP>`. Read through the output line by line — every open port and the service guessed for it.
2. Cross-reference what you found against the "common ports" cheat sheet — try to guess, before checking, what each open port is probably for.
3. Look up the certificate history of a domain **you already own or that was provided for the course** on `crt.sh` — a completely passive, read-only lookup of public records.

**Your mini-challenge — "File the Intel Report" (self-scored)**

Turn your Nmap output into a one-page report: for each open port, name the service, guess what it's used for, and flag anything that looks worth a closer look on Day 7. Compare your report against the model answer sheet to self-grade — the goal is a complete, accurate report, not speed.

Badge system: 100% of ports correctly identified = "Field Analyst," partial = "Recruit" — either way, you're ready for the capstone.

**📚 Resources you can use**

* [Nmap Getting Started Guide](https://nmap.org/book/man.html)
* [crt.sh](https://crt.sh/) — only look up domains you own or that are provided for the course
* [Common Ports Cheat Sheet (Cloudflare)](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/)

---

## Day 7 — Capstone: Putting It All Together

**Your challenge** — a guided, connected walkthrough (not an unassisted attack chain) that revisits each layer from the week, in order:

1. **Recon** a provided practice server using Nmap (Day 6) to see what's running.
2. **Visit its web app** and recognize a deliberately-placed, simple vulnerability from Day 2 (like an obvious XSS or an ID you can change in the URL) to find a clue.
3. **Decode/crack** a weak hash found via that clue using CyberChef (Day 3 skills).
4. **Check its certificate** using what you learned on Day 4, and note whether the connection is trustworthy.
5. **Connect via the provided VPN and SSH in** using the credential you recovered in step 3, to find the final flag.
6. Submit the final flag via the Google Form/Discord bot for leaderboard points.

**📝 Final Quiz (Day 1–7 cumulative, ~15–20 questions)**

A relaxed, timed Kahoot/Quizizz session mixing recall questions from each day — a fun send-off rather than a hard test.

---

## How Your Week Fits Together

* Each day starts with a quick 5-question recall quiz on yesterday's content, kept light.
* You earn quiz points and challenge points toward a running leaderboard as you go.

---

## What You Can Do By Day 7

* Explain how a webpage loads and read basic requests/cookies in DevTools
* Recognize XSS, SQL injection, and broken access control when you see an example of each
* Explain the difference between encryption and hashing, and why hashed+salted passwords matter
* Explain what a certificate and the padlock icon actually guarantee, and spot an invalid certificate
* Explain what DNS, firewalls, VPNs, and proxies each do
* Run a basic Nmap scan and look up a domain's certificate history
* Follow a guided path connecting an app vulnerability to a network-level flag, understanding how each day's topic linked to the next
