---
description: Let's get started with using Lazysodium for Java and Android.
---

# Getting started

Before we begin, please ensure you have added Lazysodium as a dependency in your project. This is outlined in [the Installation page](installation.md). We know it's a pain to read the docs \(and the length of this page may put some of you off\), but once you've read these sections, you'd be able to use super-safe and super-secure crypto easily.

## Part 1: Getting started

### Read the docs

**Remember, read the** [**Libsodium documentation**](https://libsodium.gitbook.io/doc/) **to find out what each function outlined below does!**

### How Lazysodium names its functions

To understand how Lazysodium's naming works, here's a piece of code. Don't worry if you don't understand it just yet, you just need to understand how we're naming our classes and functions. In the next section, we will provide more information on how this code works.

```java
// This is the native C function for generating a key.
crypto_secretbox_keygen(key);

// And these are Lazysodium's functions.
// Let's initialise LazySodium
LazySodiumJava lazySodium = new LazySodiumJava(new SodiumJava());

// Now you can cast to an interface so that your
// IDE picks up and intelligently loads suggestions. 
SecretBox.Native secretBoxNative = (SecretBox.Native) lazySodium;
SecretBox.Lazy secretBoxLazy = (SecretBox.Lazy) lazySodium;

// The first one is Lazysodium's Native implementation which
// is just like the above C function but with tiny enhancements
// to make your life easier.
secretBoxNative.cryptoSecretBoxKeygen(key);
// Convert key to string and save to DB


// This one is Lazysodium's Lazy implementation of the
// above C function. 
String key = secretBoxLazy.cryptoSecretBoxKeygen();
```

If you've familiarised yourself with the Libsodium docs, then you will be able to see a **pattern** emerging in the above code. As you can see, the C code from Libsodium has been camel-cased and encapsulated in a class that's relevant to the operation. For example `crypto_secretbox*` functions are in a class called `SecretBox` and `crypto_generichash*` functions are in a class called `GenericHash`. 

### Seriously, read the official docs

Again, it's **vitally** important that you've read the [`Libsodium documentation`](https://libsodium.gitbook.io/doc/) otherwise you will be lost and confused. We want you to read Libsodium's documentation because practically everything is already written there. Lazysodium does not want to rewrite all that documentation - all Lazysodium does is provide a nice Java interface to those official functions.

Moreover, Libsodium's documentation changes from time-to-time so it would make maintaining Lazysodium's documentation an impossible task. That is the reason why we have provided you this section to read... To help you figure out what classes you need yourself based on the Libsodium docs. 

### Function list

[Here](https://github.com/terl/lazysodium-java/tree/master/src/main/java/com/goterl/lazycode/lazysodium/interfaces) are all the functions you can use.

## Part 2: Usage

#### 1. Initialise Sodium

The first step to getting started with Lazysodium is to create a `sodium` object. The `sodium` object should be initialised only once as it loads the native C Libsodium library.

```java
// Place near the start of the program's execution.
// Use the following if in a Java program
SodiumJava sodium = new SodiumJava();

// Use the following if in a Android program
SodiumAndroid sodium = new SodiumAndroid();

// Or you can supply a path. The exact semantics
// are outlined in SodiumJava.java or SodiumAndroid.java
SodiumJava sodium = new SodiumJava("/absolute/path/to/libsodium");
SodiumAndroid sodium = new SodiumAndroid("/absolute/path/to/directory/of/android/ABIS");
```

The first 1 to 6 lines demonstrates auto-loading of the prepackaged _native_ C Libsodium library from Lazysodium's `resources` folder \(if you're on Java, that is\). In lines 7 to 11 the code demonstrates that you can provide a path to your own Libsodium file. This is the preferred choice if you are running on a operating system or platform which may not be mainstream and so have compiled Libsodium yourself. Or if you simply don't trust us.

**Note:** If you need some hints on how to self-provide your own Libsodium native library please see the [self-provisioning page](self-provisioning-libsodium.md).

For Linux, the native Libsodium library will be contained in a `libsodium.so` file. For Mac, it's called `libsodium.dylib`. For Windows, it's called `libsodium.dll`. For Android, it's `libsodium.so`, but it's **NOT** the same as the Linux `libsodium.so`, it's compiled differently.

We shall leave you to find out how best to detect your platform as it may vary depending on your use case.

#### 2. Initialise Lazysodium

After initialising a `sodium` object you should now pass that `sodium` object to a `LazySodium` instance. You can add a charset, which will make Lazysodium functions like `lazySodium.str(...)` use the correct charset.

```java
// Optionally add a default charset.
LazySodiumJava lazySodium = new LazySodiumJava(sodium, StandardCharsets.UTF_8);

// Or if using the Android variant
LazySodiumAndroid lazySodium = new LazySodiumAndroid(sodium, StandardCharsets.UTF_8);
```

#### 3. Native or Lazy

Now that you have a `lazySodium` object, you can start using Libsodium lazily or natively. The way we've developed Lazysodium is that you can cast the `lazySodium` object to the interfaces that you need at that time. It makes it much easier for you to code. Modern text editors and IDEs can auto-suggest the code that you are interested in. Here's an example of said casting to accomplish password hashing. If you don't understand what `cryptoPwHashStr` means then please [view the Libsodium documentation](https://download.libsodium.org/doc/password_hashing/the_argon2i_function.html) that provides excellent information on what it does.

```java
LazySodiumJava lazySodium = new LazySodiumJava(sodium);

// Now you can cast and use the enhanced native 
// Libsodium functions
byte[] pw = lazySodium.bytes("A cool password");
byte[] outputHash = new byte[PwHash.STR_BYTES];
PwHash.Native pwHash = (PwHash.Native) lazySodium;
boolean success = pwHash.cryptoPwHashStr(
    outputHash,
    pw,
    pw.length,
    PwHash.OPSLIMIT_MIN,
    PwHash.MEMLIMIT_MIN
);

// ... or you can use the super-powered lazy functions.
// For example, this is equivalent to the above.
PwHash.Lazy pwHashLazy = (PwHash.Lazy) lazySodium;
String hash = lazySodium.cryptoPwHashStr("a cool password", PwHash.OPSLIMIT_MIN, PwHash.MEMLIMIT_MIN);
```

#### Step 4: Raw functions \(Optional\)

In one of the first steps above, you created a `sodium` object. You can actually use that object to call raw native C Libsodium functions.

```java
SodiumJava sodium = new SodiumJava();
// Or if you're on Android: Sodium sodium = new SodiumAndroid();

// You can also get the sodium object like this...
SodiumJava sodium = lazySodium.getSodium();

// Use a raw password hash
int res = sodium.crypto_pwhash(outputHash,
                outputHashLen,
                password,
                passwordLen,
                salt,
                opsLimit,
                memLimit,
                alg);

if (res == 0) { 
    // Successfully hashed a password!
}
```

It's just another way Lazysodium provides you to do different things with potentially the same outcomes 😄. To view every `Native` function, please browse the [`Sodium`](https://github.com/terl/lazysodium-java/blob/master/src/main/java/com/goterl/lazycode/lazysodium/Sodium.java) class.

#### 5. Constants

All constants are also located in their relevant operational interfaces.

Let's say for example you were performing a key exchange. The first thing you do is generate a key. You opted to use this native method `KeyExchange.cryptoBoxKeypair(Key publicKey, Key secretKey)`. But that leaves you wondering how many bytes to use for `publicKey` and `secretKey`. The `KeyExchange` interface has these byte sizes available for you:

```java
public interface KeyExchange {
    int PUBLICKEYBYTES = 32;
    int SECRETKEYBYTES = 32;
    int SESSIONKEYBYTES = 32;
    int SEEDBYTES = 32;
    String PRIMITIVE = "x25519blake2b";

    // ... rest of interface
}
```

There is an obvious naming strategy to our constants. For example, `crypto_kx_PUBLICKEYBYTES` is available in Lazysodium as `KeyExchange.PUBLICKEYBYTES`. Similarly, `crypto_shorthash_KEYBYTES` is available as `ShortHash.KEYBYTES`.

## Part 3: Helpful hints

### The Native and Lazy divide

We'd like to take an opportunity to show you the difference between using the Native interface and using the Lazy interface.

Here are two equivalent functions in Lazysodium that do the same thing. They both create a secret key so that you can encrypt a message or file.

```java
public interface SecretBox {    
      // Function 1    
      interface Native {        
            void cryptoSecretBoxKeygen(byte[] key);    
      }        
      // Function 2    
      interface Lazy {        
            String cryptoSecretBoxKeygen();    
      }    
}
```

You'd use them like like this:

```java
// Create a LazySodium instance near the start
// of program execution.
LazySodiumJava ls = new LazySodiumJava(new SodiumJava());

// ...

// Then you can cast to the interface, if you want
// to help your IDE give you intelligent suggestions
SecretBox.Lazy secretBoxLazy = (SecretBox.Lazy) ls;
SecretBox.Native secretBoxNative = (SecretBox.Native) ls;

// NATIVE
byte[] secretBoxBytes = new byte[SecretBox.KEYBYTES];
secretBoxNative.cryptoSecretBoxKeygen(secretBoxBytes);
System.out.println(ls.str(secretBoxBytes));

// LAZY
Key key = secretBoxLazy.cryptoSecretBoxKeygen();
System.out.println(key.getAsHexString());

```

The `Native` and `Lazy`  functions are practically the same BUT the Lazy functions almost always return a `String` in **hexadecimal** format.

1. **Never** mix Native and Lazy functions, unless you know what you're doing.
2. Lazy functions, **in most cases**, take and return hexadecimal strings - be careful and read the code's documentation to be sure.

### Checker

In some interfaces, you have static checkers which can check things like key, mac, hash length for you, so you don't have to look around for the correct lengths.

```java
public interface SecretBox {    
    class Checker extends BaseChecker {        
        public static boolean checkKeyLen(int len) {            
            return KEYBYTES == len;        
        }        
        public static boolean checkMacLen(int len) {            
            return MACBYTES == len;        
        }
        public static boolean checkNonceLen(int len) {            
            return NONCEBYTES == len;        
        }    
    }        
    interface Native {        
        void cryptoSecretBoxKeygen(byte[] key);    
    }
    interface Lazy {        
        String cryptoSecretBoxKeygen();    
    }    
}

byte[] key = new byte[32];
boolean correctLength = SecretBox.Checker.checkKeyLen(key.length);
```

### Convenience methods

There are some convenience methods located in the `LazySodium` class that can help you even further. Here's a few:

```java
// Set a default charset
LazySodiumJava ls = new LazySodiumJava(sodium, charset);

// Remove then nulls off the end of 
// an array. Useful for cryptoPwHashStr
ls.removeNulls(byte[]);

// Converts a byte array to a string
// using the charset provided above.
ls.str(byte[] bs);

// Convert a String to a byte array
ls.bytes(String s);

// Properly convert a byte array to a hexadecimal
// string. Hex strings are great because they
// give us strings like A12D22E1524 instead of
// 29kjdkadaldkalkl-?ald
ls.sodiumBin2Hex(byte[] bs);
```

## Extra code samples

### Lazysodium Examples

The [Lazysodium Examples](https://github.com/terl/lazysodium-examples) project should provide you with a lot of sample code that will surely help you out. Look inside the appropriate folders for your use-case to view the readme's on how to run the example code.

### Tests

You can also review our [test classes](https://github.com/terl/lazysodium-java/tree/master/src/test/java) for more code samples.

### Lazysodium: The Android App

There is also a Lazysodium [open-source app](https://github.com/terl/lazysodium-examples/tree/master/android) available on [Google Play](https://play.google.com/store/apps/details?id=com.goterl.lazycode.lazysodium.example) that allows you to see some operations:

[![Download Lazysodium](.gitbook/assets/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.goterl.lazycode.lazysodium.example)

