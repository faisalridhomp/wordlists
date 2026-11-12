# wordlists

Password and word lists for hash cracking and security research.

These are the lists used for dictionary attacks against hashes (SHA-256,
bcrypt, etc.). All lists are public, standard security-research wordlists —
nothing here is private data.

## Lists

| File | Entries | Description |
|------|---------|-------------|
| `rockyou.txt.gz` | 14,344,391 | rockyou — the classic leaked-password list (gzipped, 53 MB) |
| `ncsc-100k.txt` | 99,840 | UK NCSC top 100,000 most common passwords |
| `english-20k.txt` | 20,000 | 20,000 most common English words |
| `bip39-english.txt` | 2,048 | BIP39 English mnemonic wordlist |
| `indonesian-crypto.txt` | 322 | Indonesian common words + crypto/agent terms |
| `numbers-2m.txt` | 2,000,001 | Numbers 0–2,000,000 (numeric PINs / brute force) |

## Usage

```bash
# Decompress rockyou
gunzip -k rockyou.txt.gz

# Hashcat (SHA-256 example)
hashcat -m 1400 -a 0 hash.txt rockyou.txt

# John the Ripper
john --wordlist=rockyou.txt --format=Raw-SHA256 hash.txt

# Quick check with Python
python3 -c "
import hashlib
target = 'PUT_HASH_HERE'
for w in open('rockyou.txt', errors='ignore'):
    if hashlib.sha256(w.strip().encode()).hexdigest() == target:
        print('found:', w.strip()); break
"
```

## Notes

- `rockyou.txt.gz` is gzipped because the raw file (133 MB) exceeds
  GitHub's 100 MB file limit. Decompress before use.
- These lists only find passwords that are common, dictionary words, or
  simple numbers. A random or unique passphrase will not be in any list —
  that is expected; SHA-256 cannot be reversed, only guessed.
- Use responsibly and only on hashes you own or are authorized to test.
