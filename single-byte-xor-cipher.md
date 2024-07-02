# Single-byte XOR cipher

The hex encoded string:

```
1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736
```

... has been XOR'd against a single character. Find the key, decrypt the message.

You can do this by hand. But don't: write code to do it for you.

How? Devise some method for "scoring" a piece of English plaintext. Character frequency is a good metric. Evaluate each output and choose the one with the best score.

***

We can solve this using many methods. Since we know our correct result will be English plaintext and not some sort of weird ASCII character set, we could solve it by scoring a piece of English plaintext, as recommended by the question.&#x20;

```python
# English frequencies used letter provided by ChatGPT
english_letter_freq = {
    'a': 0.0651738, 'b': 0.0124248, 'c': 0.0217339, 'd': 0.0349835,
    'e': 0.1041442, 'f': 0.0197881, 'g': 0.0158610, 'h': 0.0492888,
    'i': 0.0558094, 'j': 0.0009033, 'k': 0.0050529, 'l': 0.0331490,
    'm': 0.0202124, 'n': 0.0564513, 'o': 0.0596302, 'p': 0.0137645,
    'q': 0.0008606, 'r': 0.0497563, 's': 0.0515760, 't': 0.0729357,
    'u': 0.0225134, 'v': 0.0082903, 'w': 0.0171272, 'x': 0.0013692,
    'y': 0.0145984, 'z': 0.0007836, ' ': 0.1918182
}

# Variables to store final result
best_score = 0
best_message = ""
best_key = None

hex_string = "1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736"
hex_bytes = bytes.fromhex(hex_string)

# Iterate over all possible single-byte keys
for xored_key in range(256):
    xored_message = bytearray()

    # XOR each byte of the hex string with the current key
    for byte in hex_bytes:
        xored_message.append(byte ^ xored_key)

    try:
        # Decode the XORed message
        decrypted_text = xored_message.decode(errors='ignore')
        current_score = 0

        # Calculate the score based on English character frequency
        for text in decrypted_text:
            for char in english_letter_freq:
                if char == text.lower():
                    current_score += english_letter_freq[char]
                
                # Update the best score and message if the current is higer
                if current_score > best_score:
                    best_score = current_score
                    best_message = decrypted_text
                    best_key = xored_key

    except UnicodeError:
        pass    

print(f"Key in letter: {chr(best_key)}, Key in digit: {best_key}\nMessage: {best_message}")
```

<figure><img src=".gitbook/assets/Single-byte XOR cipher0.png" alt=""><figcaption></figcaption></figure>

We can also solve it by matching it with English plain text.\
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

<figure><img src=".gitbook/assets/Single-byte XOR cipher1.png" alt=""><figcaption></figcaption></figure>
