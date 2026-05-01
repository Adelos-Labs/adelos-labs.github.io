---
title: Information-Theoretic Security
---

## What is Information-Theoretic Security

Information-theoretic security is mathematically proven to be robust against adversaries with infinite computing resources and time.

In a landmark paper originally entitled ["A Mathematical Theory of Cryptography"](https://pages.cs.wisc.edu/~rist/642-spring-2014/shannon-secrecy.pdf) and published in 1946, Claude Shannon introduced the idea of something being provably secure against infinite computation and time.

This means that a system which is information-theoretic secure can't be cracked by advances in number theory, quantum computing, or even brute-force attacks. This is possible because the ciphertext reveals no information about which plaintext was chosen among messages of that length. In other words, if Alice sends the ciphertext `EQNVZ` to Bob and Eve is trying to decrypt it, all five character messages are equally likely and there's no way for Eve to know which five character message this is. It could equally be `HELLO`, `MAPLE`, or `U MAD`.

Mathematically, this can be represented as:

```
P(M = m) = P(M = m | C = c)
```

Broken down:

- M is the random variable for the plaintext message.
- m is one specific possible plaintext, like "attack at dawn".
- C is the random variable for the ciphertext.
- c is one specific ciphertext the attacker observes.
- P(M = m) is the attacker’s prior belief that the message was m before seeing the ciphertext.
- P(M = m | C = c) is the attacker’s updated belief after seeing ciphertext c.

The | means "given that" or "conditioned on."

So in total:

> The attacker's prior belief that a message was m before a ciphertext is the same as the belief that the message is m after seeing the ciphertext.

Seeing the ciphertext doesn't give the attacker any new information, so it doesn't allow him/her to update the probability of m being a certain message.

## How is it Achieved?

One way to achieve information-theoretic security is to use a [one-time pad](./2026-04-25-what-is-a-one-time-pad). To be information-theoretic secure, a one-time pad's key must be:

1. Truly random
2. At least as long as the message (no repeating the key)
3. Used only once
4. Kept secret

If any one of these is not true, the system is not information-theoretically secure.

Here's how this works for [Adelos-Labs/one-and-done](https://github.com/Adelos-Labs/one-and-done) one-time pad implementation:

1. Key is truly random: By default, we use golang's [crypto/rand](https://pkg.go.dev/crypto/rand) library to generate keys. Crypto/rand is a [CSPRNG](https://en.wikipedia.org/wiki/Cryptographically_secure_pseudorandom_number_generator) which offers some level of security, but is not fully information-theoretic complete. To be fully information-theoretic complete, one would want to use a hardware RNG. If you have a hardware RNG, you can use that to generate keys that one-and-done can use.
2. Key is at least as long as the message (no repeating): We generate keys of a specified length and don't let you encipher anything longer than the remaining key length.
3. Key is used only once: We only use a key once and delete content you use from the start of the key file.
4. Key is kept secret: This is up to you!

