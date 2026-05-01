---
title: What is a One-Time Pad?
author: Floyd
---

## What is a one-time pad?

Broadly speaking, a one-time pad is a way to encipher information that is impossible to decipher from the ciphertext alone when [certain criteria]({% post_url 2026-05-04-information-theoretic-security %}) are met.

Here's an example that illustrates the basic ideas. This example isn't how [Adelos-Labs/one-and-done](https://github.com/Adelos-Labs/one-and-done) works, but it gets the idea of a one-time pad across.

The basic idea is this:

You create a file w/ random letters (the randomness of how you choose the letters is actually very important, but we'll gloss over that for now). Let's say this file contains: `XMCKL`.

You then share this file w/ a friend via USB so you both have a copy on your computers.

To encipher and decipher, you can use modular arithmetic like this:

  - Encrypt: for each position i, EncipheredMessage[i] = (Plain[i] + Key[i]) mod 26
  - Decrypt: Plain[i] = (EncipheredMessage[i] − Key[i]) mod 26

In python code, enciphering a message (`HELLO`) looks like:

```python
message = 'HELLO'
key = 'XMCKL'
enciphered_message = ''


def get_character_number(character: str) -> int:
    return ord(character.upper()) - ord('A')


for index, message_character in enumerate(message):
    message_character_number = get_character_number(message_character)  # e.g. A -> 0

    key_character = key[index]
    key_character_number = get_character_number(key_character)

    enciphered_character_number = (message_character_number + key_character_number) % 26
    enciphered_message += chr(enciphered_character_number + ord('A'))

print(enciphered_message)
```

This yields `EQNVZ`.

You send your friend this message.

On their end, they can decipher with:

```python
enciphered_message = 'EQNVZ'
key = 'XMCKL'
deciphered_message = ''


def get_character_number(character: str) -> int:
    return ord(character.upper()) - ord('A')


for index, enciphered_message_character in enumerate(enciphered_message):
    enciphered_message_character_number = get_character_number(enciphered_message_character)  # e.g. B -> 1

    key_character = key[index]
    key_character_number = get_character_number(key_character)

    deciphered_character_number = (enciphered_message_character_number - key_character_number) % 26  # This is the only line which is different than encoding
    deciphered_message += chr(deciphered_character_number + ord('A'))

print(deciphered_message)
```

So a one-time pad relies on a shared secret key and a reversible operation, such as modular addition (what we did here) or XOR, that combines each plaintext symbol with its own independent random key symbol.

The term 'one-time' is relevant because once part of a key file is used to encipher a message, it should never be used again (it is for one-time use). This is an important security requirement we'll dive into more in [a post on Information-Theoretic security]({% post_url 2026-05-04-information-theoretic-security %}).

Happy ciphering!
