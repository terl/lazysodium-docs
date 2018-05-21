---
description: >-
  If you want to use your own Libsodium native library rather than ours, that's
  fine! On this page, we'll help you do that.
---

# Self-provisioning Libsodium

## Compiling your own Libsodium

Let's say you're interested in building for Mac, Windows. Linux and Android. This means you need 4 files:

* [x] Mac: `libsodium.dylib`. `.dylib` means shared library for MacOS.
* [x] Windows: `libsodium.dll`. `.dll` means shared library for Windows.
* [x] Linux: `libsodium.so`. `.so` means shared library for Linux and Android.
* [x] Android: `libsodium.so`. `.so` means shared library for Linux and Android.

### Windows

Well, the Windows `libsodium.dll` is provided for you in the [mingw releases](https://download.libsodium.org/libsodium/releases/). Easy 👍

### Linux

Get the latest [stable release](https://download.libsodium.org/libsodium/releases/). Gain access to a Linux machine and compile it on there using:

```bash
$ ./configure
$ make && make check
$ sudo make install
```

Then it should appear somewhere installed under the directory `/usr/`.

### Mac

Get the latest [stable release](https://download.libsodium.org/libsodium/releases/). You can cross compile for Mac on Linux by running `./dist-build/osx.sh`. Or if you have a Mac and have [Homebrew](https://brew.sh/) then just run:

```bash
$ brew install libsodium
```

And take note of where it installs `libsodium.dylib`.

### Android

Get the latest [stable release](https://download.libsodium.org/libsodium/releases/). You can cross compile for Android on Linux by running `./dist-build/android-<abi>.sh`, where `<abi>` is the name of the ABI you want to generate a `libsodium.so` file for. \(It's recommended to build each one\).

```bash
$ ./dist-build/android-arm.sh
$ ./dist-build/android-armv7-a.sh
# etc...
```

## Libsodium native library location

After [compiling](self-provisioning-libsodium.md) Libsodium you have to make sure that the resulting files `libsodium.so/libsodium.dylib/libsodium.dll (i.e the native libraries)` are accessible by your Java or Android program. There are a myriad of ways you can make the native Libsodium files accessible:

1.  On program start-up, copy them to a temporary location.
2. Set `java.library.path`
3. Make your build tool \(such as Gradle\) look for them in specific folders.

### Java

Lazysodium for Java has actually placed the native library files within the `resources` folder and as soon as `SodiumJava()` is created, we load the relevant native library depending on the platform we're on:

```java
private String getLibSodiumFromResources() {    String path = getPath("windows", "libsodium.dll");    if (Platform.isLinux() || Platform.isAndroid()) {        path = getPath("linux", "libsodium.so");    } else if (Platform.isMac()) {        path = getPath("mac", "libsodium.dylib");    }    return path;}
```

**Warning**: If you intend on JAR'ing your compiled native library then a different procedure must be used. See below in the Quirks section.

### Android

If you want to use your compiled native library files within Android instead of Lazysodium's, then create the folder:

```text
src/main/jniLibs
```

Then, add the following to the `android` block in your app level build.gradle file:

```groovy

android {    sourceSets.main {        jni.srcDirs = []        jniLibs.srcDirs = ['src/main/jniLibs']    }}
```

Then in your `jniLibs` folder make sure you have all the Android ABIs listed as folders in there. Within each ABI folder, you need the `libsodium.so` files that was compiled against that ABIs \(see above\). Once you've done that, rename those `libsodium.so` files to something like `libmysodium.so.` \(The renaming overrides the loading of Lazysodium's `libsodium.so` files.\)

Then initialise a Lazysodium object as follows:

```java
final LazySodium lazySodium = new LazySodium(new SodiumAndroid("mysodium"));
```

This will load your compiled Libsodium native files rather than Lazysodium's pre-included compiled native files.

## Quirks

When you've compiled your own Libsodium you have to watch out for certain things.

### JARs

If you bundle native libraries inside a JAR \(which is what Lazysodium for Java does\) then you have to copy the native libs to a temporary location first:

```java
NativeUtils.loadLibraryFromJar();
```

This is because finding a path to the native libraries inside a JAR is difficult. So we move the native library files to a temp location.

