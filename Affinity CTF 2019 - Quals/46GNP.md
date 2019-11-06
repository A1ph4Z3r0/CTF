# 46GNP

## Problem
Sophisticated malware got into our server and malformed all the images. Can you find the name of that bug?
![Malformed Image](./files/46gnp.png)

## Solution

We didn't end up solving this challenge during the competition, but here is the solution...

From the name of the challenge, you get clues to using Base64 and reversing the file. 
So after a bit of trial and error, this command will print the flag.

```sh
$ base64 46gnp.png | rev | base64 -d | strings | grep -o AFFCTF{.*}
AFFCTF{Ju$t_r3v!}
```
