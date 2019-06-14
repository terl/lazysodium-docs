# Installation

This page is devoted to showing you how to install Lazysodium into your project.

## Java

### Requirements

Lazysodium for Java requires:

* JDK 8 or higher.
* Gradle 4.7 or higher \(if compiling and building\).
* No effort whatsoever.

### Add the library

Substitute `VERSION_NUMBER` with the latest version available on [this page](https://bintray.com/terl/lazysodium-maven/lazysodium-java) \(located in the bottom right\). Substitute `LATEST_JNA_VERSION` for the [latest JNA](https://mvnrepository.com/artifact/net.java.dev.jna/jna) version.

{% tabs %}
{% tab title="Gradle" %}
```groovy
// Top level build file 
repositories {
    // You can either sync with our bintray repo
    // for faster updates
    maven {
        url  "https://dl.bintray.com/terl/lazysodium-maven"
    }
    
    // ... Or use jcenter (many people already have jcenter synced)
    jcenter()
}

// Add to dependencies section in app level build file
dependencies {
    implementation "com.goterl.lazycode:lazysodium-java:VERSION_NUMBER"
    implementation "net.java.dev.jna:jna:LATEST_JNA_VERSION"
}
```
{% endtab %}

{% tab title="Maven" %}
```markup
<--! Add the repo first. Can also use jcenter but updates are slower !-->
<repositories>
    <repository>
      <id>lazysodium-java</id>
      <url>https://dl.bintray.com/terl/lazysodium-maven</url>
    </repository>
 </repositories>

<--! Now add the dependencies !-->
<dependency>
  <groupId>com.goterl.lazycode</groupId>
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
If you're wanting to just add the JAR then grab it from the files section on the [Bintray page](https://bintray.com/terl/lazysodium-maven/lazysodium-java/_latestVersion). Then verify it using our GPG key which can be found on our [FAQ](../extras/faq.md#how-do-i-verify-a-file-through-gpg) page. You will also need the Java JNA jar file \(link above\).
{% endtab %}
{% endtabs %}

## Android

### Requirements

Lazysodium for Android requires:

* Android 19 or higher. Untested on lower versions.
* Gradle \(if compiling and building\).
* No effort whatsoever.

### Add the library

Substitute `VERSION_NUMBER` with the latest version available on [this page](https://bintray.com/terl/lazysodium-maven/lazysodium-android) \(located in the bottom right\). Substitute `LATEST_JNA_VERSION` for the [latest JNA](https://mvnrepository.com/artifact/net.java.dev.jna/jna) version, keep the `@aar` ending.

{% tabs %}
{% tab title="Gradle" %}
```groovy
// Top level build file
repositories {
    maven {
        url  "https://dl.bintray.com/terl/lazysodium-maven"
    }
}

// Add to dependencies section in app level build file
dependencies {
    implementation "com.goterl.lazycode:lazysodium-android:VERSION_NUMBER@aar"
    implementation "net.java.dev.jna:jna:LATEST_JNA_VERSION@aar"
}
```
{% endtab %}

{% tab title="AARs" %}
If you're wanting to just add the AAR then grab it from the files section on the [Bintray page](https://bintray.com/terl/lazysodium-maven/lazysodium-android/_latestVersion). Then optionally verify it using our GPG key which can be found on our [FAQ](../extras/faq.md#how-do-i-verify-a-file-through-gpg) page. You will also need to grab the AAR JNA file \(link above\).
{% endtab %}
{% endtabs %}

## Verification using GPG

If you want to verify an AAR or a JAR or any file, then [this question in the FAQ](../extras/faq.md#how-do-i-verify-a-file-through-gpg) on GPG usage will help.
