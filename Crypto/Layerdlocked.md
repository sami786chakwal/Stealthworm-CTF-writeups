# Challenge: LayeredLocked
**Category:** Cryptography / Multi-layer (archive + password + hash)  
**Points:** 250 
**Author:** Sami Choudhary

**Challenge statement:**  
> You’ve stumbled upon a mysterious archive named `Layeredlocked.zip`. It won’t give up its secrets so easily.  
> Flag Format: `SW{some_thing}`

---

## Files provided
- `Layeredlocked.zip` — password-protected ZIP archive

---

## Reproduction (what players see)
Trying to open the ZIP normally fails because it is password protected.

---

## Quick solution (one-line)
Use a wordlist to crack the ZIP password (`abc123`), extract `raw.txt` which contains a hash, then crack that hash (via CrackStation / online or local cracking) to recover `p@ssw0rd!`. The flag is:

SW{p@ssw0rd!}

yaml
Copy code

---

## Step-by-step analysis & solution

### 1. Confirm the archive
Check the file type:

```bash
$ file Layeredlocked.zip
Layeredlocked.zip: Zip archive data, at least v2.0 to extract
2. Crack the ZIP password (layer 1)
The ZIP is password-protected. Use john (or fcrackzip / 7z + wordlist) with rockyou.txt to bruteforce:

Example using fcrackzip:

bash
Copy code
fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt Layeredlocked.zip
# Found password: abc123
Example using john (create a zip hash first using zip2john):

bash
Copy code
zip2john Layeredlocked.zip > zip.hash
john --wordlist=/usr/share/wordlists/rockyou.txt zip.hash
# john shows password: abc123
3. Extract archive with discovered password
Use the found password to extract:

bash
Copy code
unzip -P abc123 Layeredlocked.zip -d layeredlocked_extracted
# or
7z x -pabc123 Layeredlocked.zip -olayeredlocked_extracted
You should get a file named raw.txt (or similar).

4. Inspect raw.txt — it contains a hash
Open raw.txt:

bash
Copy code
$ cat layeredlocked_extracted/raw.txt
# (shows a hash string, e.g. something like: 5f4dcc3b5aa765d61d8327deb882cf99)
(Exact hash value will depend on the challenge; the file contained a crackable hash.)

5. Crack the hash (layer 2)
Use an online service (CrackStation) or local cracking tools. Example with an online lookup (CrackStation) or hashcat/john locally:

Quick online lookup (CrackStation): paste the hash → discovered plaintext: p@ssw0rd!

Local example with john (if hash type known — here we show a generic flow):

bash
Copy code
# if hash type is MD5:
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 raw.txt

# if hash type unknown, try hash-identifier or use hashcat with -m auto (or try common types)
hash-identifier layeredlocked_extracted/raw.txt
Result: the hash resolves to p@ssw0rd!.

6. Final flag
The cracked password is the flag payload:

css
Copy code
SW{p@ssw0rd!}
