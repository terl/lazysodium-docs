---
description: >-
  If you want to provide your own compiled Libsodium library and inject it into
  Lazysodium, then this is the page for you.
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

But if you really want to compile it, you'll have to download extra dependencies using `sudo apt-get` or whatever your package manager is. Then run the Windows build scripts in the `build` folder.

```bash
$ ./configure
$ cd build
$ ./msys2-win64
```

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

After [compiling](self-provisioning-libsodium.md) Libsodium you have to make sure that the resulting files `libsodium.so/libsodium.dylib/libsodium.dll` \(i.e the native libraries\) are accessible by your Java or Android program. There are mainly 3 methods that you can use to make your freshly compiled native Libsodium files accessible:

1.  On program start-up, copy them to a temporary location, then load them. This is actually the method used in Lazysodium for Java when calling `new SodiumJava()`.
2. Set `java.library.path`. This is a well recognised environment variable in Java programs suitable for pointing to the location of your native files. This environment variable may not be available on Android.
3. Make your build tool \(such as Gradle or Maven\) look for them in specific folders.

### Java

**Point 1**. Lazysodium for Java has actually placed the native library files within the `resources` folder and as soon as `SodiumJava()` is created, we load the relevant native library depending on the platform we're on. This process is mentioned in point 1 in the list above.

```java
// Loading different native libraries depending on 
// the platform
private String getLibSodiumFromResources() {    
    String path = getPath("windows", "libsodium.dll");    
    if (Platform.isLinux() || Platform.isAndroid()) {        
        path = getPath("linux", "libsodium.so");    
    } else if (Platform.isMac()) {        
        path = getPath("mac", "libsodium.dylib");    
    }    
    return path;
}
```

Here's what the resources folder looks like:

```text
src/main/resources
├── mac/
│   └── libmysodium.so
├── armeabi/
│   └── libmysodium.so
└── armeabi-v7a/
│   └── libmysodium.so
```

**Point 2** is quite simple but there are many ways of doing it such as setting environment variables in your `.bashrc` or `export`ing them. Seek and you shall find.

**Point 3** is difficult if you are JAR'ing your application, but relatively simple otherwise. \(See the Quirks section\). Lazysodium dropped this method of loading the its native library files in favour of loading them via copying \(**point 1**\) as it was more reliable and did not cause exceptions. We were loading via relative paths but it failed within JARs.

### Android

We found that **point 3** \(see above for the points\) worked well for Android. If you want to use your compiled native library files within Android instead of Lazysodium's, then create the folder:

```text
src/main/jniLibs
```

Then, add the following to the `android` block in your app level build.gradle file:

```groovy

android {    
    sourceSets.main {        
        jni.srcDirs = []        
        jniLibs.srcDirs = ['src/main/jniLibs']    
    }
}
```

Then in your `jniLibs` folder make sure you have all the Android ABIs listed as folders in there. Within each ABI folder, you need the `libsodium.so` files that was compiled against that ABIs \(see above\). Once you've done that, rename those `libsodium.so` files to something like `libmysodium.so`

Then initialise a Lazysodium object as follows:

```java
final LazySodium lazySodium = new LazySodium(new SodiumAndroid("mysodium"));
```

This will load your renamed native library files rather than Lazysodium's pre-included compiled native files. The renaming overrides the loading of Lazysodium's `libsodium.so` files. After this, you can now use `LazySodium` as usual.

At the end of everything you should have a directory structure as follows:

```text
app/src/main/jniLibs/
├── arm64-v8a/
│   └── libmysodium.so
├── armeabi/
│   └── libmysodium.so
└── armeabi-v7a/
│   └── libmysodium.so
└── x86/
│   └── libmysodium.so
└── x86_64/
    └── libmysodium.so
```

Congratulations, you now have compiled, included and loaded your own Libsodium native library.

## Quirks

When you've compiled your own Libsodium you have to watch out for certain things.

### JARs

If you want to bundle native libraries inside a JAR then you have to copy the native libs to a temporary location first \(this is the method outlined in point 1 in the bulleted list in the previous section\). We have performed this action via a utility class:

```java
NativeUtils.loadLibraryFromJar();
```

This is because finding a path to the native libraries inside a JAR is difficult. So we move the native library files to a temp location and then load them from there. We found that loading the libraries this way is the most reliable. See the Java block in the previous section.

