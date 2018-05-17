---
description: 'In this section, we''ll see how you''d compile the native libsodium libraries.'
---

# Compiling Libsodium

Let's say you don't trust us to include a malware-free libsodium library. So the first thing you'd do is compile Libsodium yourself.

Let's say you're interested in building for Mac, Windows. Linux and Android. This means you need 4 files:

* [x] Mac: `libsodium.dylib`. `.dylib` means shared library for MacOS.
* [x] Windows: `libsodium.dll`. `.dll` means shared library for Windows.
* [x] Linux: `libsodium.so`. `.so` means shared library for Linux and Android.
* [x] Android: `libsodium.so`. `.so` means shared library for Linux and Android.

## Windows

Well, the Windows `libsodium.dll` is provided for you in the [mingw releases](https://download.libsodium.org/libsodium/releases/). Easy 👍

## Linux

Get the latest [stable release](https://download.libsodium.org/libsodium/releases/). Gain access to a Linux machine and compile it on there using:

```bash
$ ./configure
$ make && make check
$ sudo make install
```

Then it should appear somewhere installed under the directory `/usr/`.

## Mac

Get the latest [stable release](https://download.libsodium.org/libsodium/releases/). You can cross compile for Mac on Linux by running `./dist-build/osx.sh`. Or if you have a Mac and have [Homebrew](https://brew.sh/) then just run:

```bash
$ brew install libsodium
```

And take note of where it installs `libsodium.dylib`.

## Android

Get the latest [stable release](https://download.libsodium.org/libsodium/releases/). You can cross compile for Android on Linux by running `./dist-build/android-<abi>.sh`, where `<abi>` is the name of the ABI you want to generate a `libsodium.so` file for. \(It's recommended to build each one\).

```bash
$ ./dist-build/android-arm.sh
$ ./dist-build/android-armv7-a.sh
# etc...
```

