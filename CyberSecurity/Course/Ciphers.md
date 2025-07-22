In an **Nmap scan**, "ciphers" refer to the cryptographic algorithms that are used to encrypt data during communication between a client and a server, particularly for securing protocols like **SSL/TLS**. These algorithms determine how data will be encrypted and decrypted to ensure confidentiality, integrity, and sometimes authentication of the transmitted data.

When performing a **cipher scan** with Nmap (typically with the `--script ssl-enum-ciphers`), Nmap tests the supported ciphersuites of a server that uses SSL/TLS. This provides insight into the security of the server’s encryption configuration and helps identify weak or outdated ciphers that could make the server vulnerable to attacks.

### What Is a Cipher Suite?
A **cipher suite** is a combination of cryptographic algorithms used to establish a secure connection. A cipher suite usually includes:
1. **Key exchange algorithm**: Determines how the encryption keys will be exchanged securely (e.g., RSA, Diffie-Hellman).
2. **Encryption algorithm (cipher)**: Encrypts the data being transmitted (e.g., AES, 3DES, RC4).
3. **Message authentication algorithm**: Ensures the integrity and authenticity of the message (e.g., HMAC with SHA-256).

### Nmap Cipher Scan Output:
When Nmap scans for ciphers, the output typically provides the following information:
- **TLS/SSL versions supported**: This includes which versions of SSL/TLS are enabled on the server (e.g., TLS 1.2, TLS 1.3, SSLv3). Older versions like SSLv3 or TLS 1.0 are insecure and should be disabled.
- **Cipher suites supported**: The list of ciphers supported by the server for each protocol version.
- **Cipher strength**: Nmap categorizes the ciphers into levels of security such as `strong`, `weak`, or `insecure`. Weak ciphers could be susceptible to attacks (e.g., **RC4**, **DES**, or **3DES**).
- **Key lengths**: Information about the key length used by each cipher (e.g., 128-bit, 256-bit). Longer keys generally provide stronger encryption.

### Why It Matters:
1. **Weak Ciphers**: If a server supports weak or outdated ciphers, it may be vulnerable to cryptographic attacks like **BEAST**, **POODLE**, or **Sweet32**. These attacks can compromise the confidentiality and integrity of the data being transmitted.
2. **Compliance**: Certain regulations (like PCI-DSS or HIPAA) require servers to disable insecure ciphers and only support strong encryption.
3. **Security Audits**: A cipher scan helps identify misconfigurations and weaknesses in the server’s SSL/TLS setup that could lead to data breaches or exploitation.

### Example of an Nmap Cipher Scan Command:
To run a cipher suite scan with Nmap, you can use the following command:
```bash
nmap --script ssl-enum-ciphers -p 443 <target>
```
This command scans the SSL/TLS configuration on **port 443** (the default HTTPS port) of the target server and provides a detailed report on the supported cipher suites.

### Example Output from a Cipher Scan:
```bash
PORT    STATE SERVICE
443/tcp open  https
| ssl-enum-ciphers:
|   TLSv1.2:
|     ciphers:
|       TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (secp256r1) - A
|     compressors:
|       NULL
|   TLSv1.3:
|     ciphers:
|       TLS_AES_128_GCM_SHA256 - A
|       TLS_AES_256_GCM_SHA384 - A
|_  least strength: A
```

In this output:
- **TLSv1.2** and **TLSv1.3** are the supported protocol versions.
- The **ciphers** listed are the specific encryption algorithms in use (e.g., `AES_128_GCM_SHA256`).
- The "A" indicates the strength rating of the cipher (A is strong).
- The **compressors** refer to whether compression is enabled; `NULL` means no compression, which is typical.

### Common Weak Ciphers to Watch Out For:
- **RC4**: Known to be weak and deprecated.
- **DES** and **3DES**: These are older ciphers with shorter key lengths (e.g., 56-bit for DES), making them vulnerable to brute-force attacks.
- **MD5**: Weak hashing algorithm that should no longer be used for message authentication.

### Key Takeaways:
- **Cipher scans** in Nmap help you evaluate the security of a server’s SSL/TLS configuration.
- **Weak or outdated ciphers** should be disabled to avoid vulnerabilities.
- Strong ciphers (e.g., AES with at least 128-bit keys) and secure protocol versions (like TLS 1.2 and 1.3) should be used to ensure encrypted communications are protected.

Do you want more details on a specific cipher, or need help interpreting a particular Nmap scan output?