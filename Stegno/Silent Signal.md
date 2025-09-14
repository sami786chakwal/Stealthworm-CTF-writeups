# Challenge: Silent Signal
**Category:** Steganography / Audio (Morse Code)  
**Points:** 500  
**Author:** Sami Choudhary  

**Challenge statement:**  
> Something unusual is hidden in the static. The transmission seems meaningless at first—just a series of short and long pulses. But if you listen with the right ears, the silence begins to speak.  
>
> Can you uncover what the signal is trying to tell you?

---

## Files provided
- `SilentSignal.zip` — archive containing:
  - `decode.wav` (audio file containing encoded message)  
  - `flag.txt` (helper hint file with partial info: `SW{itna_bhi_asan_nahi_ha}` and note about 750 Hz)

---

## Reproduction (what players see)
Extract the archive:

```bash
unzip SilentSignal.zip -d silent_signal
cd silent_signal
ls
# decode.wav  flag.txt
The flag.txt gives a misleading placeholder (SW{itna_bhi_asan_nahi_ha}) but also contains a hint about using frequency 750 Hz.

Quick solution (one-line)
The .wav file contains Morse code at ~750 Hz. Decoding it yields the string:

nginx
Copy code
RIPH4HMAH4CKERBNAG4
So the flag is:

Copy code
SW{RIPH4HMAH4CKERBNAG4}
Step-by-step analysis & solution
1. Inspect the audio file
Check the file metadata:

bash
Copy code
$ file decode.wav
decode.wav: RIFF (little-endian) data, WAVE audio
Play it:

bash
Copy code
$ play decode.wav
You will hear a sequence of beeps (short and long tones), typical of Morse code.

2. Use frequency hint (750 Hz)
The flag.txt mentions 750 Hz — that’s the frequency of the beeps. This confirms Morse encoding.

3. Decode Morse code
There are multiple ways to decode:

a. Online Morse decoder
Upload the .wav to an online tool such as Morse Code World decoder.
Make sure to set tone frequency to ~750 Hz for proper decoding.

b. Local tools
You can use morse-audio-decoder or minimodem:

bash
Copy code
minimodem --rx morse -f 750 < decode.wav
4. Output
Decoding gives the following text:

nginx
Copy code
RIPH4HMAH4CKERBNAG4
5. Final Flag
The decoded string is wrapped in the flag format:

Copy code
SW{RIPH4HMAH4CKERBNAG4}
