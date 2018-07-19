---
description: Lazysodium is a cryptographic library. Find out how to use it on this page.
---

# About

## Why Lazysodium?

The problem with current cryptography libraries is that they can be one of 4 things: difficult to use, poorly documented, out of date or poorly architected.

So we built Lazysodium to make it **absolutely effortless** to get started with cryptography. We picked [Libsodium](https://github.com/jedisct1/libsodium) `1.0.16` as our underlying C library. Libsodium is an immensely powerful, well-tested and well-audited cryptography library that we found was perfect for securing all types of things.

### A small example

Compare the two ways you could use Lazysodium:

**1. Using native functions**

The following shows how to create a subkey from a master key. The subkey can then be used to encrypt and decrypt text.

```java
byte[] subkey = subkey[32];
byte[] context = "Examples".getBytes(StandardCharsets.UTF_8);
byte[] masterKey = "a_master_key_of_length_32_bytes!".getBytes(StandardCharsets.UTF_8);
int result = lazySodium.cryptoKdfDeriveFromKey(subkey, subkey.length, 1L, context, masterKey);

// Now check the result
if (res == 0) {
    // We have a positive result. Let's store it in a database.
    String subkeyString = new String(subkey, StandardCharsets.UTF_8);
}
```

**2. Or use Lazysodium's lazy functions**

You could use the above native functions **or** you could use the "Lazy" functions 😄

```java
String context = "Examples";
String masterKeyStr = "a_master_key_of_length_32_bytes!";
Key masterKey = Key.fromPlainString(masterKeyStr);
Key subKey = lazySodium.cryptoKdfDeriveFromKey(KeyDerivation.BYTES_MIN, 1L, context, masterKey);
System.out.println(subKey.getAsHexString());
// 20808D40E92E968AC65F4A95A015116C
```

As you can see Lazysodium makes cryptography effortless!

## Supported languages

Currently Lazysodium only supports [Android](https://github.com/terl/lazysodium-android) and [Java](https://github.com/terl/lazysodium-java). You can also drop Lazysodium in Kotlin projects via [Lazysodium for Java](https://github.com/terl/lazysodium-java).

## Features

Please view the [Features](features.md) page for more details.

## Questions

Please view the [FAQ](faq.md) page for more info.

## Our GPG Key

See [this](faq.md#how-do-i-verify-a-file-through-gpg) question in the FAQ.

## Become a supporter

Lazysodium needs money to survive. We've setup the following accounts for you to easily keep Lazysodium and all our other projects going. Your money would primarily be used to fund our open-source ventures. Please consider supporting us through these accounts.

[![](https://filedn.com/lssh2fV92SE8dRT5CWJvvSy/patron_button.png)](https://www.patreon.com/terlacious)   
 [![](https://filedn.com/lssh2fV92SE8dRT5CWJvvSy/liberapay.png)](https://liberapay.com/terlacious/)

