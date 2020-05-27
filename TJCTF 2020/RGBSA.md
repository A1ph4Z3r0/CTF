# RGBSA

## Problem

I love me some pixel art! Peep the red channels!

Flag is in flag{} format

Written by boomo

[Challenge File](./files/gifs.zip)

## Solution

We are given three files, `n.gif`, `e.gif` and `c.gif`. Based on the title of the challenge, we can assume that it is referring to a textbook RSA Challenge with the values n, e and c hidden in the gifs. 

The challenge hint stated that there are 76 keys, and the gifs contained 76 frames. So we need to extract a number from each frame of each gif. Also the challenge title says peep the red channels, so we need to extract it from the red channel. 

We can use imagemagick's command line tool `convert` to get the individual frames from the gifs. 

```bash
convert e.gif e.png
convert n.gif n.png
convert c.gif c.png
```

Then we can use PIL and python to do the rest. The numbers were hidden in the least significant bit (last bit in binary) of the red channel. 

That is, you need to take the red values of each pixel converted them to binary and save the last bit. Then you join them all together and convert it back to an integer.

```python
from PIL import Image

n = []
for i in range(76):
    im = Image.open('n-' + str(i) + '.png').convert('RGB')
    n.append(int(''.join([bin(v)[-1] for v in list(im.getchannel(0).getdata())]), 2))

e = []
for i in range(76):
    im = Image.open('e-' + str(i) + '.png').convert('RGB')
    e.append(int(''.join([bin(v)[-1] for v in list(im.getchannel(0).getdata())]), 2))

c = []
for i in range(76):
    im = Image.open('c-' + str(i) + '.png').convert('RGB')
    c.append(int(''.join([bin(v)[-1] for v in list(im.getchannel(0).getdata())]), 2))
```

Now we have a list of all the values of n, e and c. The hint gives us the first value of e and we can check if they are the same. 
`assert(e0 == e[0])`

From here on, its simply an RSA challenge. Taking a closer look we can see that each key has the same n value. The value of e and c is different however. 
The hint says the same message is encoded each time. This is starting to smell a lot like a common modulus attack.

```python
from Crypto.Util.number import *

def findIndex():
	for i in range(len(e)):
		for j in range(len(e)):
			if GCD(e[i], e[j]) == 1:
				return i, j

i, j = findIndex()

e1 = e[i]
c1 = c[i]
e2 = e[j]
c2 = c[j]

a = inverse(e1, e2)
b = (GCD(e1, e2) - (a * e1))//e2
assert((a * e1 + b * e2) == 1)

N = n[0]

i = inverse(c2, N)
mx = pow(c1, a, N)
my = pow(i, -1 * b, N)
m = mx * my % N

print(long_to_bytes(m))
```

Flag: **flag{excitable_illumination_wanderer_}**
