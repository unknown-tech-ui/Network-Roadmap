# Scope & Ethics

This roadmap teaches offensive-security and networking *concepts* (HTTP inspection, XSS, SQL injection, XOR/hash cracking, packet forensics, DNS lookups, port scanning) using deliberately safe, purpose-built targets. Please read this before running or joining the course.

## What's in bounds

- **OWASP Juice Shop** — a legally provided, intentionally vulnerable practice app (Day 1–2, and reused in the Day 7 capstone)
- **CyberChef** — an in-browser tool used for encoding/hashing/XOR exercises, never touches a live target (Day 3)
- **A provided, curated sample `.pcap` file** — pre-captured traffic used for forensics practice; no live network is captured or intercepted (Day 4)
- **`dig`/`nslookup`/`ping`/`traceroute`** — passive, read-only lookups against domains that are already public information (Day 5)
- **A provided or your own test/free VPN** — used only to compare your own public IP before/after (Day 5, bonus)
- **crt.sh** — a public, passive log of already-issued certificates; only ever used to look up domains you own or that are explicitly provided for the course (Day 6)
- **The provided practice VM(s)** — isolated, disposable machines set up specifically for this course, targeted only with the provided Nmap scan (Day 6–7)

## What's out of bounds

- Running any scan, exploit, or credential-guessing attempt against **any system you do not own or have explicit written permission to test** — this includes real companies, public infrastructure, other participants' machines, or "just curious" targets outside the practice environment
- Using skills from this course against production systems, even ones you have partial access to (e.g. your employer's network) without formal authorization
- Attempting DNS spoofing, packet injection, or any active man-in-the-middle technique on a live/shared network — Day 5's spoofing content is a *story and pre-made evidence exhibit*, never performed live
- Sharing real credentials, real leaked hash dumps, real pcaps of real traffic, or real malware — all example hashes, XOR ciphers, and captures used in this course are synthetic/curated and provided for the exercises only

## Why this matters

Unauthorized scanning, exploitation, packet capture, or access — even "just testing," even against sites that turn out to be weakly defended — can be illegal under computer-misuse laws in most countries, regardless of intent. The whole point of this roadmap is to build the judgment to test *only* what you're authorized to test.

## If you fork or adapt this course

Please keep a version of this notice, and make sure any new "practice VM," pcap, or target you add is:
1. Isolated from production/real infrastructure,
2. Something you own or have explicit rights to use for this purpose, and
3. Clearly labeled as a training target to participants.
