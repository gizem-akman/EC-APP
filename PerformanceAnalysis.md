# Performance Analysis Breakdown

## Chosen Cryptographic Primitives

- *Public key primitive:*
  
  - Elliptic Curve Integrated Encryption Scheme (ECIES) with curve P-256.

  - Module-Lattice-Based Key Encapsulation Mechanism (ML-KEM-512).

  - Elliptic Curve Digital Signature Algorithm (ECDSA) with curve P-256.

  - Module-Lattice-Based Digital Signature Algorithm (ML-DSA-44).

- *Symmetric key encryption:* Advanced Encryption Standard with a block size of 128 bits and in CCM mode (AES-128-CCM). 
- *Hash function:* Secure Hash Algorithms (SHA-256).
- *Certificate:* X.509 with Elliptic Curve Cryptography (ECC) with curve P-256.
## Communication Cost

- **ECIES P-256:**
  - Public key: 32 bytes
  - Secret key: 32 bytes
  - Ciphertext: 32 bytes
- **ECDSA P-256:**
  - Public key: 32 bytes
  - Secret key: 32 bytes
  - Signature: 64 bytes 
- **ML-KEM-512:**
  - Encapsulation key: 800 bytes
  - Decapsulation key: 1632 bytes
  - Ciphertext: 768 bytes
- **ML-DSA-44:**
  - Public key: 1312 bytes
  - Secret key: 2560 bytes
  - Signature: 2420 bytes 
- **AES-128-CCM:**  
  - Key: 16 bytes
  - Ciphertext: 16 bytes
- **SHA-256:** 
  - Hashed value: 32 bytes.
- **X.506 certificate:**
  - Certificate: 256 bytes
- **Further Assumptions**
  - Nonces: 16 bytes
  - PSM notification message: ~16 bytes
  
With the cryptographic primitive choices, listed above, the communication cost of the protocol is summarized in the table below. Note that N is the amount of e-coupons generated in that session. 

|                        | **N=50, ECC (bytes)** | **N=50, ML (bytes)** | **N=1000, ECC (bytes)** | **N=1000, ML (bytes)** |
|------------------------|:-------------:|:------------:|:---------------:|:--------------:|
| **Initiation (V1&V2)** |      1664     |     4020     |      32064      |      34420     |
| **Only Variant 1**     |      240      |      240     |       240       |       240      |
| **Only Variant 2**     |      2776     |     8728     |      33176      |      39128     |
| **V1 total**           |      1904     |     4260     |      32304      |      34660     |
| **V2 total**           |      4440     |     12748    |      65240      |      73548     |


## Computational Cost
We used OpenSSL 3.5.0 on an *x86-64 machine with Linux kernel version 6.14.0-37-generic* to obtain the computation times of the cryptographic operations, show below in the table and the list. 

|                 	| **Key Generation (μs)** 	| **Encapsulation (μs)** 	| **Decapsulation (μs)** 	|
|-----------------	|:------------------:	|:-----------------:	|:-----------------:	|
| **ECIES P-256** 	|        11.87       	|       81.11       	|       64.54       	|
| **ML-KEM-512**  	|        30.00       	|       19.37       	|       29.81       	|
|                 	| **Key Generation (μs)** 	|   **Signature (μs)**   	|  **Verification (μs)** 	|
| **ECDSA P-256** 	|        11.87       	|       21.10       	|       64.92       	|
| **ML-DSA-44**   	|        23.13       	|       69.89       	|       22.99       	|

- **AES-128-CCM:** 0.0134  μs
- **SHA-256:** 0.0188 μs
- **RAND generator (16-byte):** 0.0224 μs

The table below shows the total number of operations executed in each phase of the protocol.

|Phase|Operations|
|---|---|
|**Initiation**|2n random generation, 2n hashes, signing, verifying|
|**Variant 1**|2 random generation, 2 hashes|
|**Variant 2**|2 random generation, 2 hashes, 4 public key operation, 6 symmetric key operation|

We used the computation time of the cryptographic operations, presented in the table above, to estimate the computation
time of the two variants. The table below shows the total computation time for the protocol variants.

|                        | **N=50, ECC (μs)** | **N=50, ML (μs)** | **N=1000, ECC (μs)** | **N=1000, ML (μs)** |
|------------------------|:-------------:|:------------:|:---------------:|:--------------:|
| **Initiation (V1&V2)** |     90.14     |     96.99    |      168.42     |     175.28     |
| **Only Variant 1**     |     0.08    |     0.08     |       0.08      |      0.08      |
| **Only Variant 2**     |     291.46    |     98.52    |      291.46     |      98.52     |
|||
| **V1 total**           |     90.22     |     97.08    |      168.50     |     175.36     |
| **V2 total**           |     381.60    |    195.52    |      459.88     |     273.80     |
```
