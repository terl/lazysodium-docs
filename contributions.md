---
description: What to do if you've found an issue or want to contribute code to Lazysodium.
---

# Contributions

##  Introduction

All contributions are appreciated and very welcome. There are many forms that contributions could take.

1. Issues and suggestions.
2. Code contributions.
3. Promoting Lazysodium.

##  Issues and suggestions

### Found an issue

Report these immediately on the issue tracker. If it's a very dangerous bug, then confidentiality would be preferred.

### Making a suggestion

Typically these take the form: "It would be great if Lazysodium implemented feature X." 

If you are suggesting a feature, please make sure it's within the scope of what Lazysodium actually does - which is to wrap Java functions around the native C functions provided by Libsodium.

## Code contributions

If you want to contribute code, you first have to understand how Lazysodium works.

### Lazysodium and JNA

Lazysodium uses [JNA with Direct Mapping](https://github.com/java-native-access/jna/blob/master/www/DirectMapping.md) to access native libraries. This means that you can write the native functions in Java without having to use more complex tools such as JNI and Swig.

### Read the docs

In order to complete the next section, you must have the Libsodium docs page open. You need to look at the parameters of the functions you are interested in. Read about the function then move on to the next section.

### Adding new functions

Let's say you've found a function in Libsodium that does not exist in Lazysodium and you really want it. As Lazysodium uses JNA, adding a new function is pretty simple.

Theoretically, let's assume that you really wanted the function `crypto_generichash` as it did not exist in the Lazysodium library. The first thing you'd do is add the function definition to the `Sodium.java` class. This class is the "circuitry" behind Lazysodium. 

So how would you convert the following into Java?

```java
int crypto_generichash(unsigned char *out, size_t outlen,
                           const unsigned char *in, unsigned long long inlen,
                           const unsigned char *key, size_t keylen);
```

The solution is pretty simple. The exact type mappings are explained over in the JNA repository in [full](https://github.com/java-native-access/jna/blob/master/www/Mappings.md). Here's what the above C function looks like in terms of JNA Java:

```java
public native int crypto_generichash(
    byte[] out, int outLen,
    byte[] in, long inLen,
    byte[] key, int keyLen
);
```

The reason why JNA is so cool is that it maps those C types for you. For example, `unsigned char *out` and `size_t outlen` maps to `byte[] out` and `int outLen`, respectively. There are so many examples already in [`Sodium.java`](https://github.com/terl/lazysodium-java/blob/master/src/main/java/com/goterl/lazycode/lazysodium/Sodium.java) that it would be pointless writing more examples here.

You may run across some `state` parameters in those native functions. These convert to simple classes in Java. For example the  `crypto_generichash_init` function asks for a state:

```java
// An example of a native function asking for state
public native int crypto_generichash_init(GenericHash.State state,
                                       byte[] key,
                                       int keyLength,
                                       int outLen);
```

Here's what the state struct looks like in C:

```java
typedef struct CRYPTO_ALIGN(64) crypto_generichash_state {
    uint64_t h[8];
    uint64_t t[2];
    uint64_t f[2];
    uint8_t  buf[2 * 128];
    size_t   buflen;
    uint8_t  last_node;
} crypto_generichash_state;
```

To convert that C code to JNA Java, you add the following class anywhere. It can also be named anything, however the fields inside it must be the same as the C variant.

```java
 // This is an inner class residing in GenericHash.java
 class State extends Structure {
        public static class ByReference extends GenericHash.State implements Structure.ByReference {

        }

        @Override
        protected List<String> getFieldOrder() {
            return Arrays.asList("h", "t", "f", "buf", "buflen", "last_node");
        }

        public long[] h = new long[8];
        public long[] t = new long[2];
        public long[] f = new long[2];
        public byte[] buf = new byte[2 * 128];
        public int buflen = 2 * 128;
        public int last_node;
}
```

To provide that state to functions, you first create it as follows:

```java
final GenericHash.State state = new GenericHash.State.ByReference();
```

### Adding Native and Lazy definitions

Now that you've converted the native `crypto_generichash` function and added it to the `Sodium.java` class, you can now think about adding `Native` and `Lazy` functions. But first, you need to create an interface in the interfaces package. If you're starting off with `crypto_generichash` then the interface should be called `GenericHash.java`. The `crypto_` part always comes off.

Then, create the `Native` and `Lazy` interfaces within that interface. 

```java
public interface GenericHash {
    interface Native {
    }
    interface Lazy {
    }
}
```

In the `Native` interface, you add a function that is almost an exact replica of the one you added to `Sodium.java`, but with minor enhancements like returning a boolean instead of an int. Plus, it's less confusing and less of an eye-sore for new developers to get started with Lazysodium when you give them properly camel-cased functions.

```java
public interface GenericHash {
    interface Native {
        boolean cryptoGenericHash(
                byte[] out, int outLen,
                byte[] in, long inLen,
                byte[] key, int keyLen
        );    
    }
    interface Lazy {
    }
}
```

After that, you add the `Lazy` variant. This variant should be purposefully built to make it super-easy for developers to use. Whatever enhancements you can think of you should add. The best way to think about it is this: If you were building an app what functions would YOU like to use? Would you like auto byte array to String conversion so that you can store the hash in an SQLite database? Would you like intelligent auto defaults on parameter variables so that you don't have to provide those variables yourself? Would you like the safety of an exception when hashing?

```java
public interface GenericHash {
    interface Native {
        boolean cryptoGenericHash(
                byte[] out, int outLen,
                byte[] in, long inLen,
                byte[] key, int keyLen
        );    
    }
    interface Lazy {
        String cryptoGenericHash(String in) throws SodiumException;
    }
}
```

### Adding implementations into LazySodium

After adding the `Native` and `Lazy` variants, you can now move onto the actual implementation, which you will add to `LazySodium.java`. First add the implementations to the top of the class:

```java
// Add to LazySodium because GenericHashing is
// compatible on both Android and Java.
// You would add the following line to
// LazySodiumJava if it was only compatible on
// Java, or LazySodiumAndroid if it was only
// compatible on Android.
public class LazySodium implements
        Base,
        Random,
        AEAD.Native, AEAD.Lazy,
        // Add this
        GenericHash.Native, GenericHash.Lazy
        ...
{
    // ... 
}
```

In the above block of code we're adding it to `LazySodium` because the functions inside `GenericHash` are compatible with both Java and Android. If `GenericHash` was only compatible with Android, you would add the line above and the implementation to `LazySodiumAndroid` instead. If GenericHash was only compatible with Java, then you would add the line above and the implementation to `LazySodiumJava`.

Then provide an implementation to the functions:

```java
public class LazySodiumJava implements
        Base,
        Random,
        AEAD.Native, AEAD.Lazy,
        GenericHash.Native, GenericHash.Lazy
        ...
{
    
    //// -------------------------------------------|
    //// GENERIC HASH
    //// -------------------------------------------|

    @Override
    public boolean cryptoGenericHash(byte[] out, int outLen, byte[] in, long inLen, byte[] key, int keyLen) {
        return boolify(nacl.crypto_generichash(out, outLen, in, inLen, key, keyLen));
    }
    
    @Override
    public String cryptoGenericHash(String in) throws SodiumException {
        byte[] message = bytes(in);
        byte[] hash = randomBytesBuf(GenericHash.BYTES);
        boolean res = cryptoGenericHash(hash, hash.length, message, message.length, null, 0);

        if (!res) {
            throw new SodiumException("Error could not hash the message.");
        }

        return toHex(hash);
    }
}
```

### Adding constants

From the implementations above, you can see a constant has been defined: `GenericHash.BYTES`.

Constants are very much a mandatory feature of Lazysodium. If you submit a Pull Request with an implementation but with no constants \(or, in other words, simply hard-coded values\) then it will most likely be rejected. Those constants are vital in making Lazysodium easy to use for other software engineers.

Please add constants wherever you can. Constants are added to every major operation. The `GenericHash.java` interface has [various constants](https://github.com/terl/lazysodium-java/blob/c2a297b8cb28e521de0ddcd9e5270dc799455bf0/src/main/java/com/goterl/lazycode/lazysodium/interfaces/GenericHash.java#L22) that serve as examples.

### Maintaining the Checker interface

The problem with low-level libraries such as Libsodium, is that their function definitions often require byte arrays with a required size. Lazysodium wants to make this easier by checking that the size of the byte array you defined was, in fact, the correct size.

The `Checker` interface inside whatever operation you are working in should be updated with static methods to help developers check their arrays and other sizes. The inner `Checker` class should ALWAYS extend `BaseChecker`.

The `Checker` class inside [`KeyDerivation.java`](https://github.com/terl/lazysodium-java/blob/c2a297b8cb28e521de0ddcd9e5270dc799455bf0/src/main/java/com/goterl/lazycode/lazysodium/interfaces/KeyDerivation.java#L90) serves as an example.

## Promoting Lazysodium

Another method of contributing to Lazysodium is by promoting it. This inspires confidence and has the beautiful side effect of more people becoming involved in privacy and security which is the over-arching goal of Lazysodium.

You can promote via blog posts, in a news article, or comments on sites like HackerNews or Reddit. Or even word of mouth. Or anything really.

[Terl](https://terl.co), the default copyright owners of Lazysodium, does not stipulate a method of promotion. A link back to the [repo](https://github.com/terl/lazysodium-java) or [Terl](https://terl.co) would be nice though!

