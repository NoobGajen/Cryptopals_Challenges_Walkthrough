# Fixed XOR

#### Fixed XOR

Write a function that takes two equal-length buffers and produces their XOR combination.

If your function works properly, then when you feed it the string:

```
1c0111001f010100061a024b53535009181c
```

... after hex decoding, and when XOR'd against:

```
686974207468652062756c6c277320657965
```

... should produce:

```
746865206b696420646f6e277420706c6179
```

***

```python
def FixedXOR(val1, val2):
    value1_byte = bytes.fromhex(val1)  # Convert the first hex string to bytes
    value2_byte = bytes.fromhex(val2)  # Convert the second hex string to bytes
    
    xor_array = []  # Create an empty list to store XOR results
    for i in range(len(value1_byte)):  # Iterate over the length of one of the byte arrays
        xor_result = value1_byte[i] ^ value2_byte[i]  # Perform XOR on corresponding bytes
        xor_array.append(xor_result)  # Append the XOR result to the list

    xor_bytes = bytes(xor_array)  # Convert the list of XOR results to a bytes object

    result = xor_bytes.hex()  # Convert the bytes object to a hex string
    return result  # Return the hex string
    

# Call the function with the values provided in the question
print(FixedXOR("1c0111001f010100061a024b53535009181c", "686974207468652062756c6c277320657965"))

```

<figure><img src=".gitbook/assets/preforming XOR on 2 value.png" alt=""><figcaption></figcaption></figure>
