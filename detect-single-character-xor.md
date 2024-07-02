# Detect single-character XOR

One of the 60-character strings in [this file](https://cryptopals.com/static/challenge-data/4.txt) has been encrypted by single-character XOR.

Find it.

(Your code from #3 should help.)

***

```python
import requests
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

url_request = requests.get("https://cryptopals.com/static/challenge-data/4.txt")
hex_strings = url_request.text.strip().split("\n")  # Split the content by new lines

# Iterate over all hex_strings
for hex_string in hex_strings:
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

<figure><img src=".gitbook/assets/Detect single-character XOR 0.png" alt=""><figcaption></figcaption></figure>

