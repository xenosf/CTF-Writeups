# Airlock Breakout

**Category**: Reverse Engineering

100 points

----

## Challenge Description
I've stolen the Nova Core, but it seems like I've been locked in within these 3 airlock doors. I'm not sure how much longer I can hold them off Please help me crack the password so that I can be on my merry way!

`http://X.X.X.X:55030` [IP redacted]

The flag is in the flag format: STC{...}

**Author:** PlatyPew

----

## Solution
Upon visiting the website, we are greeted with a page with only a password input textbox and a submit button.

<!--screenshot-->

We can use the inspect element tool to investigate, and find out that pressing the button calls the     `unlock` function, with the contents of the textbox as the arguments. We can also see that the function is located in a file called `flag.js`.

<!--screenshot-->

To look at `flag.js`, we can go to the Network tab of the inspect element panel.

<!--screenshot-->

Let's look at the aforementioned `unlock` function:
```js
function unlock(pwd) {
    var flagSplit = pwd.split("_");
    if (flagSplit.length !== 6) return false;

    if (part1(flagSplit[0])) console.log("Airlock 1 is opened");
    else return false;
    if (part2(flagSplit[1])) console.log("Airlock 2 is opened");
    else return false;
    if (part3(flagSplit[2], flagSplit[3], flagSplit[4], flagSplit[5]))
        console.log("Airlock 3 is opened");
    else return false;

    alert(`Congratulations!\nFlag: STC{${pwd}}`);
}
```

From this, we can deduce that:
* The password comprises 6 segments, separated by underscores.
* The function checks it in 3 stages, and outputs a message to the JavaScript console when each stage passes.

Now, let's take a look at `part1`:

```js
function part1(str1) {
    if (str1.length === 5) {
        arr = str1.split("");
        if (
            arr[0] === String.fromCharCode(51) &&
            arr[1] === String.fromCharCode(74) &&
            arr[2] === String.fromCharCode(51) &&
            arr[3] === String.fromCharCode(67) &&
            arr[4] === String.fromCharCode(55)
        )
            return true;
    }
}
```

Here's what we can deduce from the above:

* The first part of the string is 5 characters long.
* The character codes for the characters are 51, 74, 51, 67, and 55.

This one's pretty simple. To get the text, we can get the characters from the character codes given.

<!--screenshot-->

Password so far:
```
3J3C7_x_x_x_x_x
```

Time for `part2`:
```js
function part2(str2) {
    if (str2.length === 4) {
        if (
            str2.charCodeAt(0) +
                2 * str2.charCodeAt(1) -
                3 * str2.charCodeAt(2) +
                4 * str2.charCodeAt(3) ===
                354 &&
            2 * str2.charCodeAt(0) +
                2 * str2.charCodeAt(1) -
                2 * str2.charCodeAt(2) +
                3 * str2.charCodeAt(3) ===
                383 &&
            3 * str2.charCodeAt(0) -
                2 * str2.charCodeAt(1) -
                4 * str2.charCodeAt(2) +
                str2.charCodeAt(3) ===
                -106 &&
            2 * Math.pow(str2.charCodeAt(0), 3) +
                3 * Math.pow(str2.charCodeAt(1), 2) -
                2 * Math.pow(str2.charCodeAt(2), 3) -
                4 * Math.pow(str2.charCodeAt(3), 2) ===
                59284
        )
            return true;
    }
}
```

This one's a bit more difficult. From the above function, we can see that:

* The length of this segment is 4.
* It's checking for whether the character codes fulfil a set of simultaneous equations:

  <!--image-->

  (Where A, B, C, D are the 1st, 2nd, 3rd, and 4th characters respectively.)

Solving the system of equations, we get A=55, B=72, C=51, and D=7. We can then convert these character codes to characters to get the next part of the password:

```
3J3C7_7H3M_x_x_x_x
```

Getting closer... Onwards to `part3`:

```js
function part3(str3, str4, str5, str6) {
    var magic = 0;
    for (var strs of [str3, str4, str5, str6]) {
        if (!/^[01347CFHKLNRUX]+$/g.test(strs)) return false;

        for (var i = 0; i < strs.length; i++) {
            magic = (magic << 3) + strs.charCodeAt(i) - magic;
        }
    }

    if (
        str3.length === 3 &&
        str4.length === 2 &&
        str5.length === 3 &&
        str6.length > 5 &&
        str3[0] === "0" &&
        str5[0] === "7" &&
        str3.charCodeAt(0) + str5.charCodeAt(0) - str6.charCodeAt(0) === 51 &&
        str3.charCodeAt(0) === str4.charCodeAt(0) &&
        str3.charCodeAt(2) - str3.charCodeAt(1) === -30 &&
        (str4.charCodeAt(1) / 7) * str5.charCodeAt(1) === 720 &&
        (str5.charCodeAt(0) + str5.charCodeAt(2) + 2) / 3 === 36 &&
        str6.charCodeAt(3) - str6.charCodeAt(2) === -6 &&
        str6.charCodeAt(2) * str6.charCodeAt(4) === 3936 &&
        magic === -859895409
    )
        return true;
}
```

This one's more difficult. Let's see what we can deduce easily, going from top to bottom.

Firstly, we can see that the last 4 segments of the password are handled by this part.

Breaking down the regular expression `/^[01347CFHKLNRUX]+$/g` in the first `if` statement:
* `/` marks the start and the end of the expression.
* The global `g` flag at the end tells it to find all matches (rather than stopping at the first match). This doesn't matter in our case, since it's testing one segment at a time.
* `^` and `$` match the start and end of a line respectively. In this case, this means that the entire line's contents have to match what's between them.
* `[01347CFHKLNRUX]` matches any of the characters 0, 1, 3, 4, 7, C, F, H, K, L, N, R, U, X.
* The quantifier `+` matches 1 or more of the preceding token. In our case, it matches 1 or more of any character in `01347CFHKLNRUX`.

Putting this together, we now know that the valid characters for the last few segments are: `01347CFHKLNRUX`.

I ignored the magic sum part first and moved onto the next if statement. From those, we can deduce that:

* The 3rd segment has 3 characters.
* The 4th segment has 2 characters.
* The 5th segment has 3 characters.
* The 6th segment has more than 5 characters.
* The first character of the 3rd segment is "0".
* The first character of the 5th segment is "7".

What we have so far is:
```
3J3C7_7H3M_0xx_xx_7xx_xxxxxx...
```

The next few parts are more mathematics. For simplicity, I'll be identifying each character's character code using a placeholder letter:
```
3J3C7_7H3M_0ab_cd_7ef_ghijkl...
```

The character codes of "0" is 48,  and "7" is 55. Using this information, we can then form more equations. (For simplicity, I didn't show all of them here – only the ones that immediately gave us more characters)

<!--image-->

Converting the character codes back to characters, we can fill in a few more letters in the password:
```
3J3C7_7H3M_0ab_0d_7e3_4hijkl...
```

This, along with the question context and list of possible characters, was enough for me to guess the rest of the sentence, "eject them out of the airlock":
```
3J3C7_7H3M_0U7_0F_7H3_41RL0CK
```

To find the remaining characters of the password in a less guessy way, we could do a bruteforce, using the list of accepted characters, and checking using the rest of the `if` statement.

Sure enough, when I entered in that password, the 3 airlocks opened and triggered an alert with the flag.

<!--screenshot-->



### Flag:
```
STC{3J3C7_7H3M_0U7_0F_7H3_41RL0CK}
```
