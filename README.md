# Crypto
[[عربي]](readme.ar.md)

This library is a binding of libcrypto to Alusus, which contains functions for many encryption algorithms.
However, this is an early version that currently only supports the md5 function.

## Adding to the Project

We can install this library using the following statements:
```
import "Srl/Console";
import "Apm";
Apm.importFile("Alusus/Crypto");
```

## Example

```
import "Srl/Console";
import "Apm";
Apm.importFile("Alusus/Crypto");

use Srl;

// define a variable to hold a text we want to md5.
def text: String("Crypto binding for Alusus");

// call the md5 function on the text we defined and print the result.
Console.print("%s\n", Crypto.md5(text).buf);

// Load a private key and create a digital signature for text.
def pkey: ref[Crypto.EvpPkey](Crypto.EvpPkey.readPem("rsakey"));
Console.print("hmac signature: %s\n", Crypto.sign(text, pkey, Crypto.EvpMd.sha256()).buf);
```

## Classes and Functions

### md5

```
func md5 (plainText: String): String;
```

This function applies the md5 algorithm on a given text.

`plainText` the text we want to hash.

Returns the md5 hash of the given text.

```
@expname[MD5]
func md5 (planText: CharsPtr, lingth: ArchInt, hashedText: ref[array[Char,16]]): CharsPtr;
```
This function is used by the previous function, essentially it does the same work but
using lower level types.

`plainText`: the text as a pointer to a char.

`length`: the length of the text.

`hashedText`: a reference to the array we want to store the hashed text in.

### encodeBase64

```
func encodeBase64(input: ptr, inputSize: ArchInt): String;
```

```
func encodeBase64(input: String): String;
```

This function applies the Base64 encode algorithm on a given data.

The first version of this function receives a pointer to the data to be encoded, and the size of
that data, while the second version receives a string to be encoded. The function returns the
Base64 encoded text.

### decodeBase64

```
func decodeBase64(input: String, output: ref[ptr], outputSize: ref[ArchInt]);
```

* `input`: The Base64 encoded text we want to decode.
* `output`: A reference to the pointer that will receive the pointer to the decoding result. The
  function will allocate the needed buffer and will leave it to the caller to release the buffer
  after use.
* `outputSize`: A reference to an integer to receive the size of the output.

```
func decodeBase64(input: String): String;
```

Decodes a given Base64 encoded text.

`input`: The Base64 encoded text we want to decode.

Returns the decoded text.

### EvpPkey

Used to load private keys that are used in envelope encryption operations.

It has the following functions:

#### readPem

```
func readPem(filename: CharsPtr): ref[EvpPkey];
```

Loads a private key from a file in PEM format.

#### free

```
handler this.free();
```

Decreases the reference count of the key, freeing it if the count reaches 0.

### EvpMd

The message digest algorithm used in encryption operations. This type is used in operations that require
encryption, such as the `sign` function.

This type has the following constructor functions:

```
func sha224(): ref[EvpMd];
func sha256(): ref[EvpMd];
func sha512_224(): ref[EvpMd];
func sha512_256(): ref[EvpMd];
func sha384(): ref[EvpMd];
func sha512(): ref[EvpMd];
```

### sign

```
func sign(data: CharsPtr, pkey: ref[EvpPkey], tp: ref[EvpMd]): String;
```

Creates a digital signature for the given raw string.

* `data`: The data for which to create the signature.
* `pkey`: The private key used for the signature.
* `tp`: The type of message digest to use in the encruyption.

The function returns a signature in base64 format.

```
func sign(
    input: ptr, inputSize: ArchInt,
    pkey: ref[EvpPkey],
    tp: ref[EvpMd],
    output: CharsPtr, outputSize: ref[ArchInt]
);
```

A binary version of the sign function. This function receives data in binary format and generates
a signature also in binary format (rather than base64).

* `input`: The data to be signed.
* `inputSize`: The size of the input data, in bytes.
* `pkey`: The private key used for the signature.
* `tp`: The type of message digest to use in the encruyption.
* `output`: A pointer to a buffer to hold the signature. The buffer must be big enough to hold
  the signature.
* `outputSize`: A reference to an integer initially containing the size of the provided buffer.
  After the call this will be updated to the actual size of the signature.

---

## License

Copyright (C) 2026 Sarmad Abdullah

This project is licensed under the Apache License v2.0. See the `LICENSE` file for details.


