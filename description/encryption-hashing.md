# Encryption

Encryption is one of the most fundamental cybersecurity concepts - and for good reason.

It’s all about protecting data from unauthorized access by converting it to unreadable formats.

There are two main types:

Symmetric: Uses the same key to encrypt and decrypt data. It’s fast, efficient, and great for large volumes of data. (Ex. AES)

Asymmetric: Uses a public key to encrypt and a private key to decrypt. It solves the key exchange problem and enables use cases like secure web browsing, email encryption, and digital signatures. (Ex. RSA)

A real world example: When you visit a website over HTTPS, asymmetric encryption helps initiate the secure connection. After the handshake, symmetric encryption takes over for faster performance.

# Hashing

Hashing is the process of turning data into a fixed-length string using an algorithm. Unlike encryption, it’s one-way - meaning you can hash data, but you can’t “unhash” it.

But here’s the key: the same input will always produce the same output when using the same hashing algorithm.

It’s important to note that hashing is not encryption. It’s not meant for secrecy, it’s meant for integrity. And it’s not secure by default. Hashes can be brute-forced or cracked using rainbow tables if they’re not properly salted.

Some common use cases for hashing algorithms, like SHA256 and MD5, are:

Password Storage: Hash and salt password before saving them to a database.

File Integrity: Verify a file hasn’t been tampered with.

Digital Signatures: Ensure authenticity and data integrity.
