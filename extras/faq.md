---
description: Any questions go here.
---

# FAQ

## Why should I use Lazysodium?

We get it. You've tried a million other cryptography libraries and you're tired of performing mental gymnastics just to get something, anything working. We've done the work for you. We've compiled and wrapped the native libraries inside this project so you don't have to. We've designed our library in such a way that just makes cryptography infinitely easier to work with and deploy in a matter of minutes.

Under the hood, we're using Libsodium, an excellent audited cryptography library that just works. We've implemented all the [features](../about-1/features.md) of Libsodium in Lazysodium. To find out more about why we created Lazysodium visit the [about](../about-1/about.md) page.

## Why is this project called Lazysodium?

We wanted developers to have an effortless experience using cryptography. Every cryptography library we came across for Java and Android was plagued with inconsistencies and outdatedness. We thought, "Someone should do something about this...". So we did.

## Why create both Lazysodium for Java and Lazysodium for Android? Aren't they the same?

Even though you can code in Java on Android, it does not mean that Android conforms to Java's semantics surrounding the build and packaging phases. For example, Java packages files as a `jar` and Android packages files as an `aar`. This is so that you can include `res` files in your Android projects. This is why we have both Lazysodium for Android and Lazysodium for Java even if the former is just a wrapper around the latter.

## Why are some functions unavailable on Android?

The reason why some functions are not included in Android, is because we have compiled Libsodium against "minimal mode" which means that more demanding functions won't run on low powered hardware. If you really want to use those functions recompile Libsodium in "full mode" and then [self provision it](self-provisioning-libsodium.md).

## What platforms does Lazysodium support?

Please see [this page](../about-1/supported-platforms.md) for more information.

## Does Lazysodium have example apps?

Yes! Please download it from the [Google Play Store](https://play.google.com/store/apps/details?id=com.goterl.lazycode.lazysodium.example).

## Lazysodium contains a bug

We're sorry to hear this. It goes against our ethos to make you do some work, but if you could create an issue on the issue tracker [https://github.com/terl/lazysodium-java/issues](https://github.com/terl/lazysodium-java/issues%29\), that'd be perfect.

## Every time I use Lazysodium I get an error like java.lang.UnsatisfiedLinkError: Unable to load library. What is this?

This can occur for a number of reasons. Most commonly the error occurs when we try to load `libsodium.so / libsodium.dll / libsodium.dylib` - Lazysodium can't find it. Or, the error may occur for a totally different reason: The native Libsodium library \(i.e. the `libsodium.so / libsodium.dll / libsodium.dylib` files\) requests access to C functions which your OS does not have and so it results in a UnsatsifiedLinkError. Confusing, huh? There's more info in [this](https://github.com/terl/lazysodium-java/issues/34) issue. 

## I get the `Failed resolution of: Lcom/sun/jna/Native` error, how do I resolve it?

This error occurs if you don't include JNA as a dependency in your project. Please add the following line in your project:

{% tabs %}
{% tab title="Android" %}
```groovy
dependencies {
    implementation 'net.java.dev.jna:jna:LATEST_VERSION@aar' // Add this line
}
```
{% endtab %}

{% tab title="Java" %}
```groovy
dependencies {
    implementation 'net.java.dev.jna:jna:LATEST_VERSION' // Add this line
}
```
{% endtab %}
{% endtabs %}

Where `LATEST_VERSION` is the latest version of [JNA](https://mvnrepository.com/artifact/net.java.dev.jna/jna).

## How do I verify a file through GPG?

Our Ed25519 GPG Public Key is:

```text
-----BEGIN PGP PUBLIC KEY BLOCK-----
Comment: GPGTools - https://gpgtools.org

mDMEWvHA7BYJKwYBBAHaRw8BAQdA6CKcyDyLS17UCb2YyBLbvHHUY6uQthJNvWg4
RWrvnrO0HVRlcmwgVGVjaCBMdGQgPGhlbGxvQHRlcmwuY28+iJAEExYKADgWIQRa
p4l2+w6d03iL4SWYW6aY4ebaXAUCWvHA7AIbAwULCQgHAwUVCgkICwUWAgMBAAIe
AQIXgAAKCRCYW6aY4ebaXCVZAQC7SfhlxI0lrfUNJz/m4obDpBw9sDLHDBO1bhJB
g/7fMgD/YtWEk0FTZAdCtjZA7WpOu3tI1WoGS4joMpAXvU0x8gu4OARa8cDsEgor
BgEEAZdVAQUBAQdAdCqE0OtY3OGM5huHDrMvkU5j5qKmY228mt5zm8RkkRcDAQgH
iHgEGBYKACAWIQRap4l2+w6d03iL4SWYW6aY4ebaXAUCWvHA7AIbDAAKCRCYW6aY
4ebaXN2bAPoCen+Xip4ZgU+QsIZN53qQ9X0D8FfHT1hy5aAka7n6DgD/U7UOS1HU
Z6loj4sW6qvvMXoIh9GqBiVrfbgmWmk1qwm4MwRa8cgpFgkrBgEEAdpHDwEBB0Ds
HzWlmxqMaZ/6JO5YPEXuR30lZ94wPVf1D/DB81sk0YjvBBgWCgAgFiEEWqeJdvsO
ndN4i+ElmFummOHm2lwFAlrxyCkCGyIAgQkQmFummOHm2lx2IAQZFgoAHRYhBNGp
I08SFBwoCLPmAV/L5CItLGbXBQJa8cgpAAoJEF/L5CItLGbXoFkBAKqayqV0syoL
B5DRvnYqIfnUAJjhqu5uRQMvrqOnRXWJAP49PBLfRlNOaZ4nS77Nwrj4yJ1wcdM4
FDgZPJRxySnNB4GPAQCWErB65qUTxDqWAyNNVs3IdxBcJpwFrfL4RArErranJQEA
s11r7q/66aI/OyTlbnjxsnMfqzHgwcgIRpOU3k9kgAY=
=3eeN
-----END PGP PUBLIC KEY BLOCK-----
```

You can verify that a file was from us by performing the command:

```bash
# The general format is gpg --verify signature signedFile, for example:
$ gpg --verify lazysodium-java-2.3.0.jar.asc lazysodium-java-2.3.0.jar
```

Which should output:

```text
gpg: Signature made Tue  5 Jun 01:33:36 2018 BST
gpg:                using EDDSA key D1A9234F12141C2808B3E6015FCBE4222D2C66D7
gpg: Good signature from "Terl Tech Ltd <hello@terl.co>" [ultimate]
```

## Do you have any apps to showcase Lazysodium?

As a matter of fact, we do! Visit [Google Play](https://play.google.com/store/apps/details?id=com.goterl.lazycode.lazysodium.example) to see some of what Lazysodium can do.

[![Download Lazysodium](../.gitbook/assets/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.goterl.lazycode.lazysodium.example)

