---
description: >-
  This page is devoted to showing you how to install Lazysodium into your
  project.
---

# Installation

## Java

Lazysodium for Java requires:

* JDK 8 or higher.
* Gradle 4.7 or higher \(if compiling and building\).
* No effort whatsoever.

Substitute `VERSION_NUMBER` with the latest version available on [this page](https://search.maven.org/search?q=a:lazysodium-java) \(located in the bottom right\). Substitute `LATEST_JNA_VERSION` for the [latest JNA](https://mvnrepository.com/artifact/net.java.dev.jna/jna) version.

{% tabs %}
{% tab title="Gradle" %}
```groovy
// Top level build file
repositories {
    // Add this to the end of any existing repositories
    mavenCentral() 
}

// Project level dependencies section
dependencies {
    implementation "com.goterl:lazysodium-java:VERSION_NUMBER"
    implementation "net.java.dev.jna:jna:JNA_NUMBER"
}
```
{% endtab %}

{% tab title="Maven" %}
```markup
<--! Now add the dependencies !-->
<dependency>
  <groupId>com.goterl</groupId>
  <artifactId>lazysodium-java</artifactId>
  <version>VERSION_NUMBER</version>
</dependency>

<dependency>
  <groupId>net.java.dev.jna</groupId>
  <artifactId>jna</artifactId>
  <version>LATEST_JNA_VERSION</version>
</dependency>
```
{% endtab %}

{% tab title="JARs" %}
If you're wanting a JAR instead then grab it from the Download button on the right hand side of the [Maven Sonatype search page](https://search.maven.org/search?q=a:lazysodium-java). Then verify it using our GPG key which can be found on our [FAQ](../extras/faq.md#how-do-i-verify-a-file-through-gpg) page. You will also need the Java JNA jar file \(link above\).
{% endtab %}
{% endtabs %}

## Android

Lazysodium for Android requires:

* Android 19 or higher. Untested on lower versions.
* Gradle \(if compiling and building\).
* No effort whatsoever.

Substitute `VERSION_NUMBER` with the latest version available on [this page](https://bintray.com/terl/lazysodium-maven/lazysodium-android) \(located in the bottom right\). Substitute `LATEST_JNA_VERSION` for the [latest JNA](https://mvnrepository.com/artifact/net.java.dev.jna/jna) version, keep the `@aar` ending.

{% tabs %}
{% tab title="Gradle" %}
```groovy
// Top level build file
repositories {
    // Add this to the end of any existing repositories
    mavenCentral() 
}

// Project level dependencies section
dependencies {
    implementation "com.goterl:lazysodium-android:VERSION_NUMBER@aar"
    implementation "net.java.dev.jna:jna:JNA_NUMBER@aar"
}
```
{% endtab %}

{% tab title="AARs" %}
If you're wanting to just add the AAR then grab it from the Download button on the right hand side of the [Maven Sonatype search page](https://search.maven.org/search?q=a:lazysodium-android). Then optionally verify it using our GPG key which can be found on our [FAQ](../extras/faq.md#how-do-i-verify-a-file-through-gpg) page. You will also need to grab the AAR JNA file \(link above\).
{% endtab %}
{% endtabs %}

### Proguard

If you are using Proguard then it is advisable to use the following rules for JNA. This is taken from the [JNA's wiki page](https://github.com/java-native-access/jna/blob/master/www/FrequentlyAskedQuestions.md#jna-on-android).

```text
-dontwarn java.awt.*
-keep class com.sun.jna.* { *; }
-keepclassmembers class * extends com.sun.jna.* { public *; }
```

## Verification using GPG

If you want to verify an AAR or a JAR or any file, then [this question in the FAQ](../extras/faq.md#how-do-i-verify-a-file-through-gpg) on GPG usage will help.

