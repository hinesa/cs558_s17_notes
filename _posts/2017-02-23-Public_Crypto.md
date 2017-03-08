---
layout: notes
title: Public Crypto
scribe: Alex Hines
---


Today **a collision was found in SHA-1**; this is is fairly historic! Biggest crypto news since MD5 in 2004.
  - this **isn't terrible news**; Google Team has been pushing SHA-1 out of use with certificates for some time now.
  - You can look at the certificates used by your browser to see that they aren't compromised. 
  - *shattered.io* presents the collision. 
  
Knowing *what public key belongs to which party* is very important; *identity mis-binding* is a serious problem. 

Certificates have a *heirarchy*: id of subject, id of user, and subject's public key; all signed by issuer.
  - ie. Google == subject, Google Internet Authority == issuer. GIA signs cert naming the parties and including the pk of Google.
  - This ensures that the public key is for whom you think it is for; tampering with the pk will mess up the signature.
  
But where is the *root of trust*? How do we know the *signer is who they say they are?*
  - Your *browser ships* with trusted public keys. They are *hardcoded into the software*. This is called the **root store**.

This heirarchy of keys and signatures of keys is called **"public key infrastructure"**. 
  - *Trusted roots* have their pk in the *root store*.
  - There is on the order of 40 Trusted Roots.
  
*Certificates last longer on the top of the heirarchy* (~200 years!); down the chain it drops to only a few months.
  - if a certificate gets compromised, the parent revokes it.
  
**DigiNotar Attack**: a trusted root (Netherlands) was compromised and as a result certificates could be issued as the attackers pleased.
  - https://en.wikipedia.org/wiki/DigiNotar#Issuance_of_fraudulent_certificates
  - The attackers could pose as anybody they wanted to be. Tens of thousands of Iranian GMail users were targeted. 
  - The sophistication of the attack suggested that a state actor was responsible;
      -an investigation by the Dutch government suspected Iranian government involvement.
  
So *why is the SHA-1 break important*?
  - **its about the signatures**. Don't want them to be forged.
  - **Flame Attack** did it with MD5. Again targetting Iranians, this time government officials. "20x more complicated than Stuxnet".
    - (https://en.wikipedia.org/wiki/Flame_(malware))
  - If you can *find 2 messages that collide in the hash function, they sign the same*.
    - Get the signer to sign a message, find a collision of your choice, swap out the messages and use the old signature. 
    
**Key Exchange**: for symmetric key schemes.

For a group G and generator g:

A sends B (g^x) for some random secret x. B sends A (g^y) for some random secret y. *Both compute key = g^(xy)* ie (g^x)^y and (g^y)^x.

*Discrete log problem*: x, g ==> g^x is easy. g, y ==> x s.t. y = g^x hard. g^x, g^y, and g may be known by anyone. 

  - Issues: this method allows A & B to generate a shared secret key, but does not ensure the identities of the parties. Secure only against a passive (listening) adversary. 

  - *Diffie Hellman exchange*; is used in TLS. Incorporates additional security like B signing (g^b).
      - **1-Sided Authentication**; A knows B is who they say they are, but B doesn't know A at all. Best for client-server applications.
      - In practice, there would be a chain of certification used in order to trust B. 
  
