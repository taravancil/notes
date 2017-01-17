# Practical Cryptography with Go - Kyle Isom

[https://leanpub.com/gocrypto/read]()

## 1. Introduction

## 2. Engineering concerns and platform security

### Authentication

Any method for verifying the identity of some party such as:

1. Something you know (a password)
2. Something you have (auth token)
3. Something you are (fingerprint)

### Authorisation

Access control mechanism. Common models: list-based and role-based.

### Auditing

Maintaining audit logs, ensuring they haven't been tampered with.

### On Errors

Chosen ciphertext attacks rely on things like padding oracles, which utilize
errors to infer useful information about the cipher. Best to use generic errors
like "encryption failed".

### Input sanitisation

Blacklisting - default allow all then mark disallowed inputs
Whitelisting - default block all then mark acceptable inputs

### Memory

Sensitive data sometimes needs to be loaded into memory. Go is a managed-memory
languge, which means user has little control over memory.

#### Process isolation

Prevents attacker from accessing a process's memory space.

Memory is not the only concern. Secrets can be swapped to disk as well, for
example when a laptop is sleeping, using a peripheral that has direct memory
access (most do), if a program crashes and dumps core, that's often written to
disk. CPU caches can also store secrets, which may be an additional attack
surface (especially on shared environments).

#### Mitigating risks

Use stack to prevent secrets from entering heap, zero sensitive data in memory
once done with it (though not always effective). Ultimately no guarantee that
disk can be securely erased. If sector on disk fails, disk controller may mark
the block as bad and try to copy to another sector. Disk controller can also be
subverted. Disk drives contain drive controllers that are poorly audited.

TL;DR memory is a difficult attack surface to secure. Helpful to ask these
questions for each secret:

1. Does it live on disk for long-term storage? If so who has access to it? What
   auth mechs enforce access control?
2. When loaded in memory, who owns it? How long does it live in memory? What
   happens when no longer in use?
3. If secret is on VM, how much trust can be placed in parties that can access
   the host? Can other tenants access the secrets? What happens when machine is
   decommisioned?

### Randomness

Just use `/dev/urandom`! `crypto/rand.Reader` in Go's stdlib uses urandom.
Ensuring sufficient randomness is also an important concern. On VMs one might
want to include external source of entropy in the kernel's PRNG.

### Time

Challenging to sync time between peers in a system. This is relevant to
cryptography because we often need to check if a key has expired.


Real time clocks are helpful, but not all systems have them, also drift based on
hardware properties of the machine. Network time sync works often, but subject
to network failures. VMs subject to clock on the host. Using clock as a
monotonic counter also sometimes problematic. A clock that has drifted forward
may be corrected (via NTP), which results in the counter stepping backward.

TL;DR treat clocks with suspicion.

### Side channels

A side channel is attack surface based on the physical implementation. Alg may
be correct, but hardware can leak information about private key or ciphertext. A
few different categories include:

* timing: observing time required to carry out an operation
* power consumption
* power glitching: power to system is glitched, which causes system to fail in
  unexpected ways and reveal information about keys or messages
* EM leaks: some circuits leak electromagnetic emissions

Crypto implementations must be designed with these attacks in mind (for example,
by using constant time functions in `crypto/subtle`)

### Public key infrastructure

Needs to be able to handle key rotation, key revocation. Marking keys as expired
is easy, but effectively communicating that is a bit more complicated.
Certificate revocation lists can be distributed for TLS, and user agents
periodicalyl re download them, but the period between a certificate expiring and
the list being redownloaded allows for compromise. OCSP is an improvement - it
allows for querying an authoratative source about the status of a certificate,
but of course that is subject to network outages.

### Crypto does not provide...

* message length obscurity
* anonymity
* protection against replay attacks (spoofing previously-sent messages)

### Follow up reading

* Brumley, Boneh - "Remote timing attacks are practical"
* Percival - "How to zero a buffer"

