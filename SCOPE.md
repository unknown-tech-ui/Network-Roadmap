# Scope & Ethics

This roadmap teaches offensive-security *concepts* (scanning, XSS, SQL injection, hash cracking, DNS lookups) using deliberately safe, purpose-built targets. Please read this before running or joining the course.

## What's in bounds

- **OWASP Juice Shop** — a legally provided, intentionally vulnerable practice app (Day 1–2)
- **badssl.com** — a site built specifically to safely demonstrate broken TLS configurations (Day 4)
- **crt.sh** — a public, passive log of already-issued certificates; only ever used to look up domains you own or that are explicitly provided for the course (Day 6)
- **The provided practice VM(s)** — isolated, disposable machines set up specifically for this course (Day 6–7)
- **A provided or your own test/free VPN** — used only to compare your own public IP before/after (Day 5, bonus)
- Passive, read-only lookups (`nslookup`, `dig`, certificate inspection) against domains that are already public information

## What's out of bounds

- Running any scan, exploit, or credential-guessing attempt against **any system you do not own or have explicit written permission to test** — this includes real companies, public infrastructure, other participants' machines, or "just curious" targets outside the practice environment
- Using skills from this course against production systems, even ones you have partial access to (e.g. your employer's network) without formal authorization
- Sharing real credentials, real leaked hash dumps, or real malware — all example hashes and vulnerable code in this course are synthetic and provided for the exercises only

## Why this matters

Unauthorized scanning, exploitation, or access — even "just testing," even against sites that turn out to be weakly defended — can be illegal under computer-misuse laws in most countries, regardless of intent. The whole point of this roadmap is to build the judgment to test *only* what you're authorized to test.

## If you fork or adapt this course

Please keep a version of this notice, and make sure any new "practice VM" or target you add is:
1. Isolated from production/real infrastructure,
2. Something you own or have explicit rights to use for this purpose, and
3. Clearly labeled as a training target to participants.
