# Reflection

## Problem
![gif](./files/task.gif)

## Solution

This was a forensics challenge so we try to look for embedded files,

```sh
$ binwalk -e task.gif

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             GIF image data, version "89a", 480 x 480
285539        0x45B63         gzip compressed data, from Unix, NULL date (1970-01-01 00:00:00)
285695        0x45BFF         GIF image data, version "89a", 540 x 283
491472        0x77FD0         gzip compressed data, from Unix, NULL date (1970-01-01 00:00:00)

$ cd _task.gif.extracted/
$ strings * | grep AFFCTF{.*}
AFFCTF{m@k3s_y0u__th0nk}
AFFCTF{m@k3s_y0u__th0nk}
```

Flag: **AFFCTF{m@k3s_y0u__th0nk}**
