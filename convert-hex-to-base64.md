# Convert hex to base64

### Task Objective

#### Convert hex to base64

Hex\_string: `49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d`&#x20;

Expected Result: `SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t`

***

**Solution:**

```python
#!/usr/bin/env python3
import base64

hex_string = "49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d"

string_bytes = bytes.fromhex(hex_string)    # Convert hex to bytes
b64_string = base64.b64encode(string_bytes) # Encode bytes to base64

print(b64_string.decode())      # Print the base64 encoded string by decoding from bytes strings
```

<figure><img src=".gitbook/assets/Convert hex to base64.png" alt=""><figcaption></figcaption></figure>
