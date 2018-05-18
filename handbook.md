---
description: >-
  A more detailed look into using Lazysodium and common pitfalls to watch out
  for.
---

# Handbook

This page assumes you've read Getting started.

### The Native and Lazy split

We'd like to take an opportunity to show you the difference between using the Native interface and using the Lazy interface.

Here are two equivalent functions in Lazysodium that do the same thing. They both create a secret key so that you can encrypt a message or file.

```java

public interface SecretBox {    interface Native {        void cryptoSecretBoxKeygen(byte[] key);    }    interface Lazy {        String cryptoSecretBoxKeygen();    }    }
```

You'd use them like like this:

```java
// Create a LazySodium instance near the start
// of program execution.
LazySodium ls = new LazySodium(new SodiumJava());

// ...

// Then you can cast to the interface, if you want
// to help your IDE give you intelligent suggestions
SecretBox.Lazy secretBoxLazy = (SecretBox.Lazy) ls;
SecretBox.Native secretBoxNative = (SecretBox.Native) ls;

// NATIVE
byte[] secretBoxBytes = new byte[SecretBox.KEYBYTES];
secretBoxNative.cryptoSecretBoxKeygen(secretBoxBytes);
System.out.println(new String(secretBoxBytes, StandardCharsets.UTF_8));

// LAZY
String key = secretBoxLazy.cryptoSecretBoxKeygen();
System.out.println(key);

```

The Native and Lazy `cryptoSecretBoxKeygen`  functions do not have any difference in themselves... It's what you _**do**_ with what's _**returned**_ that makes the difference. 

In the case of the Native `cryptoSecretBoxKeygen` function, we create a new string from the bytes \(line 15\), whereas in the Lazy `cryptoSecretBoxKeygen` function, we return a `String` \(line 18\). 

When we converted the bytes to `String` \(line 15\), you'd expect it to equal line 18:

```java
// Does Native equal Lazy?
new String(secretBoxBytes, StandardCharsets.UTF_8) == secretBoxLazy.cryptoSecretBoxKeygen()
```

The answer is that they are **NOT EQUAL** as `secretBoxLazy.cryptoSecretBoxKeygen()` returns a **hexadecimal** string. If you do go down the route of `new String()` then you're going to be in a world of pain as that will leave null bytes and carriage returns in your String. You will therefore have a hard time storing your string and, even worse, converting back to bytes.

Therefore, if you want to convert a `byte[]` array to a `String`, then it's best to use either the `lazySodium.sodiumBinToHex(byte[])` method or the `LazySodium.toHex(byte[])` static method. 

#### Summary

1. **Never** mix Native and Lazy functions, unless you know what you're doing.
2. Lazy functions, **in most cases**, take and return hexadecimal strings - be careful and read the code's documentation to be sure.
3. **Always** convert your Lazy function byte arrays to strings using the `lazySodium.sodiumBinToHex(byte[])` method or the `LazySodium.toHex(byte[])` static method, otherwise you'd suffer painful consequences in the form of null bytes.

### Checker

In some interfaces, you have static checkers which can check things like key, mac, hash length for you, so you don't have to look around for the correct lengths.

```java
public interface SecretBox {    class Checker extends BaseChecker {        public static boolean checkKeyLen(int len) {            return KEYBYTES == len;        }        public static boolean checkMacLen(int len) {            return MACBYTES == len;        }        public static boolean checkNonceLen(int len) {            return NONCEBYTES == len;        }    }        interface Native {        void cryptoSecretBoxKeygen(byte[] key);    }        interface Lazy {        String cryptoSecretBoxKeygen();    }    }byte[] key = new byte[32];boolean correctLength = SecretBox.Checker.checkKeyLen(key.length);
```

### Convenience methods

There are some convenience methods located in the `LazySodium` class that can aid you on your way. Here's a few:

```text
// Set a default charset
LazySodium ls = new LazySodium(sodium, charset);

// Remove then nulls off the end of 
// an array. Useful for cryptoPwHashStr
ls.removeNulls(byte[]);

// Converts a byte array to a string
// using the charset provided above. Warning
// this will produce null bytes and unexpected
// carriage returns. Please use sodiumBin2Hex(byte[])
// to ensure no nulls or carriage breaks.
ls.str(byte[] bs);

// Convert a String to a byte array
ls.bytes(String s);

// Properly convert a byte array to a string
// without null bytes and carriage arrays.
// Be careful in providing the returned string
// into functions that expect hexadecimal strings
ls.sodiumBin2Hex(byte[] bs);
```

