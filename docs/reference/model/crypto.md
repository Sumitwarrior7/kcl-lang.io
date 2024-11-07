---
title: "crypto"
linkTitle: "crypto"
type: "docs"
description: crypto system module
weight: 100
---

## md5

`md5(value: str, encoding: str = "utf-8") -> str`

Encrypt the string `value` using `MD5` and the codec registered for encoding.

```python
import crypto

md5 = crypto.md5("ABCDEF")
```

## sha1

`sha1(value: str, encoding: str = "utf-8") -> str`

Encrypt the string `value` using `SHA1` and the codec registered for encoding.

```python
import crypto

sha = crypto.sha1("ABCDEF")
```

## sha224

`sha224(value: str, encoding: str = "utf-8") -> str`

Encrypt the string `value` using `SHA224` and the codec registered for encoding.

```python
import crypto

sha = crypto.sha224("ABCDEF")
```

## sha256

`sha256(value: str, encoding: str = "utf-8") -> str`

Encrypt the string `value` using `SHA256` and the codec registered for encoding.

```python
import crypto

sha = crypto.sha256("ABCDEF")
```

## sha384

`sha384(value: str, encoding: str = "utf-8") -> str`

Encrypt the string `value` using `SHA384` and the codec registered for encoding.

```python
import crypto

sha = crypto.sha384("ABCDEF")
```

## sha512

`sha512(value: str, encoding: str = "utf-8") -> str`

Encrypt the string `value` using `SHA512` and the codec registered for encoding.

```python
import crypto

sha = crypto.sha512("ABCDEF")
```

## blake3

`blake3(value: str, encoding: str = "utf-8") -> str`

Encrypt the string `value` using `BLAKE3` and the codec registered for encoding.

```python
import crypto

blake3 = crypto.blake3("ABCDEF")
```

## uuid

`uuid() -> str`

Generate a random UUID string.

```python
import crypto

a = crypto.uuid()
```

## filesha256

`filesha256(filepath: str) -> str`

Calculate the SHA256 hash of the file `filepath`.

```python
import crypto

sha = crypto.filesha256("test.txt")
```
