---
date: 2026-08-07
tags:
  - reverse-engineering
---
## <span style="color:rgb(0, 176, 240)">Challenge Overview</span>
- **Platform**: PicoCTF/CyLab
- **Name**: vault-door-5
- **Description**: In the last challenge, you mastered octal (base 8), decimal (base 10), and hexadecimal (base 16) numbers, but this vault door uses a different change of base as well as URL encoding! The source code for this vault is here: [VaultDoor5.java](https://challenge-files.picoctf.net/c_fickle_tempest/a8fa4d19d7d2445ee8152987624b46c1ff4eeeab9319ab48c513f66f5b903ef8/VaultDoor5.java).
- **Difficulty**: Medium
- **Category**: Reverse Engineering
## <span style="color:rgb(0, 176, 240)">Solution</span>
```java
public boolean checkPassword(String password) {
	String urlEncoded = urlEncode(password.getBytes());
	String base64Encoded = base64Encode(urlEncoded.getBytes());
	String expected = "JTYzJTMwJTZlJTc2JTMzJTcyJTc0JTMxJTZlJTY3JTVm"
					+ "JTY2JTcyJTMwJTZkJTVmJTYyJTYxJTM1JTY1JTVmJTM2"
					+ "JTM0JTVmJTM3JTY2JTM4JTM1JTM1JTY2JTYzJTM1";
	return base64Encoded.equals(expected);
}
```
In this `checkPassword` method, we can see that the password is URL encoded then base64 encoded to get a final string of `JTYzJTMwJTZlJTc2JTMzJTcyJTc0JTMxJTZlJTY3JTVmJTY2JTcyJTMwJTZkJTVmJTYyJTYxJTM1JTY1JTVmJTM2JTM0JTVmJTM3JTY2JTM4JTM1JTM1JTY2JTYzJTM1`. 

Using CyberChef to decode the expected string gives us the solution `picoCTF{c0nv3rt1ng_fr0m_ba5e_64_7f855fc5}`.