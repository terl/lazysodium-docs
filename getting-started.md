---
description: >-
  Before we begin, please ensure you have added Lazysodium as a dependency in
  your project. This is outlined in the
  [readme](https://github.com/terl/lazysodium-java#get-started).
---

# Getting started

Before we begin, please ensure you have added Lazysodium as a dependency in your project. This is outlined in [this page](installation.md). 

### Read the docs

**Remember, read the** [**Libsodium documentation**](https://download.libsodium.org/doc/) **to find out what each function outlined below does!**

### How Lazysodium names its functions

To understand how Lazysodium's naming works, here's a piece of code. Don't worry if you don't understand it just yet. In the next section, we will provide more information on what each step does.

```java
// This is the native C function for generating a key.
crypto_secretbox_keygen(key);

// And these are Lazysodium's functions.
// Let's initialise LazySodium
LazySodiumJava lazySodium = new LazySodiumJava(new SodiumJava());

// Now you can cast to an interface so that our
// IDE picks up and intelligently loads up the correct methods. 
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

As you can see, the C code has been camel-cased and encapsulated in a class that's relevant to the operation. For example `crypto_secretbox*` functions are in a class called `SecretBox` and `crypto_generichash*` functions are in a class called `GenericHash`.

### Usage

#### 1. Initialise Sodium

The first step is to create a `sodium` object. The `sodium` object should be initialised only once as it loads the native Libsodium library.

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
byte[] pw = "A cool password".getBytes(StandardCharsets.UTF_8);
byte[] outputHash = new byte[PwHash.STR_BYTES];
PwHash.Native pwHash = (PwHash.Native) lazySodium;
boolean success = pwHash.cryptoPwHashStr(
    outputHash,
    pw,
    pw.length,
    PwHash.PWHASH_ARGON2ID_OPSLIMIT_MIN,
    PwHash.PWHASH_ARGON2ID_MEMLIMIT_MIN
);

// ... or you can use the super-powered lazy functions.
// For example, this is equivalent to the above.
PwHash.Lazy pwHashLazy = (PwHash.Lazy) lazySodium;
String hash = pwHashLazy.cryptoPwHashStr(
    "A cool password", 
    PwHash.PWHASH_ARGON2ID_OPSLIMIT_MIN,
    PwHash.PWHASH_ARGON2ID_MEMLIMIT_MIN
);
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

## Extra code samples

View [this page](more-examples.md) for more examples. Also, please review the [test classes](https://github.com/terl/lazysodium-java/tree/master/src/test/java) for more code samples.

