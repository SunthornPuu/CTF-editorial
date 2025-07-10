# 🔑 keygenme-py

**Credit:** [picoCTF - keygenme-py](https://play.picoctf.org/practice/challenge/121)

Welcome to the funnest (my most favourite) section of CTF — **CODING!!!!**

This one is a good fresh start for you in programming problems. Why? Let's see:

![image](https://github.com/user-attachments/assets/05d8f2d8-8df9-4d3d-9201-085244ab2825)

---

## 🔎 Observation

Take a look at this code. To start debugging/reverse engineering, you need to know: **"WHAT IS THE CODE DOING?"**

![CODE](https://github.com/user-attachments/assets/a0a07ac2-5896-487c-9a0e-b51af68918d1)

Let's take an observation of this code. Let's summarize its flow:

* Start with the main loop calling `ui_flow()` function
* This will show the menu to select an operation \[calls `menu_trial()`]
* In the menu, you will see 4 options. Use your common sense and follow the flow of option C \[go to `enter_license()`]

You may ask: "Why?" — That's so simple. If you want to unlock the door, just take a look inside the keyhole.

Here we are in `enter_license()` — let's see how the key works with this keyhole. Oh! Here we are:

```python
if check_key(user_key, bUsername_trial):
    decrypt_full_version(user_key)
```

The whole thing is actually here! You can either check `check_key()` or `decrypt_full_version()`.

If you're wondering why, let me explain:

```
if the key is right then open the fucking door
```

So, checking either "what does the right key look like" or "how do you open the door if I give you the right key" is fine.
I'm kinda lazy, so let's take a look inside the keyhole first \[go to `decrypt_full_version()`]:

```python
def decrypt_full_version(key_str):

    key_base64 = base64.b64encode(key_str.encode())
    f = Fernet(key_base64)

    try:
        with open("keygenme.py", "w") as fout:
            global full_version
            global full_version_code
            full_version_code = f.decrypt(full_version)
            fout.write(full_version_code.decode())
            global arcane_loop_trial
            arcane_loop_trial = False
            global jump_into_full
            jump_into_full = True
            print("\nFull version written to 'keygenme.py'.\n\n" +
                  "Exiting trial version...")
    except FileExistsError:
        sys.stderr.write("Full version of keygenme NOT written to disk, " +
                         "ERROR: 'keygenme.py' file already exists.\n\n" +
                         "ADVICE: If this existing file is not valid, " +
                         "you may try deleting it and entering the " +
                         "license key again. Good luck")
```

Okay, the next question is: **"Where is the license key I want?"** — ask yourself first. Don't read the spoiler yet.

```python
key_base64 = base64.b64encode(key_str.encode())
f = Fernet(key_base64)
full_version_code = f.decrypt(full_version)
fout.write(full_version_code.decode())
```

Here we are!
We can use this part to reverse the function, but won’t that be too complex?
Let’s take an observation from the other side \[go to `check_key()`]:

```python
def check_key(key, username_trial):

    global key_full_template_trial

    if len(key) != len(key_full_template_trial):
        return False
    else:
        i = 0
        for c in key_part_static1_trial:
            if key[i] != c:
                return False
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[4]: return False
        i += 1
        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]: return False
        i += 1
        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]: return False
        i += 1
        if key[i] != hashlib.sha256(username_trial).hexdigest()[6]: return False
        i += 1
        if key[i] != hashlib.sha256(username_trial).hexdigest()[2]: return False
        i += 1
        if key[i] != hashlib.sha256(username_trial).hexdigest()[7]: return False
        i += 1
        if key[i] != hashlib.sha256(username_trial).hexdigest()[1]: return False
        i += 1
        if key[i] != hashlib.sha256(username_trial).hexdigest()[8]: return False

        return True
```

This function returns a boolean `True` or `False`.
It also mentions three variables: `username_trial`, `key_full_template_trial`, and `key_part_static1_trial`,
which you'll notice locally at the top of the code (please use **Ctrl+F** — don't bruteforce scroll like a monkey):

```python
username_trial = "FREEMAN"
key_part_static1_trial = "picoCTF{1n_7h3_|<3y_of_"
key_part_dynamic1_trial = "xxxxxxxx"
key_part_static2_trial = "}"
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial
```

So `key_full_template_trial`, which is the key template, is a combination of three parts.

What this function really does is use the variable `i` to iterate over the key's characters,
checking if it matches what the correct key should be.

YEAH here we go — that’s actually the **full solution** of this problem!
After the `key_part_static1_trial`, it uses `hashlib.sha256(username_trial).hexdigest()[X]` to extract some characters.

Go print them yourself:

```python
username_trial = b"FREEMAN"
import hashlib
print(hashlib.sha256(username_trial).hexdigest()[4])
print(hashlib.sha256(username_trial).hexdigest()[5])
print(hashlib.sha256(username_trial).hexdigest()[3])
print(hashlib.sha256(username_trial).hexdigest()[6])
print(hashlib.sha256(username_trial).hexdigest()[2])
print(hashlib.sha256(username_trial).hexdigest()[7])
print(hashlib.sha256(username_trial).hexdigest()[1])
print(hashlib.sha256(username_trial).hexdigest()[8])
```

That’s all for `key_part_dynamic1_trial` — **YAY!**

---

This is actually a **good problem** — it’s a good meter to test if you’re good at **flow reading**, one of the most-wanted skills of a developer.

Recently, I got invited to a dev team by a junior dev. I was wondering how good he was at coding logic,
so I sent him this problem. He took so long to figure it out (that’s kinda disappointing btw).

Anyway, if you don’t get it — go learn CP. It will help you a lot in programming.

If you don't get any part, feel free to ask me.
📩 IG: [a\_phkhn](https://www.instagram.com/a_phkhn)

---

**Have a nice day!**
