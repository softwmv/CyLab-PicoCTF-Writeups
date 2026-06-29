## <span style="color:rgb(0, 176, 240)">Challenge Overview</span>

- **Platform**: PicoCTF/CyLab
- **Name**: keygenme.py
- **Description**: [A python file was provided.](https://challenge-files.picoctf.net/c_wily_courier/2f1715b58e4cb2cbfeff85a29a12c7e120b9b6bd120b9604a835499300abf2ef/keygenme-trial.py)
- **Difficulty**: Medium
- **Category**: Reverse Engineering
## <span style="color:rgb(0, 176, 240)">Solution</span>

If we look at the source code, we can see that the full key is composed of two static parts and one dynamic part, i.e. `key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial`.

```python
key_part_static1_trial = "picoCTF{1n_7h3_kk3y_of_"
key_part_dynamic1_trial = "xxxxxxxx"
key_part_static2_trial = "}"
```

For this problem, we just need to find the value of `key_part_dynamic1_trial`, as entering the correct key calls the `decrypt_full_version()` function to decrypt the encrypted full version blob.

 ```python
 def check_key(key, username_trial):

    global key_full_template_trial

    if len(key) != len(key_full_template_trial):
        print("Invalid length")
        return False
    else:
        # Check static base key part --v
        i = 0
        for c in key_part_static1_trial:
            if key[i] != c:
                return False

            i += 1

        # TODO : test performance on toolbox container
        # Check dynamic part --v
        if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
            return False

        return True
 ```

The `check_key()` function checks static_part_1 and the dynamic part, which uses 8 characters from the hex digest of `username_trial`.

You can create a keygen using the trial username, which in our case is **"BENNET."** The SHA256 value of "BENNET" is `ba6c084a4d888e1f7c3b0fc71d61c4625708bd915b5e0e60eb73e1667251b567`. Using the key values of the digest will give us the dynamic part of the key.

Here is a table for better visualization:

| Key position | Key Value | Hash index |     |
| ------------ | --------- | ---------- | --- |
| 0            | B         | 4          | 0   |
| 1            | E         | 5          | 8   |
| 2            | N         | 3          | c   |
| 3            | N         | 6          | 4   |
| 4            | E         | 2          | 6   |
| 5            | T         | 7          | a   |
| 6            | T         | 1          | a   |
| 7            |           | 8          | 4   |


This gets us the final flag: `picoCTF{1n_7h3_kk3y_of_08c46aa4}`
