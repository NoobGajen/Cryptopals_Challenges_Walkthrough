# Single-byte XOR cipher

The hex encoded string:

```
1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736
```

... has been XOR'd against a single character. Find the key, decrypt the message.

You can do this by hand. But don't: write code to do it for you.

How? Devise some method for "scoring" a piece of English plaintext. Character frequency is a good metric. Evaluate each output and choose the one with the best score.

***

We can solve this using many methods. Since we know our correct result will be English plaintext and not some sort of weird ASCII character set, we could solve it by scoring a piece of English plaintext, as recommended by the question. However, I am going to solve it by matching it with English plain text.

First of all, to solve this challenge, we need to convert the given hexadecimal value into byte form so we can perform further actions. After converting it, we will XOR every byte of the hexadecimal bytes with all the possible ASCII characters, which will be in the range of decimal 0 to 255. Then, we can match every character from the XORed result with normal English plain text. Finally, we get our final message by decoding those matched bytes into a normal string.

```python
import re

regex_pattern = re.compile(r"[A-Za-z !@#$%^&*()'\",./<>?\[\]\\]")
hex_string = "1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736"
hex_bytes = bytes.fromhex(hex_string)

for xored_key in range(256):   # All the possible ASCII char
    xored_message = bytearray()  # Byte Array to store xor_message

    for byte in hex_bytes:
        xored_message.append(byte ^ xored_key)
        # print(xored_message.decode())
    try:
        if all(re.match(regex_pattern, chr(byte)) for byte in xored_message):
            print(f"Key in letter: {chr(xored_key)}, Key in digit: {xored_key}\nMessage: {xored_message.decode()}")
    except UnicodeDecodeError:
        pass
```

<figure><img src=".gitbook/assets/Single-byte XOR cipher.png" alt=""><figcaption></figcaption></figure>

