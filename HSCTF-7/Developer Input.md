# Developer Input

## Problem

I asked the developers of CTFd about their opinion on HSCTF. Can you decode their input?

Author: AC

[Challenge File](./files/DeveloperInput)

## Solution

The file is a log of output from `\dev\input`

Basically key logs. Required a bit of research of the format in which the data is encoded. 

https://thehackerdiary.wordpress.com/2017/04/21/exploring-devinput-1/

Quick script to get our flag

```python
import struct

f = open("DeveloperInput", "rb")

KEYS = {"KEY_RESERVED":0, "KEY_ESC":1, "KEY_1":2, "KEY_2":3, "KEY_3":4, "KEY_4":5, "KEY_5":6, "KEY_6":7, "KEY_7":8, "KEY_8":9, "KEY_9":10, "KEY_0":11, "KEY_MINUS":12, "KEY_EQUAL":13, "KEY_BACKSPACE":14, "KEY_TAB":15, "KEY_Q":16, "KEY_W":17, "KEY_E":18, "KEY_R":19, "KEY_T":20, "KEY_Y":21, "KEY_U":22, "KEY_I":23, "KEY_O":24, "KEY_P":25, "KEY_LEFTBRACE":26, "KEY_RIGHTBRACE":27, "KEY_ENTER":28, "KEY_LEFTCTRL":29, "KEY_A":30, "KEY_S":31, "KEY_D":32, "KEY_F":33, "KEY_G":34, "KEY_H":35, "KEY_J":36, "KEY_K":37, "KEY_L":38, "KEY_SEMICOLON":39, "KEY_APOSTROPHE":40, "KEY_GRAVE":41, "KEY_LEFTSHIFT":42, "KEY_BACKSLASH":43, "KEY_Z":44, "KEY_X":45, "KEY_C":46, "KEY_V":47, "KEY_B":48, "KEY_N":49, "KEY_M":50, "KEY_COMMA":51, "KEY_DOT":52, "KEY_SLASH":53, "KEY_RIGHTSHIFT":54, "KEY_KPASTERISK":55, "KEY_LEFTALT":56, "KEY_SPACE":57, "KEY_CAPSLOCK":58, "KEY_LEFT":105}
KEYS = {v: k for k, v in KEYS.items()}

key_codes = []

while 1:
     try:
          data = struct.unpack('4IHHI',f.read(24))
     except:
          break
     if data[4] == 1 and data[6] == 1:
          key_codes.append(KEYS[data[5]])    

flag = ""

for v in key_codes:
     if v == "KEY_SPACE":
          flag += " "
     elif(len(v) == 5):
          flag += v[4:]

print(flag)
```

```
EVALUATE THE LINE INTEGRAL WHERE C IS THE GIVEN CURVE WERE INTEGRATING OVER THE CURVE C Y TO THE THIRD DS AND C IS THE CURVE WITH PARAMETRIC EQUATIONS X  4 CUBED Y  4 WERE GOING FROM T  0 TO 5  2 SO WERE GOING TO INEGRATE OVER THA TTAT CURVE C OF Y TO THE THIRD DS WERE GOING TO CONVERT EVERYTHIN G INTO OUR PARAMETER T TIIN TERMS OF OUR PARAMETER T SO IM GOING TO BE INTEGRATING FROM T  0 TO T  2 THOSE WILL BE MY LIMITS OF INTEGRATIONS NOW Y IS EQUATOL TO T SO IIM GOING TO REPLACE Y WITH THAWHAT ITS EQUAL TO IN TERMS OF T SO IM GOING TO BE INTEGRATING THE FUNCTION T O TO THE THIRD NOW DS WERE GOING TO WRITE AS A SQUARE ROOT OF DSX TDT DQSQUARDED  DY DT DSQUARED SQUARED OF ALL THAT AS WE SAID DT SO WERE INETEGRATING NOW EVERYTHING WITH RESPECT TO T SO THIS IS GOING TO BE EQUAL TO THE INTEGRAL FROM 0 TO 2 OF 5 TO THE THIRD TIMES THE SQUARE ROOT OF  SEE THE EDERIVATIVE OF X WITH RESPECT TO T IS 3 T SQUARED SO WE HAVE 3 T SQUARED SQUARED  DY DT WELL THATS JUST 1 SUQUARED DT SO WE HAVE THE INTEGRAL FROM 0 TO 2 OF 5 TO THE THIRD TIMES THE SQUARED ROOT OF 9 T TO THE FOURTH  1 DT SO THIS IS A PRETTY STRAIGHTFORWARD INTEGRATION HERE WERE GOING TO LET U BE EQUAL TO 9 T TO THE FOURTH  1 THEN DU IS EQUAL TO 36 T TO THE THIRD DT AND SO THAT TELLS ME I CAN REPLACE A T TO THIE THIRD DT WITH A DU OVER 36 AND SO WERE GOING TO HAVE THE INTEGRAL THEN FROM  WELL NEW LIMITS OF INTEGRATION IM JUST GOIN GG TO PUT SOME SQUIGGLY MASRKS THERE TO REMIND MYSELF THAT WE SWITCHED VARIABLES SO IM NOT GOING FROM T  0 TO T  2 IM DOING THINGS IN TERMS OF YOU RIGHT NOW BUT I HAVE A 1 OVER 736 ILL PUT THAT OUT FRONT AND WERE GOING TO HAVE THE SQUARE ROOT OF U SO U TO THE 12 T TO THE THIRD DT WAS REPLACED BY DU OVER 36 WE GOT THE 36 OUT FRONT AND SO NOW THIS IS A PRETTY EASY ANTIDERIVATIVE IN TERMS OF U ITS U TO THE 32 TIMES 23 AND AGAIN PRETTDIFFERENT LIMITS OF INTEGRATION WE COULD FIGURE OUT WHAT THEY ARE IN TERMS OF U BUT IM GOING TO CONVERT BACK TOINTO T SO WERE GOING TO HAVE 1 OVER 36 TIMES 23 TIMES U TO THE 32 NOW U IS 9 T TO THE FOURTH  1 THAT TO THE 32 POWER AND NOW WE CNAN GO AHEAD AND GO FROM ORIGINAL LIMITS OF INTEGRATION 0 TO 2 SO LETS SEE WHAT EN I PUT A 2 IN HERE WERE GOING TO HAVE  1 OVER 36 TIMES 323 THATS GOING TO BE 1 OVER 54 ISNT IT SSO WELL HAVE 1 OVER 54 TTIMES  PUTTING A 2 IN WE HAVE 9 TIMES 2 TO THE FOURTH THATS 9 TIMES 16 WHICH IS 1441 IS 145 SO WE PUT THE 2 IN THERE THATWE GET GET 145 TO THE 32 MINUS PUTTING THE 0 IN WE GET 9 X 0 TO THE FOURTH THATS 0 01 IS 1 SO WE JUST GET 1 TO THE 32 OR 1 SO LETS SEE  WHATS THE BEST WAY TO WRITE THIS HOW ABOUT 1 OVER 54  I GUESS WE COULD LEAVE IT LIKE THAT  WE COULD ALSO WRITE 145 TO THE 32 AS 145 TIMES THE SQUARE ROOT OF 145 AND THEN MINUS 1 AND THAT IS THAT LINE INTEGRAL OF Y TO THE THIRD DS OVER THE GIVEN CURVE C TYP3RASDF FLAGTYP3TYP3RC
```

Flag: **flag{TYP3TYP3R}**