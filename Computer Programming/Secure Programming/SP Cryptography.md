---
Tags: 
Created: 2023-10-03 00:28:36
Links: "[[Secure Programming]]"
---
# Introduction
Cryptography allows two people to communicate securely without anyone intercepting or modifying the messages. Cryptography tries to prevail confidentiality, integrity, authentication and non-repudiation (denying something you sent).
## Terminology
- **Plaintext**: The readable text to be transmitted
- **Cipher text**: The unreadable text actually sent
- **Encryption**: Converting plaintext into cipher text
- **Decryption**: Recovering plaintext from cipher text

To minimize that the encrypted data is compromised, we separate the algorithm from the key. This is known as **Kerckhoffs's Principle**. We always assume that the attacker knows the algorithm since we want to reuse the algorithm. The opposite of this would be [[Security by Obscurity]]. This would also mean that if the if there were an attack, it wouldn't be because the algorithm was kept secret, but because the algorithm was faulty. The algorithm can then be examined further.
If an attacker manages to get hold of the plaintext and the cipher text, then for an $n$-bit key, there are at most $2^n$ keys to try.
Third parties can eavesdrop on your traffic at practically every [[Computer Networks|stage over the network]]. So that two people can communicate securely we need to use **end-to-end encryption**. To make sure that data has not been tampered with, we use **digital signatures**, which is linked to the message and the sender. If the message changes, then the signature is invalid, and only the sender has the key to sign.
To prevent anyone from denying that they signed something, we must use **asymmetric digital signatures** where we can verify the signature without needing the signing key.
# Building Blocks
## Cryptographic [[Hashes]]
Hash functions map a message to a hash. These hash algorithms are designed so that the original message is really hard to find out (**hard to reverse**). This also makes it really hard to **create another message** with the same hash.

Cryptographic algorithms require unpredictable numbers for key generation. It is common knowledge however that computers cannot create truly random numbers since CPU's are deterministic. Instead we must use cryptographically secure pseudo-random number generators (PRNG). The pseudo-random number is created from a seed such as the date and time.
# Symmetric Cryptography
Symmetric Cryptography uses a **single key** to encrypt plaintext and decrypt cipher text. It protects confidentiality and with symmetric signatures it provides integrity and authentication.
A type of cipher called the block cipher divides the data into blocks and maps each plaintext block to a cipher block. This is commonly known as Electronic Codebook (ECB). It however allows an attacker to reorder blocks and in most cases reveals repetitions, since the same plaintext transforms to the same cipher text.
To prevent this, Cipher Block Chaining (CBC) is used. Each previous block is linked to the next one.
![[Pasted image 20231007193353.png]]
Not all messages have a size a multiple of the block size, this is where we must use **padding** to add extra bytes. We do not add extra zero bytes, instead we add bytes containing the padding length. Ex: `6e 20 74 05 05 05 05 05`. 
## AES Example in OpenSSL
```c
#include <openssl/evp.h>

static unsigned char *parsehex(const char *s, size_t len) {
  unsigned char *buf = calloc(len, 1);
  for (int i = 0; s[i]; i++)

    buf[i/2] |= (s[i]%16 + (s[i]>>6)*9) << 4*(1-i%2);
  return buf;

}

int main(int argc, char **argv) {  
	const EVP_CIPHER *type = EVP_aes_128_cbc();  
	EVP_CIPHER_CTX *ctx = EVP_CIPHER_CTX_new();  
	unsigned char *key = parsehex(argv[1], EVP_CIPHER_key_length(type)); 
	unsigned char *iv = parsehex(argv[2], EVP_CIPHER_iv_length(type));
	int outlen;  
	ssize_t r;  
	EVP_CipherInit(ctx, type, key, iv, ENCRYPT); 
	do {
	    r = read(0, inbuf, sizeof(inbuf));
	    if (r > 0) {
			EVP_CipherUpdate(ctx, outbuf, &outlen, inbuf, r); 
		} else {
		    EVP_CipherFinal(ctx, outbuf, &outlen);
		}
		write(1, outbuf, outlen); 
	} while (r > 0); 
	EVP_CIPHER_CTX_free(ctx);
}
```
## Symmetric Signature
Message authentication code (MAC) provides signatures for messages, by requiring the key for generating a valid MAC and validating it. It provides integrity, authentication but no non-repudiation. Hash MAC combines the key with the message and then applies a hash function. To verify the message, we compute the signature and verify if it is identical to the one received.
# Asymmetric Cryptography
Asymmetric cryptography (also known as public key encryption) requires the use of two keys: one for the sender and one for the receiver.
# Key Management
# Blockchain


---
References: