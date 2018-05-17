---
description: View what has been added to Lazysodium.
---

# Features

### How to read each table

A checkmark in the `Native` column means that the particular feature has C native functions written in the APIs language. A checkmark in the `Lazy` column means that the particular feature has been smartly implemented so that the developer has an effortless experience. A checkmark in the `Tests` column means that testing has been performed on that feature.

### Android + Java

[Lazysodium for Android](https://github.com/terl/lazysodium-android) and [Lazysodium for Java](https://github.com/terl/lazysodium-java) both benefit from the same API through JNA. They both have been designed with [Direct Mapping](https://github.com/java-native-access/jna/blob/master/www/DirectMapping.md) for extra speed.

#### API

| Feature | Native | Lazy | Tests |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Auth - `crypto_auth*`  functions | ✔ | ✔ | ✔ |
| Advanced - all functions not implemented yet | ✘ | ✘ | ✘ |
| AEAD -`crypto_aead*` functions | ✔ | ✔ | ✘ |
| Box - `crypto_box*` functions | ✔ | ✔ | ✔ |
| Detached - some `detached` functions not implemented | ✘ | ✘ | ✘ |
| GenericHash - `crypto_generichash*` functions | ✔ | ✔ | ✔ |
| KeyDerivation - `crypto_kdf*` functions | ✔ | ✔ | ✔ |
| KeyExchange - `crypto_kx*` functions | ✔ | ✔ | ✔ |
| Padding - several padding functions | ✔ | ✔ | ✔ |
| PwHash - `crypto_pwhash*`functions | ✔ | ✔ | ✔ |
| Random - several random functions | ✔ | ✔ | ✔ |
| SecretBox - `crypto_secretbox*` functions | ✔ | ✔ | ✔ |
| SecretStream - `crypto_secretstream*` functions | ✔ | ✔ | ✔ |
| ShortHash - `crypto_shorthash*` functions | ✔ | ✔ | ✔ |
| Sign - `crypto_sign*` functions | ✔ | ✔ | ✔ |

