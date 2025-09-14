# Challenge: The Minions
**Category:** Steganography  
**Points:** 250  
**Author:** Sami Choudhary

**Challenge statement:**  
> The minions have hidden a flag inside `Minions.jpeg` using steganography. Clues in the image’s metadata may reveal the passphrase needed to extract it. Can you uncover their secret?

---

## Files provided
- `Minions.jpeg` — image containing hidden data (steg)
- (After extraction) `flag.txt` — base64-encoded flag

---

## Reproduction (what players see)
Open the image or run quick forensics tools:

```bash
$ file Minions.jpeg
Minions.jpeg: JPEG image data, JFIF standard 1.01, ...
A quick metadata check reveals useful keywords/clues.

Quick solution (one-line)
Inspect metadata with exiftool to find the keyword my admin (used as a passphrase), use steghide to extract flag.txt, then base64-decode the file to reveal the flag SW{this_is_fun}.

Step-by-step analysis & solution
1. Inspect metadata with exiftool
Check image metadata to look for hints (keywords, comments, author, etc.):

bash
Copy code
$ exiftool Minions.jpeg
# ... output ...
# Keywords                        : my admin
# Comment                         : ...
The Keywords (or another metadata field) contains the passphrase: my admin.

2. Use steghide to extract data (use the passphrase)
Use the discovered passphrase to extract the hidden file:

bash
Copy code
$ steghide extract -sf Minions.jpeg
Enter passphrase: my admin
wrote: "flag.txt"
This creates flag.txt in the current directory.

3. Inspect and decode flag.txt
Open flag.txt — it contains base64 text:

bash
Copy code
$ cat flag.txt
U1d7dGhpcy5pc19mdW58Cg==
Decode it:

bash
Copy code
$ base64 -d flag.txt
SW{this_is_fun}
The flag is:

Copy code
SW{this_is_fun}
Optional: alternative tools / checks
If steghide fails, try other tools (binwalk, zsteg for PNGs, or foremost) — but here steghide with passphrase from metadata is the correct method.

Always inspect metadata first (exiftool) — flags or passphrases are often hidden there as hints.