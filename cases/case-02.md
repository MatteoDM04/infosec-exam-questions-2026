# Case 2

Email accounts may be very vulnerable. A few years ago (2013), it was the Prime Minister of Belgium who made the news because his private email account had been hacked.

Assume you are an ISP. Your clients have the possibility to access their email accounts using either a mail client or webmail.

**How would you secure your email service to minimise the risk of client accounts being hacked? Don't forget system security and don't forget to consider the usability of the system. How would you manage the password recovery process (yes, some customers will forget their passwords)?**

*Note: I expect you to make a choice and to defend this choice. Don't present a range of possible solutions. Be sufficiently specific in your implementation (algorithms, key lengths, modes, etc.).*

## Answer

### Security against hacking

**Transport**
- TLS:
    * To prevent eavesdropping when login credentials are being sent, TLS is used on top of TCP.
    * TLS is application independent and could serve as an extra security layer on top of our mail service securiy. 
- TLS 1.3. over 1.2.
    * TLS 1.3. provides only key exchange mechanisms with forward secrecy (= past communications remain secure even if long-term keys are compromised in the future).
    * Version List in stead of version negotiation (lower risk for downgrade).
    * All handshake messages are encrypted. 
    * Uses RSA-PSS in stead of v1.5. In stead of a deterministic approach, randomness (salt) is added before signing, making each signature probabilistic -> Probabilistic Signature Scheme.   
- With parameters AES_256_GCM, and ECDHE for key exchange. 
    * AES_256_GCM: It is an AEAD cipher (Authenticated Encryption with Associated Data) -> you get both confidentiality and integrity in one operation. Encrypts and authenticates application data + padding + content type. Authenticates fragment length and plaintext header fields. Also uses random nonce.
    * ECDHE: Both parties generate ephemeral EC keypairs, exchange public keys, derive a shared secret -> establishes a fresh session key. All actual data is encrypted and authenticated using that session key. Protects against replay attacks. 

**Authentication**
- Implement a rate limit to protect against brute force login attacks. If this limit is broken, the user has to verify using MFA/eID/...
- Multi-factor authentication: When the user registers, they get an email, set a password and set up MFA (SMS (simple) or authentication application on phone (more secure)).

**Storage**
- Argon2id
    * The app hashes the entered password
    * It compares the result to the stored hash
    * If they match, authentication succeeds
    * Conceptual: password123 + random salt + memory-intensive computation + multiple iterations = Argon2id hash
- Emails are encrypted at rest (AES-256-GCM)
    * Protects against physical theft, data exposure, ...
    * Encryption keys are stored in a Hardware Security Module (HSM) and rotated periodically.

**System Security**
- Network Firewall
    * Allow only necessary ports: 993 - IMAPS, 465/587 - SMTPS (limit attack surface).
- Application Firewall
    * In front of the webmail server, filters malicious HTTP requests before they reach the application.
- Sender Policy Framework (SPF): mechanism to allow which servers could send emails with your domain name.
- Provide mails with digital signature.
- Separation of webmail servers and storage servers.
    * Storage servers are never directly exposed to the public internet.

**Password Recovery**
- When the user askes to recover, a code will be sent by text (or better: shown in an authenticator app). The user then has to enter this code on the webbrowser, proving that he is in possession of the phone, and is therefore (most likely) able to authenticate himself as the owner of the account. 
- In case of a lost authentication device, the user can always authenticate itself via their eID. This is not the most user friendly, but it should not happen too often per user. 
- When an authentication devide is compromised, the MFA has to be revoked and initiated all over again. Also all open sessions have to be invalidated.

### Security versus Usability
The user logs in the first time with full login and they have the possibility to stay logged in on that device for a limited amount of time (e.g. 30 days). After this period, they are asked to login again (with MFA).
The length of period has to be chosen carefully. A longer period means less security but more usability...

### Sources 
- Questions_Martijn_Hendrik
- 2024_Information_Security_Exam_Questions_GVdV
- IS_UG_3_6_Appl_TLS (notes)
- <https://support.cloud86.io/hc/nl/articles/28141198052381-E-mail-beveiliging-en-spoofing-SPF-DKIM-DMARC>
- Claude-AI for further insights. 

_Status: Complete_  
_Done by: Hann1bal20_