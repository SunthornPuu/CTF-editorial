# 🔠 rotation  
**Credit:** [picoCTF - rotation](https://play.picoctf.org/practice/challenge/373)

For beginners, this is a good start in cryptography. Why? Let's see:  
![image](https://github.com/user-attachments/assets/10a35568-d8fa-42bf-9b76-49e6a3d0996a)

`"xqkwKBN{z0bib1wv_l3kzgxb3l_4k71n5j0}"`
hint 1: Sometimes rotation is right

---

## 🔎 Observation

Take a look at the encrypted message.  
Does it look familiar to you?  
If not, take a little time to observe.

```
pico → xqkw  
CTF  → KBN
```

It follows with the curly bracket `{` and ends with the curly bracket `}` too. What a coincidence!

So, the next question is:  
**"Yo, how will I turn `pico` into `xqkw`, or how will I turn `CTF` into `KBN`?"**  
This is the fun part of the problem. Take a moment to observe.  
If you're tired of trying — take a look below.

---

It's actually so simple. If you convert each character to its alphabetical index:

```
x q k w = 24 17 11 23  
p i c o = 16  9  3 15
```

So, it's just a **-8 shift**.

What about the uppercase part? Is it the same?

```
K B N = 11 2 14  
C T F = 3 20 6
```

Sure it is! For example, if `'B'` got minus 8 (with wrap-around), it becomes `'T'`.

So the function looks like this:

```
f(x) = (x - 8 + 26) % 26

```

Where `f(x)` is the encrypted alphabetical order of `x`.

---

## 🚫 Stop the Bruteforce Campaign

**Bruteforce is for monkeys only.**  
We are civilized humans — so it's kinda good if you don't solve this one with just paper or Notepad.

Here’s my decrypting code:

```cpp
#include "bits/stdc++.h"
using namespace std;

int main() {
    string s; //--> given string s as an encrypted string
    cin >> s;
    for (auto i : s) { //--> iterate over s as i (range-based for loop)
        char base; //--> base is the first order of i
        // we will use base to tell if i is uppercase or lowercase
        if (i >= 'a' && i <= 'z') base = 'a';     //--> lowercase
        else if (i >= 'A' && i <= 'Z') base = 'A'; //--> uppercase
        else {
            cout << char(i);
            continue;
            // we skip the curly bracket once, so I assumed there's no need to decrypt symbols
        }
        cout << char((i - base - 8 + 26) % 26 + base);
        // decrypt it as in the function above
    }
}
```

This code was written so simply.  
If you can't read it — go learn CP. It will help you a lot in programming.

If you don't get any part, feel free to ask me.  
📩 IG: [a_phkhn](https://www.instagram.com/a_phkhn)

---

**Have a nice day!**
