<img width="2360" height="2770" alt="image" src="https://github.com/user-attachments/assets/1c3537fd-41e5-42ae-9672-181c0a1a1339" />


1. TCP Handshake: Before any secure communication occurs, the client and server first establish a basic connection using the TCP handshake process. At this point, the data is not encrypted.

2. Certificate Check: This stage is meant to verify the server’s identity. The client starts the TLS handshake with a “hello” message that offers supported encryption algorithms. The server replies, choosing algorithms and sending its digital certificate, which contains the public key. The client verifies the certificate to ensure it’s from a trusted Certificate Authority (CA).

3. Key Exchange: Once the client validates the certificate, it starts the key exchange process. The client uses the server’s public key (from the certificate) to encrypt the session key. The encrypted session key is sent to the server, and the server can decrypt it using its private key. The change cipher spec message signifies that from this point onward, all messages will be encrypted using the agreed-upon session key and cipher. This step uses asymmetric encryption to securely exchange the session key.

4. Data Transmission: Now, both sides switch to symmetric encryption (faster) using the shared session key. Messages are encrypted and decrypted with the same key. All data exchanged is now secure and private.
