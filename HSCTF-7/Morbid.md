# Morbid

## Problem

You found a message, but you're not sure what it could mean...

118289293938434193849271464117429364476994241473157664969879696938145689474393647294392739247721652822414624317164228466

Note: the flag should be two short words, separated by an underscore (ex. flag{f4k3_fl4g}). The flag is in the message, but the message itself is not the flag.

Author: AC

## Solution

Brute force the key for the morbit cipher, a common misconception with the cipher is that there are 26^9 keys to brute force. Taking a closer look at the cipher it only uses the relative position of the alphabet to do 9 morse substitutions. Therefore, we only need to brute force 9! keys.


Script I had written for IJCTF, when I authored similar challenge ;)

```python
from itertools import permutations

possible = set()

MORSE_CODE_DICT = {'..-': 'U', '--..--': ', ', '....-': '4', '.....': '5', '-...': 'B', '-..-': 'X', '---...':':', '.-.': 'R', '--.-': 'Q', '--..': 'Z', '.--': 'W', '-..-.': '/', '..---': '2', '.-': 'A', '..': 'I', '-.-.': 'C', '..-.': 'F', '---': 'O', '-.--': 'Y', '-': 'T', '.': 'E', '.-..': 'L', '...': 'S', '-.--.-': ')', '..--..': '?', '.----': '1', '-----': '0', '-.-': 'K', '-..': 'D', '----.': '9', '-....': '6', '.---': 'J', '.--.': 'P', '.-.-.-': '.', '-.--.': '(', '--': 'M', '-.': 'N', '....': 'H', '---..': '8', '...-': 'V', '--...': '7', '--.': 'G', '...--': '3', '-....-': '-', '\n' : ' ', '..--.-':'_'}
MORBIT = ['..', '.-', '. ', '-.', '--', '- ', ' .', ' -', '  ']

#Double-space is the word delimiter
def morse(ciphertxt):
     if ' ' not in ciphertxt:
          return
     
     plaintxt = ''
     for word in ciphertxt.strip().split("  "):
          for c in word.strip().split(" "):
               if c in MORSE_CODE_DICT:
                    plaintxt += MORSE_CODE_DICT[c]
               else:
                    return
          plaintxt += ' '
     print('[+] ' + plaintxt)

def morbit(ciphertxt):
     for p in permutations('123456789'):
          MORBIT_CODE_DICT = dict(zip(p, MORBIT))
          morsetxt = ""
          for c in ciphertxt:
               if c in MORBIT_CODE_DICT:
                    morsetxt += MORBIT_CODE_DICT[c]
               else:
                    return
          morse(morsetxt)

i = input()
morbit(i)
```

```
[+] CONGRATULATIONS. PLEASE WRAP THIS MESSAGE IN A FLAG FORMAT: M0R3_B1T5
```

Flag: **flag{M0R3_B1T5}**