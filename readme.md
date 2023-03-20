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
```

## Functions

### md5 function

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

