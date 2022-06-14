---
layout: post
title:  "Clickhouse UDFs with RowBinary in Python"
subtitle:  "Faster (de)serialization"
date:   2022-06-14 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-06.jpg"
---

# The Problem

Clichouse supports User Defined Functions (UDFs) in python. The main documented way to do these is to process input from `stdin` and write output to `stdout`. The input can be tab separated but the point is that it uses *strings*.

The principle is simple:

```python
#!/usr/bin/python3
import sys
if __name__ == '__main__':
    for line in sys.stdin:
        args = line.split("\t")
        print("got " + len(args) + " arguments")
        sys.stdout.flush()
```

The problem is that string serialization and deserialization is much slower than using binary to transfer data between clickhouse and python functions.

# The Solution

The solution is to use Clickhouse's `RowBinary`. According to [Clickhouse docs](https://clickhouse.com/docs/en/interfaces/formats/#rowbinary):

> Rows and values are listed consecutively, without separators.

## Deserialization

In my case, I wanted to read an array but:

> Array is represented as a varint length (unsigned LEB128), followed by successive elements of the array.

So the first thing is to read the first byte, which will tell us the length of the array (i.e., how many elements to read):

```python
s = io.BytesIO(sys.stdin.buffer.read())
```

Now we have to decode the Unsigned LEB128 byte. This can be done with the [`leb128`](https://github.com/mohanson/leb128) package (which may need to be installed):

```python
length = leb128.u.decode_reader(s)[0]
```

Now you can read each element of the array:

```python
for k in range(length):
    next_value_in_array = leb128.u.decode_reader(s)[0]
```

So now we can reconstruct the array sent into our python function.

## Serialization

The next trick is that we have to respond in RowBinary format as well (not just print strings to `stdout`).

If we only want to respond with an UInt64, for example, we can use `struct.pack` to encode an integer using `<` for *little-endian* (because RowBinary uses little endian) and `Q` for *unsigned long long*. This gives:

```python
sys.stdout.buffer.write(struct.pack('<Q', myIntegerResponse ))
```

# Post Script

For this to work, your `myFunc_function.xml` will look something like this:

```xml
<functions>
    <function>
        <type>executable</type>
        <name>myFunc</name>
        <return_type>UInt64</return_type>
        <argument><type>Array(UInt64)</type></argument>
        <format>RowBinary</format>
        <command>my_function.py</command>
    </function>
</functions>
```