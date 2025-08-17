
---

# OSINT (Open-Source Intelligence)

## What is OSINT?

**OSINT** = collecting, processing, and analyzing **publicly available information** to produce useful intelligence. Sources are “open” (websites, social media, public records, forums, archives, metadata, etc.), not hacked/paid/closed.

## Goals & who uses it

* Threat intelligence, incident response, red team reconnaissance, journalism, law enforcement, due diligence, corporate security.
* Users: security researchers, SOC teams, investigators, journalists, fraud analysts.

## Legal & ethical rules (must-know)

* Only collect from publicly available sources.
* Respect ToS and robots.txt where relevant.
* Don’t impersonate or access protected data without permission.
* Ask for authorization for intrusive research; document consent.

## OSINT process (high level)

1. **Planning** — define objective & scope, create search plan.
2. **Collection** — use search engines, APIs, tools to gather data.
3. **Processing** — normalize data (timestamps, formats), remove noise.
4. **Analysis** — correlate, timeline, attribute, link entities.
5. **Reporting/Dissemination** — prepare actionable findings, with provenance.

## Common OSINT techniques & examples

* **Google dorking** — advanced search operators (site:, filetype:, inurl:, intitle:).
  Example (benign):

  ```text
  site:gov.in filetype:pdf "budget"
  ```
* **Whois & DNS** lookups: discover registrant, nameservers, subdomains.
* **Passive footprinting**: Shodan/Censys for exposed services, passive DNS datasets.
* **Social media recon**: mining profiles, friends, posts, public photos.
* **Metadata extraction**: EXIF from images, PDF metadata (author, software).
* **Reverse image search**: find where an image appears online.
* **Archive search**: Wayback Machine for deleted pages.
* **Geolocation** from images / videos (landmarks, shadows, EXIF GPS).
* **Email harvesting**: public email discovery and validation (ethical usage).

## Useful OSINT tools (short list)

* `whois`, `dig`, `nslookup` (CLI)
* `theHarvester` — email & subdomain harvesting
* `Recon-ng` — recon framework
* `SpiderFoot` — automated OSINT scanning
* `Maltego` — visual link analysis (GUI)
* `Shodan`, `Censys` — internet-wide device search
* `exiftool` — metadata extraction
* `Google` / `Bing` / `DuckDuckGo` with advanced operators
* `Have I Been Pwned` — breached accounts

## Simple example workflow (investigate suspicious domain)

1. `whois suspicious.example` → get registrar, created/updated dates.
2. `dig +short suspicious.example A` → IP.
3. `shodan host <IP>` → check exposed services.
4. `theHarvester -d suspicious.example -b google` → harvest emails/subdomains.
5. `exiftool some_image.jpg` → check metadata if images provided.

---

# OSINT Framework

## What it is

A curated, categorized **map of OSINT tools and resources** (website that links tools by purpose), designed to help you pick the right tool quickly.

## How to use it

1. Open the framework and pick a category (e.g., *Domains*, *Email*, *Images*).
2. Click tools listed under the category and try the ones that fit your scope.
3. Combine outputs — e.g., domain → WHOIS → subdomains → Shodan → screenshots.

## Typical categories you’ll see

* Domains & DNS, People, Email, Social Media, Metadata, Geolocation, Images, Archives, Dark Web, Mobile.

---

# CyberDefenders

## What it is

A **blue-team / defensive** CTF-style platform providing real-world labs: DFIR, incident response, log analysis, threat hunting, malware analysis, and SOC workflows.

## Why it’s useful

* Practice defensive skills in realistic scenarios.
* Learn tools and workflows used in SOCs (SIEM, EDR, forensic triage).
* Great for resume-building and interviews.

## How to practice (high level)

* Choose a lab (e.g., incident: ransomware alert).
* Collect artifacts: timeline files, logs, PCAPs.
* Use tools: `grep`, `jq`, Wireshark, Volatility, Sysinternals (Windows), Elastic/Kibana.
* Document findings: indicators of compromise (IOCs), attack timeline, remediation steps.

---

# picoCTF

## What it is

Beginner-friendly CTF created by Carnegie Mellon — gamified puzzles to learn security basics (crypto, forensics, web, reversing, binary).

## Structure & categories

* **Web**, **Crypto**, **Forensics**, **Reverse**, **Binary**, **Misc**.
* Each challenge gives a flag string when solved.

## How to start (practical tips)

1. Register on picoCTF.
2. Start with “Intro” or “Crypto 101” problems.
3. Use basic tools: `file`, `strings`, `xxd`, `binwalk`, `gdb`, Python.
4. Read writeups after you solve or if stuck.

### Example small crypto trick (hex → ascii)

```bash
# hex to text
echo 68656c6c6f | xxd -r -p
# or (in Python)
python3 -c "print(bytes.fromhex('68656c6c6f').decode())"
# outputs: hello
```

---

# CTFtime

## What it is

Calendar & aggregator of CTF competitions. Use it to:

