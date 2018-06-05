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

If you want to verify an AAR or a JAR or any file, then [this question in the FAQ](faq.md#how-do-i-verify-a-file-through-gpg) on GPG usage will help you out.

