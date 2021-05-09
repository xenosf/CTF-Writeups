# Aaand It's Gone
### Category: Reverse Engineering
---
## Challenge Description
Seems like APOCALYPSE has been up to no good again. They've been running a certain program to corrupt a random portion of everyone's files and causing chaos all around. Could you please help to reverse engineer their code and see if it's possible to recover the flag?
## Attached files
* flag.corrupted
* gone.py
## gone.py
```python
from random import randint

data = open('flag.txt', 'rb').read().strip()

data1 = data[::2]
data2 = data[1::2]
data3 = []

assert len(data1) == len(data2)

for i, j in zip(data1, data2):
    data3.append(i ^ j)

output = []

if (randint(0, 1000) % 2):
    print('aaand... data1 is gone!')
    for i, j in zip(data2, data3):
        output.append(j)
        output.append(i)
    open('flag.corrupted', 'wb').write(bytes(output))
else:
    print('aaand... data2 is gone!')
    for i, j in zip(data1, data3):
        output.append(i)
        output.append(j)
    open('flag.corrupted', 'wb').write(bytes(output))
```
---
## Solution
From the code, we can see that this ~~yeets~~ deletes a randomly chosen half of the data, replacing it with `data3`, which is `data1` ⊕ (XOR) `data2`.

Luckily for us, we can easily get `data1` / `data2` back by XORing `data3` with `data2` / `data1` respectively. This is due to the properties of XOR:

`a` ⊕ `b` = `c`  
`b` ⊕ `c` = `a`  
`c` ⊕ `a` = `b`

Knowing this, we can now write a script to decode the flag.
```python
from random import randint

data = open('flag.corrupted', 'rb').read()

# case 1: data1 gone

data3=data[::2]
data2=data[1::2]
data1=[]

output=[]

for i,j in zip(data3, data2):
    data1.append(i^j)

for i,j in zip(data1,data2):
    output.append(i)
    output.append(j)

open('flag_1.txt', 'wb').write(bytes(output))

# case 2: data2 gone

data1=data[::2]
data3=data[1::2]
data2=[]

output=[]

for i,j in zip(data3, data1):
    data2.append(i^j)

for i,j in zip(data1,data2):
    output.append(i)
    output.append(j)

open('flag_2.txt', 'wb').write(bytes(output))
```
I chose to write the above script in 2 parts as it was easier for me to conceptualise quickly (read: I was lazy). This required me to manually check which of the 2 output files contained the full decoded flag.

### flag_1.txt
```Cyberthon{f750a0f1ff4b977ed7bf2a10688ef4740e35edd63aca8c9c29fb152ee63d03cb7}```
### flag_2.txt
```:CbrhnQf5QaWf�fV49R7SdbS216]8Rf7U03eRdR3c[8Z92f1W2SeW30cJ7```

It's clear that `flag1_txt` contains the correct output, and that `data1` was the part that got Thanos-snapped in the original file. We now have the flag! Hooray 👍👍