* Find upcoming CTFs, their difficulty, format (Jeopardy/Attack-Defense).
* See team rankings, event writeups and solutions.

## How to use

* Watch the event calendar.
* Join or form a team.
* Read post-event writeups to learn solving techniques.

---

# Hashing — SHA-256 and other hash functions

## What is hashing?

A **hash function** maps arbitrary data → fixed-length string (digest). Cryptographic hash functions are deterministic and (ideally) one-way.

## Important cryptographic properties

* **Preimage resistance:** given hash `h`, hard to find `m` such that `hash(m)=h`.
* **Second preimage resistance:** given `m1`, hard to find `m2 != m1` with same hash.
* **Collision resistance:** hard to find any `m1 != m2` with same hash.

## Common hash algorithms

| Algorithm    | Output length (hex) | Notes                                |
| ------------ | ------------------: | ------------------------------------ |
| MD5          |  32 chars (128 bit) | Fast, **broken** (collisions)        |
| SHA-1        |  40 chars (160 bit) | **Broken** (collisions demonstrated) |
| SHA-256      |  64 chars (256 bit) | Widely used, currently secure        |
| SHA-512      | 128 chars (512 bit) | Stronger, slower                     |
| SHA-3 family |            variable | Modern alternative                   |

## Why hashing matters (use cases)

* **Password storage** (with salt, and better: KDFs like bcrypt/Argon2).
* **File integrity** — verify downloads (checksums).
* **Digital signatures** — sign hash of content.
* **Blockchain** — linking blocks, proof-of-work.
* **Forensics** — identify identical files.

---

## Calculate hashes (practical commands)

### Linux CLI (Kali, Ubuntu, etc.)

```bash
# SHA-256 of a file
sha256sum file.iso
# SHA-1 of a file
sha1sum file.iso
# MD5 of a file
md5sum file.iso
# SHA-256 of a string
echo -n 'hello' | sha256sum
# Using openssl (also useful for HMAC)
echo -n 'hello' | openssl dgst -sha256
```

### HMAC (hash with secret key)

```bash
echo -n 'message' | openssl dgst -sha256 -hmac 'mysecretkey'
```

### Python examples

```python
# fast examples: hashlib
import hashlib
print(hashlib.sha256(b'hello').hexdigest())

# hash a file
h = hashlib.sha256()
with open('file.iso','rb') as f:
    for chunk in iter(lambda: f.read(8192), b''):
        h.update(chunk)
print(h.hexdigest())
```

---

## Password storage best practices (do NOT store raw hashes)

* **Never** store plain hashes for passwords (e.g., raw SHA-256) — too fast for cracking.
* Use **slow, memory-hard key derivation functions**:

  * **Argon2id** (recommended)
  * **bcrypt**
  * **scrypt**
  * **PBKDF2** with high iteration count (less ideal than Argon2)
* Always use a **per-user random salt** (16–32 bytes) stored alongside the hash.
* Consider **pepper** (a server-side secret) for extra protection.
* Set parameter values tuned to your environment (CPU/memory/cost).

### Example: PBKDF2 in Python

```python
import hashlib, binascii, os
pwd = b'mysecretpassword'
salt = os.urandom(16)
dk = hashlib.pbkdf2_hmac('sha256', pwd, salt, 200_000)  # iterations example
print(binascii.hexlify(dk).decode())
```

### Example: bcrypt (Python)

```python
import bcrypt
pw = b"password123"
salt = bcrypt.gensalt(rounds=12)
hashed = bcrypt.hashpw(pw, salt)
print(hashed)        # store this
bcrypt.checkpw(pw, hashed)  # verify
```

(Install: `pip install bcrypt`)

### Example: Argon2 (Python)

```python
from argon2 import PasswordHasher
ph = PasswordHasher()
hash = ph.hash("password123")
ph.verify(hash, "password123")
```

(Install: `pip install argon2-cffi`)

---

## Hints on integrity verification & signatures

* Use `sha256sum` to verify file checksums and compare to publisher-supplied hash.
* For strong authenticity, verify **digital signatures** (GPG/PGP) rather than only hashes.
* When downloading tools, prefer official signatures over raw checksums if available.

---

## Cracking & defense (high-level)

* Attackers use **rainbow tables**, **brute-force**, and **GPU tools** to crack weak hashes.
* **Speed matters**: MD5/SHA1/SHA256 are fast → bad for passwords unless combined with KDF.
* Defenders use salts, slow KDFs, rate-limiting, 2FA, and monitoring for credential abuse.

> ⚠️ **Don’t** use cracking techniques against accounts or systems you don’t own or lack authorization for. Always practice in lab environments or CTFs.

---

# Quick Recommendations / Cheatsheet

* For **file integrity**: use `sha256sum file`.
* For **password storage**: use **Argon2id** (with per-user salt).
* For **OSINT**: plan scope → pick tools (theHarvester, Shodan, exiftool, spiderfoot) → document provenance.
* For learning: start with **picoCTF** → read writeups on **CTFtime** → practice defensive labs on **CyberDefenders**.

---
