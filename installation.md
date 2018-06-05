---
description: >-
  You can install Lazysodium in various ways. Here's a short list of the ways in
  which you can install Lazysodium.
---

# Installation

This page is devoted to showing you how to install Lazysodium into your project. 

## Java

### Requirements

Lazysodium for Java requires:

* JDK 8 or higher.
* Gradle 4.7 or higher \(if compiling and building\).
* No effort whatsoever.

### Add the library

Substitute `VERSION_NUMBER` with the latest version available on [this page](https://bintray.com/terl/lazysodium-maven/lazysodium-java) \(located in the bottom right\).

{% tabs %}
{% tab title="Gradle" %}
```groovy
// Top level build file
repositories {
    maven {
        url  "https://dl.bintray.com/terl/lazysodium-maven"
    }
}

// Add to dependencies section
dependencies {
    implementation "com.goterl.lazycode:lazysodium-java:VERSION_NUMBER"
}
```
{% endtab %}

{% tab title="Maven" %}
```markup
// Add the repo first
<repositories>
    <repository>
      <id>lazysodium-java</id>
      <url>https://dl.bintray.com/terl/lazysodium-maven</url>
    </repository>
 </repositories>
 
<dependency>
  <groupId>com.goterl.lazycode</groupId>
  <artifactId>lazysodium-java</artifactId>
  <version>VERSION_NUMBER</version>
</dependency>
```
{% endtab %}

{% tab title="JARs" %}
If you're wanting to just add the JAR then grab it from the files section on the [Bintray page](https://bintray.com/terl/lazysodium-maven/lazysodium-java/_latestVersion). Then verify it using our GPG key.
{% endtab %}
{% endtabs %}

## Android

### Requirements

Lazysodium for Android requires:

* Android 19 or higher. Untested on lower versions.
* Gradle \(if compiling and building\).
* No effort whatsoever.

### Add the library

Substitute `VERSION_NUMBER` with the latest version available on [this page](https://bintray.com/terl/lazysodium-maven/lazysodium-java) \(located in the bottom right\).

{% tabs %}
{% tab title="Gradle" %}
```groovy
// Top level build file
repositories {
    maven {
        url  "https://dl.bintray.com/terl/lazysodium-maven"
    }
}

// Add to dependencies section
dependencies {
    implementation "com.goterl.lazycode:lazysodium-java:VERSION_NUMBER"
}
```
{% endtab %}

{% tab title="AARs" %}
If you're wanting to just add the AAR then grab it from the files section on the [Bintray page](https://bintray.com/terl/lazysodium-maven/lazysodium-android/_latestVersion). Then verify it using our GPG key.
{% endtab %}
{% endtabs %}

## Verification using GPG

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

