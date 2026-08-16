## <span style="color:rgb(0, 176, 240)">Challenge Overview</span>
- **Platform**: PicoCTF/CyLab
- **Name**: vault-door-7
- **Description**: This vault uses bit shifts to convert a password string into an array of integers. Hurry, agent, we are running out of time to stop Dr. Evil's nefarious plans! The source code for this vault is here: [VaultDoor7.java](https://challenge-files.picoctf.net/c_fickle_tempest/c5f4e07beec2cd25f5eb995bfd1d91e5dc6494ff2bab2557222fe538f2c6be45/VaultDoor7.java).
- **Difficulty**: Hard
- **Category**: Reverse Engineering
## <span style="color:rgb(0, 176, 240)">Solution</span>
The provided file, VaultDoor7.java, explains that each character can be represented as a byte using ASCII encoding. An integer is typically 4 bytes / 32 bits. The password is hidden by representing the 32 character password as an array of 8 integers.

```java

    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        int[] x = passwordToIntArray(password);
        return x[0] == 1096770097
            && x[1] == 1952395366
            && x[2] == 1600270708
            && x[3] == 1601398833
            && x[4] == 1716808014
            && x[5] == 1734293606
            && x[6] == 909455713
            && x[7] == 1664103218;
    }
```
To find the password, we need to take each integer and convert it into its binary representation (32 bits), then convert each 8 bit section into an ASCII char.

<br>


| Integer    | Binary                              | ASCII |
| ---------- | ----------------------------------- | ----- |
| 1096770097 | 01000001 01011111 01100010 00110001 | A_b1  |
| 1952395366 | 01110100 01011111 00110000 01100110 | t_0f  |
| 1600270708 | 01110100 01011111 00110000 01100110 | _b1t  |
| 1601398833 | 01011111 01110011 01101000 00110001 | _sh1  |
| 1716808014 | 01100110 01010100 01101001 01001110 | fTiN  |
| 1734293606 | 01100111 01011111 00111000 01100110 | g_8f  |
| 909455713  | 00110110 00110101 00110001 01100001 | 651a  |
| 1664103218 | 01100011 00110000 00110011 00110010 | c032  |


This gives us our password `A_b1t_0f_b1t_sh1fTiNg_8f651ac032`, meaning our flag for this challenge is `picoCTF{A_b1t_0f_b1t_sh1fTiNg_8f651ac032}`!
