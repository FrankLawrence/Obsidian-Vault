# Basic Usage
```bash
# List public keys you have stored (yours and other people's keys)
gpg --list-keys
# List private keys
gpg --list-secret-keys
```
## Creating new private key:
You can generate a new key interactively with `gpg --gen-key`. Export it in armored (ASCII) format and keep the backup somewhere safe (maybe even paper).
```bash
gpg --export-secret-keys --armor XXXXXXXX > ./my-priv-gpg-key.asc
```
Delete a private key from local storage with 
```bash
# pubic
gpg --delete-keys XXXXXXXX
# private
gpg --delete-secret-keys XXXXXXXX
```
## Importing a key
You can import private and public keys with the same command. Armored exports end in `.asc`
```bash
gpg --import ./my-priv-gpg-key.asc
```
## Exporting public key
```bash
gpg --output public.txt --armor --export example@test.org
```
## Pushing public key to key server
```bash
gpg --keyserver hkp://pgp.mit.edu --send-keys XXXXXXXX
```
## Generate Revocation Key
```bash
gpg --output revoke.asc --gen-revoke <key-ID>
```
You need to import the revocation certificate into your public keyring: 
```bash
gpg --import revoke.asc
```
You can revoke your public key on a key-server e.g. for `pgp.mit.edu`:
```bash
gpg --keyserver pgp.mit.edu --send-keys <key-ID>
```
# Encryption
## Symmetric
```bash
gpg --symmetric message.txt
# Prompts you for a passphrase
# Creates message.txt.gpg (binary)

gpg --armor --symmetric message.txt
# Same, but ASCII format output instead of binary
# Creates message.txt.asc (ASCII)

# Specify the encryption algorithm
gpg --symmetric --cipher-algo AES256

# Get the list of cipher algorithms
gpg --version
# E.g. 3DES, BLOWFISH, AES256, TWOFISH

# Specify output file
gpg --output message.txt.gpg --symmetric message.txt
# Encrypt and sign (all in the single output file)
gpg --sign --symmetric message.txt
```
To encrypt an entire directory, first archive it with `tar -cf dirname.tar.gz dirname/`. To unarchive use `tar -xf dirname.tar.gz`
## Asymmetric
First [[#Importing a key]], then you can encrypt messages only they can decrypt
```bash
# This will prompt and ask the recipient's email address
# and you will have to enter the ID or email
gpg --encrypt message.txt

# or specify the recipient as the -r argument
gpg  --recipient example@test.org --encrypt message.txt

# Encrypted output will be in message.txt.gpg
# Encrypt and store in ASCII format (message.txt.asc)
# Both binary and ASCII versions decrypt the same
gpg  --armor --recipient example@test.org --encrypt message.txt

# Encrypt and sign at the same time
gpg --encrypt --sign --recipient example@test.org message.txt

# Specify output file
gpg --output message.txt.gpg 
```
## Decrpyting
```bash
gpg -d message.txt.gpg

# Decrypt and put output in decrypted.txt
gpg --decrypt message.txt.gpg > decrypted.txt
```
# Signatures
Signing does not secure the information of guarantee confidentiality, instead it only proves the authenticity of the message.
```bash
gpg --clearsign message.txt
# Outputs message.txt.asc in plain text, suitable
# for pasting in an email or posting online

gpg --sign message.txt
# Outputs message.txt.gpg a binary file

# Both of these can be verified with --decrypt
# but they are not _actually_ encrypted.
gpg --decrypt message.txt.gpg
gpg --decrypt message.txt.asc
# This will print out the message along with the signature info
```
You can also have a separate file with the signature called a **detached signature**:
```bash
# Create a separate signature file
gpg --detach-sign message.txt
# Will create message.txt.sig

# This verify will automatically check the signature
# against a file named "message.txt"
gpg --verify message.txt.sig

# Specify the file to check it against
gpg --verify some_signature.sig ./message.txt
```
# Good Practice
The primary key is your online identity. It represents your legitimacy and can be signed by others. It should be kept very secret and most likely held offline in a safe place.
It is very inconvenient to use, so we often use subkeys for encryption. 
The subkey is as secure as the primary key but is only signed by your own primary key. When the subkey is compromised, you can issue a revocation key and create a new subkey.
# How to Verify a GPG Signature
1) Import public GPG key of author
2) Obtain signature file
3) Verify signature file
## Import public GPG key
```bash
gpg --import [public-key-file]
```
**OR** download directly from keyserver
```bash
gpg --keyserver pgp.mit.edu --recv [Key-ID]
```
## Obtain signature file
There are attached and detached signatures. Signature files end with `.sig` 
## Verify signature file
```bash
# Verify only
gpg --verify [signature-file]
# Verify and extract original document from attached signature
gpg --output [original-filename] [signature-file]
```
For detached signature you can specify the file names explicitly.
```bash
gpg --verify [signature-file] [original-file]
````