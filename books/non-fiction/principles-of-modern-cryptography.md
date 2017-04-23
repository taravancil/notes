# Principles of Modern Cryptography - Dan Boneh, Victor Shoup (2016)

## 1. Introduction

Forthcoming

## 2. Encryption

### 2.1 Introduction

The problem that encryption solves: how can Alice and Bob communicate secretly over an insecure
channel? Or how can Alice store a file on disk without risking that someone unauthorized can
read it?

The methods in this chapter do not provide:

* Multiple message encryption per key
* Message integrity
* A mechanism for establishing a shared key between Alice and Bob

### 2.2 Shannon ciphers and perfect security

#### 2.2.1 Shannon ciphers

A **Shannon cipher** is a pair ε = (𝐸, 𝑫) of functions.

* The function 𝐸 (the **encryption function**) takes a key 𝑘 and a message 𝑚 and produces a
  ciphertext 𝑐. So 𝑐 = 𝐸(𝑘, 𝑚), and we say that 𝑐 is the encryption of 𝑚 under 𝑘.
* The function 𝑫 (the **decryption function**) takes 𝑘 and a ciphertext 𝑐 and produces a plaintext
  message 𝑚. So 𝑚 = 𝑫(𝑘, 𝑐) and we say that 𝑚 is the decryption of 𝑐 under 𝑘.
* We require that decryption "undoes" encryption, that is, the cipher must satisfy the following
  **correctness property**: for all keys 𝑘 and all messages 𝑚, we have: 𝑫(𝑘, 𝐸(𝑘, 𝑚)) = 𝑚.

Another formation of this is 𝒦 is the set of all keys (keyspace), ℳ  is set of all messages
(message space), and 𝒞 is the set of all ciphertexts, (ciphertext space):

𝐸: 𝒦  ⨯ ℳ  → 𝒞,
𝑫: 𝒦  ⨯ 𝒞  → ℳ,.

And we also say that ℰ is defined over (𝒦, ℳ,, 𝒞).

So if Alice wants to send an encrypted message to Bob, they must agree in advance on a key that
exists in the keyspace. She encrypts 𝑚 under 𝑘 and Bob decrypts 𝑐 under 𝑘. We must assume that the
ciphertext is not modified in transit, otherwise this won't work.

*Example 2.1* A one-time pad is a Shannon cipher where the keys, and messages are bit strings of the same length. The encryption function is defined as follows:

𝐸(𝑘, 𝑚) := 𝑘 XOR 𝑚

and the decryption function is:

𝑫(𝑘, 𝑐) := 𝑘 XOR 𝑐

