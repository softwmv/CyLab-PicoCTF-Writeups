---
date: 2026-08-07
tags:
  - reverse-engineering
---
## <span style="color:rgb(0, 176, 240)">Challenge Overview</span>
- **Platform**: PicoCTF/CyLab
- **Name**: vault-door-4
- **Description**: This vault uses ASCII encoding for the password. The source code for this vault is here: [VaultDoor4.java](https://challenge-files.picoctf.net/c_fickle_tempest/dfb236ca8b03fc1044ad906ce94fd2ed85beb1d1118f09234607b5f79d4b72fc/VaultDoor4.java).
- **Difficulty**: Medium
- **Category**: Reverse Engineering
## <span style="color:rgb(0, 176, 240)">Solution</span>
```java
    public boolean checkPassword(String password) {
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
            0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
            0142, 0131, 0164, 063 , 0163, 0137, 066 , 064 ,
            'e' , '1' , '3' , 'd' , '0' , '0' , 'b' , '2' ,
        };
        for (int i=0; i<32; i++) {
            if (passBytes[i] != myBytes[i]) {
                return false;
            }
        }
        return true;
    }
```
In the provided file, we can see that the `checkPassword` method checks if the password matches by comparing each character using an array of bytes.

```cpp
#include <iostream>
#include <string>
using namespace std;

int main()
{
    const char bytes[] = {            
        106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
        0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
        0142, 0131, 0164, 063 , 0163, 0137, 066 , 064 ,
        'e' , '1' , '3' , 'd' , '0' , '0' , 'b' , '2'
        };

    string s(bytes, sizeof(bytes));
    cout << s;
}
```
To find the password, we simply just need to print the character representation of each byte; this gives us the answer `picoCTF{jU5t_4_bUnCh_0f_bYt3s_64e13d00b2}`.