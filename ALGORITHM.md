# Algorithm

## Basic Summary

1.  When the user registers and creates their password, the password is **never sent to the server**,
    even in hashed form. Instead, a client-side script uses a public-key cryptography system (I use
    elliptic-curve, specifically ECDH) to generate a keypair from the password (or a hash of their
    password). The public key (only) is sent to the server and stored in its records.

2.  When the user logs in, the password is **still not sent to the server**. Instead, the server generates
    a random authentication token and encrypts it using the public key corresponding to the username that
    was used to log in (which *is* sent to the server, unlike the password). Only the possessor or the
    corresponding private key (which can only be computed from the user's password) can decrypt it.

3.  The client then decrypts the token using the private key it computed from the inputted password. It
    sends the decrypted token back to the server. The server compares it with the token it originally
    generated, and authenticates the user if and only if the two tokens match. (For an added layer of
    security at a minor speed cost, the server can destroy the unencrypted token after it encrypts it
    with the user's public key, preserving only the encrypted token. It would then also encrypt the token
    returned to it by the client with this public key and compare the encrypted forms.)
