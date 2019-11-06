# Breaking Bad

## Problem

HoRfSbMtInMcLvFlAcAmInMcAmTeErFmInHoLvDbRnMd

Note: put flag into AFFCTF{} format.

## Solution

Originally, the text looks like Base64 encoding, but the alternate capitalization of the letters suggests it might be something else.

The title of the challenge references that it might be chemistry related. The we notice that each group of 2 characters is an element in the periodic table.
After some googling, we find the `Periodic Table Cipher`.

In our instance, it appeared that the elements atomic numbers were decimal representation for the ASCII characters in the flag. 

```python
from mendeleev import element

cipher = 'HoRfSbMtInMcLvFlAcAmInMcAmTeErFmInHoLvDbRnMd'
cipher = [cipher[i:i+2] for i in range(0, len(cipher), 2)]

flag = ''

for name in cipher:
    flag += chr(element(name).atomic_number)

print 'AFFCTF{%s}' % flag
```

Flag: **AFFCTF{Ch3m1strY_1s_4Dd1CtiVe}**
