## <span style="color:rgb(0, 176, 240)">Challenge Overview</span>
- **Platform**: PicoCTF/CyLab
- **Name**: vault-door-3
- **Description**: This vault uses for-loops and byte arrays. The source code for this vault is here: [VaultDoor3.java](https://challenge-files.picoctf.net/c_fickle_tempest/caeef81009d61675ffc5fd38029a53105102ceafab8248d48f73aa4e96ea0cb6/VaultDoor3.java).
- **Difficulty**: Medium
- **Category**: Reverse Engineering
## <span style="color:rgb(0, 176, 240)">Solution</span>
The buffer's value is compared to the string `jU5t_a_sna_3lpm13g64f_u_4_m6r143`. This value is created from the actual password and scrambled using for loops, so we can just reverse the process by unscrambling the buffer's value.

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string str = "jU5t_a_sna_3lpm13g64f_u_4_m6r143";
    char buffer[32];

    int i;

    for (i=0; i<8; i++) {
        buffer[i] = str[i];
    }

    for (; i<16; i++) {
        buffer[i] = str[(23-i)];
    }

    for (; i<32; i+=2) {
		buffer[i] = str[(46-i)];
	}

	for (i=31; i>=17; i-=2) {
		buffer[i] = str[(i)];
	}

	for (int j=0; j<32; j++){
	    cout << buffer[j];
	}
}

```

This gives us the password `jU5t_a_s1mpl3_an4gr4m_4_u_f66133`, meaning our solution is `picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_f66133}`.
