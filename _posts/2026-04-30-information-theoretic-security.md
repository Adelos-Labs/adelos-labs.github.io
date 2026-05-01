---
title: Information-Theoretic Security
author: Floyd
---

## What is Information-Theoretic Security

A system is information-theoretically secure if its security can be proven without assuming limits on the attacker's time and computing power.

In a landmark paper entitled ["Communication Theory of Secrecy Systems"](https://pages.cs.wisc.edu/~rist/642-spring-2014/shannon-secrecy.pdf) and published in 1949, Claude Shannon introduced the idea of something being provably secure against infinite computation and time.

This means that a system which is information-theoretically secure can't be cracked by advances number theory or quantum computing. This is possible because the ciphertext reveals no information about which plaintext was chosen among messages of that length. In other words, if Alice sends the ciphertext `EQNVZ` to Bob and Eve is trying to decrypt it, it could be any five letter message and seeing it gives Eve no new information about which five letter message it enciphers. It could be `HELLO`, `MAPLE`, or `ZEBRA`.

Mathematically, this can be represented as:

```
P(M = m) = P(M = m | C = c)
```

Broken down:

- M is the random variable for the plaintext message.
- m is one specific possible plaintext, like "attack at dawn".
- C is the random variable for the ciphertext.
- c is one specific ciphertext the attacker observes.
- `P(M = m)` is the attacker’s prior belief that the message was m before seeing the ciphertext.
- `P(M = m | C = c)` is the attacker’s updated belief after seeing ciphertext c.

The `|` means "given that" or "conditioned on."

So in total:

> The attacker's prior belief that a message was m before a ciphertext is the same as the belief that the message is m after seeing the ciphertext.

Seeing the ciphertext doesn't give the attacker any new information, so it doesn't allow him/her to update the probability of m being a certain message.

## How is it Achieved?

One way to achieve information-theoretic security is to use a [one-time pad]({% post_url 2026-05-02-what-is-a-one-time-pad %}). To be information-theoretically secure, a one-time pad's key must be:

1. Truly random
2. At least as long as the message (no repeating the key)
3. Used only once
4. Kept secret (More specifically, the part of the key used to encode messages must be kept secret forever. Exposing part of the key which has never been used and never will doesn't pose a security concern unless it reveals something about the random key generator.)

If any one of these is not true, the system is not information-theoretically secure.

Here's how this works for [Adelos-Labs/one-and-done](https://github.com/Adelos-Labs/one-and-done) one-time pad implementation:

1. Key is truly random: By default, we use golang's [crypto/rand](https://pkg.go.dev/crypto/rand) library to generate keys. Crypto/rand is a cryptographically secure random source backed by the OS which is good for ordinary cryptographic key generation, but is not information-theoretic complete. To be fully information-theoretic complete, the key material must come from a source that produces genuinely random bits with enough entropy. A well-designed hardware RNG is a common source for these bits. If you have a hardware RNG, you can use that to generate keys that one-and-done can use.
2. Key is at least as long as the message (no repeating): We generate keys of a specified length and don't let you encipher anything longer than the remaining key length.
3. Key is used only once: We only use a key once and delete content you use from the start of the key file.
4. Key is kept secret: This is up to you!
