## <span style="color:rgb(0, 176, 240)">Challenge Overview</span>
- **Platform**: PicoCTF/CyLab
- **Name**: vault-door-6
- **Description**: This vault uses an XOR encryption scheme. The source code for this vault is here: [VaultDoor6.java](https://challenge-files.picoctf.net/c_fickle_tempest/e04dfee88565a7f918254fd582c9bd4b0e602916d2a7409dea39ccfb4e8cd116/VaultDoor6.java).
- **Difficulty**: Medium
- **Category**: Reverse Engineering
## <span style="color:rgb(0, 176, 240)">Solution</span>
```java
public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
            0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
            0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
            0xa , 0x67, 0x65, 0x67, 0x62, 0x6c, 0x6d, 0x66,
        };
        for (int i=0; i<32; i++) {
            if (((passBytes[i] ^ 0x55) - myBytes[i]) != 0) {
                return false;
            }
        }
        return true;
    }
```
The `checkPassword` method for this vault uses XOR encryption, which uses a key and the XOR operation to create a ciphertext. The password is XOR'd with the key 0x55 (the letter "U") and compared to the bytes stored in `myBytes`. 

```cpp
#include <iostream>
using namespace std;

int main()
{
    const char bytes[] = {
        0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
        0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
        0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
        0xa , 0x67, 0x65, 0x67, 0x62, 0x6c, 0x6d, 0x66,
        };

    for (int i=0; i<32; i++) {
        printf("%c", (bytes[i] ^ 0x55));
    }
}

```
To find the password, we just need to XOR the ciphertext with the same key! This gives us the password `picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_2027983}`.
