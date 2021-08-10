---
layout: post
title: >
    Python2 to Python3 Hex/Str/Byte Conversion Cheatsheet
hide_title: false
tags: ['Tools', 'not security']
excerpt_separator: <!--more-->
author: guy
redirect_from: /2021/02/python2-python3-hexstrbyte-conversion.html
---

If you too have been personally victimized by Python3’s `'str' object has no attribute 'decode' exception` and other string/bytes\-related exceptions, I feel your agony. Trauma from such errors have stopped me from using Python3 for code handling buffers, like POCs for vulnerabilities or CTF exploits. Here’s a reference guide on how to convert between Python3’s hexstr/str/bytes/bytearray.
<!--more-->

# Ultimate Python3 Type Conversion Chart
This has helped me when it was in my personal notes. Hopefully, now it can help you, too.
<style>
table table, td, th {
    border: 1px solid black;    
}
table thead {
    background-color: #ffeb9a;  
}
table {
    border-collapse: collapse;
    display: block;
    max-width: -moz-fit-content;
    max-width: fit-content;
    margin: 0 auto;
    overflow-x: auto;
    white-space: nowrap;  
}
.darkblue {
    color: #13378A;
}

.darkred {
    color: #860835; 
}
</style>

|                                    | have <span class="darkred">mystr</span> of type str                                                     | have <span class="darkred">myhexstr</span> of type hexstr                                           | have <span class="darkred">mybytes</span> of type bytes                                      | have <span class="darkred">mybytearray</span> of type bytearray                       |
|------------------------------------|----------------------------------------------------------------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|----------------------------------------------------------|
| want <span class="darkblue">mystr</span> of type str             | no change needed                                                           | <code><span class="darkblue">mystr</span>=binascii.unhexlify(<span class="darkred">myhexstr</span>).decode(<*encoding_see_bottom_note>)</code> | <code><span class="darkblue">mystr</span> = <span class="darkred">mybytes</span>.decode(<*encoding_see_bottom_note>)</code>               | <code><span class="darkblue">mystr</span> = <span class="darkred">mybytearray</span>.decode(<*encoding_see_bottom_note>)</code>    |
| want <span class="darkblue">hexstr</span> of type hexstr       | <code><span class="darkblue">myhexstr</span> = binascii.hexlify(<span class="darkred">mystr</span>).decode(“utf-8”)</code>                         | no change needed                                                       | <code><span class="darkblue">myhexstr</span> = binascii.hexlify(bytearray(<span class="darkred">mybytes</span>)).decode(“utf-8”)</code> | <code><span class="darkblue">myhexstr</span> = binascii.hexlify(<span class="darkred">mybytearray</span>).decode(“utf-8”)</code> |
| want <span class="darkblue">bytes</span> of type bytes         | <code><span class="darkblue">mybytes</span> = <span class="darkred">mystr</span>.encode()</code>                                                   | <code><span class="darkblue">mybytes</span> = binascii.unhexlify(<span class="darkred">myhexstr</span>)</code>                                 | no change needed                                                | <code><span class="darkblue">mybytes</span> = bytes(<span class="darkred">mybytearray</span>) </code>                            |
| want <span class="darkblue">bytearray</span> of type bytearray | <code><span class="darkblue">mybytearray</span> = bytearray(<span class="darkred">mystr</span>.encode())</code>                                    | <code><span class="darkblue">mybytearray</span> = bytearray(binascii.unhexlify(<span class="darkred">myhexstr</span>))</code>                  | <code><span class="darkblue">mybytearray</span> = bytearray(<span class="darkred">mybytes</span>)</code>                                | no change needed                                         |

<i>\*encoding is usually “ascii”/”utf-8” (they’re the same thing), but for non-English character sets, the encoding might be “utf-16”, “utf-32.”</i>

# Other Python3 Buffer Need\-To\-Know
## Convert `bytes` to/from list of ints
Convenient for binary/mathematical operations such as XOR between two buffers
### bytes to list of ints
```python
myintlist = list(mybytes)
```
###  list of ints to bytes
```python
mybytes = bytes(myintlist)
```
## Read bytes and str from files
### Read data from a file as `str`

Open the file using "r" 
Example: 
```python
mystr = open("C:\whtaguy\orderyogurt.py", "r").read()
```
### Read data from a file as `bytes`
Open the file using "rb"
Example: 
```python
mybytes = open("C:\whtaguy\orderyogurt.py", "rb").read()
```

# Python3 Buffer Type Review
## str
* Example: `mystr = “don’t forget your daily calcium”`
* An immutable unicode string
* Created statically using quotes. 

## hexstring
* Example: “calc” is `“63616c63”`
* A str consisting of hexadecimal numbers \(0\-9, a\-f\). 
* Primarily used to convert binary data to a printable format. 
* Created like str, but contains only hexadecimal numbers

## bytes
* Example: `mybytes = b“bring all the boys to the yard”`
* An immutable array of one\-byte elements
* Created statically by putting the letter “b” before quotes

## bytearray
* Example: `mybytearray = bytearray(b”I’ll grow up to be in a bytearray”) `
* A mutable list of one\-byte elements
* Created through the `bytearray` constructor, which is initialized with a more primitive type \(such as bytes, str\)

{% include image.html url="/assets/img/posts\thats_all_folks.gif" caption="" alt="" %}