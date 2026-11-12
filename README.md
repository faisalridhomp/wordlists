# wordlists

Password and word lists for hash cracking and security research.

These are the lists used for dictionary attacks against hashes (SHA-256,
bcrypt, etc.). All lists are public, standard security-research wordlists —
nothing here is private data.

## Lists

| File | Entries | Description |
|------|---------|-------------|
| `rockyou-part1.txt.gz` | 4,926,342 | rockyou, part 1 of 3 (gzipped, 18 MB) |
| `rockyou-part2.txt.gz` | 4,645,901 | rockyou, part 2 of 3 (gzipped, 17 MB) |
| `rockyou-part3.txt.gz` | 4,772,148 | rockyou, part 3 of 3 (gzipped, 18 MB) |
| `ncsc-100k.txt` | 99,840 | UK NCSC top 100,000 most common passwords |
| `english-20k.txt` | 20,000 | 20,000 most common English words |
| `bip39-english.txt` | 2,048 | BIP39 English mnemonic wordlist |
| `indonesian-crypto.txt` | 322 | Indonesian common words + crypto/agent terms |
| `numbers-2m.txt` | 2,000,001 | Numbers 0–2,000,000 (numeric PINs / brute force) |

## Usage

```bash
# Decompress rockyou (all three parts, 14,344,391 passwords total)
gunzip -k rockyou-part1.txt.gz rockyou-part2.txt.gz rockyou-part3.txt.gz
cat rockyou-part1.txt rockyou-part2.txt rockyou-part3.txt > rockyou.txt

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

- `rockyou` is split into 3 gzipped parts because GitHub's web upload
  rejects files over 25 MB (and the raw file, 133 MB, exceeds GitHub's
  100 MB file limit). Decompress and concatenate all three to rebuild it.
- These lists only find passwords that are common, dictionary words, or
  simple numbers. A random or unique passphrase will not be in any list —
  that is expected; SHA-256 cannot be reversed, only guessed.
- Use responsibly and only on hashes you own or are authorized to test.
