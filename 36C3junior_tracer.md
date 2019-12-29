As the name of the challenge indicates, we're dealing with an strace file. Looking at the file, we can see that vim is installed. 
```
264   read(0, "a", 1)                   = 1
264   write(2, "a", 1)                  = 1
264   read(0, "p", 1)                   = 1
264   write(2, "p", 1)                  = 1
264   read(0, "t", 1)                   = 1
264   write(2, "t", 1)                  = 1
264   read(0, " ", 1)                   = 1
264   write(2, " ", 1)                  = 1
264   read(0, "i", 1)                   = 1
264   write(2, "i", 1)                  = 1
264   read(0, "n", 1)                   = 1
264   write(2, "n", 1)                  = 1
264   read(0, "s", 1)                   = 1
264   write(2, "s", 1)                  = 1
264   read(0, "t", 1)                   = 1
264   write(2, "t", 1)                  = 1
264   read(0, "a", 1)                   = 1
264   write(2, "a", 1)                  = 1
264   read(0, "l", 1)                   = 1
264   write(2, "l", 1)                  = 1
264   read(0, "l", 1)                   = 1
264   write(2, "l", 1)                  = 1
264   read(0, " ", 1)                   = 1
264   write(2, " ", 1)                  = 1
264   read(0, "v", 1)                   = 1
264   write(2, "v", 1)                  = 1
264   read(0, "i", 1)                   = 1
264   write(2, "i", 1)                  = 1
264   read(0, "m", 1)                   = 1
264   write(2, "m", 1)                  = 1
264   read(0, "\r", 1)                  = 1
264   write(2, "\n", 1)                 = 1
```
Then, the file "Flag" is opened in vim.
```
264   read(0, "v", 1)                   = 1
264   write(2, "v", 1)                  = 1
264   read(0, "i", 1)                   = 1
264   write(2, "i", 1)                  = 1
264   read(0, "m", 1)                   = 1
264   write(2, "m", 1)                  = 1
264   read(0, " ", 1)                   = 1
264   write(2, " ", 1)                  = 1
264   read(0, "F", 1)                   = 1
264   write(2, "F", 1)                  = 1
264   read(0, "l", 1)                   = 1
264   write(2, "l", 1)                  = 1
264   read(0, "a", 1)                   = 1
264   write(2, "a", 1)                  = 1
264   read(0, "g", 1)                   = 1
264   write(2, "g", 1)                  = 1
264   read(0, "\r", 1)                  = 1
264   write(2, "\n", 1)                 = 1
```
Clearly, we want to know what's in this file. This section where INSERT mode is entered looks promising.
``` 541   read(0, "i", 4096)                = 1
541   write(1, "\33[?25l\33[53;164Hi\33[1;1H", 22) = 22
541   write(1, "\33[53;164H \33[1;1H", 16) = 16
541   write(1, "\33[53;1H\33[1m-- INSERT --\33[m\33[53;1"..., 57) = 57
541   write(1, "\33[1;1H\33[?25h", 12)  = 12
```
We're only interested in read operation values which we can find with below script
```
import re

for line in open('data'):
	x = re.search("541\s+read\(\d+,\s+\"([^\"]+)\"", line)
	try:
		print(x.group(1))
	except:
		pass
```

To get the flag, we simply need to recreate the vim input, keeping in mind that "\33" maps to ESC and "\177" maps to DEL.
```
j
\r
\r
\r
u
n
\r
\r
i
o
\r
r
-
\r
n
a
\33
y
y
p
A
\177
o
\r
\r
i
\r
s
\r
\r
w
a
\r
y
\r
b
e
\r
t
t
e
\r
\r
r
!
\33
g
g
J
J
J
J
J
J
J
J
J
J
J
J
J
J
J
J
J
J
b
b
~
~
~
~
~
~
~
~
\33
:
s
/
 
/
/
g
\r
\33
:
w
q
```

`junior-nanoiswayBETTER!`
