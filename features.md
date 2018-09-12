# Features

### General

* [x] Super easy install.
* [x] Completely open source.
* [x] Ability to use commercially under the terms of the [MPLv2](https://www.mozilla.org/en-US/MPL/2.0/FAQ/).
* [x] Great [collaborative](https://github.com/terl/lazysodium-docs) documentation.
* [x] Up-to-date with the awesome [Libsodium](https://github.com/jedisct1/libsodium) library \(version 1.0.16\).
* [x] Bundled and compiled native libraries so you don't have to.
* [x] Reactive and fast release cadence.
* [x] [Bring your own Libsodium](self-provisioning-libsodium.md) native libraries.
* [x] [Lazysodium for Android](https://github.com/terl/lazysodium-android) and [Lazysodium for Java](https://github.com/terl/lazysodium-java) both benefit from the same codebase.
* [x] They both have been designed with JNA [Direct Mapping](https://github.com/java-native-access/jna/blob/master/www/DirectMapping.md) for extra speed.
* [x] Architected in a composite oriented fashion to allow developers to narrow what operation they want to use at any time - less opportunity for bugs.
* [x] Architected in such a way that you can use the raw JNA wrapped native C functions whenever you want.
* [x] Built by a division of [Terl](https://terl.co) called Lazycode. We needed Lazysodium for our project [Recordo](https://recordo.co). This means Lazysodium is a long-term project.

### Operations list

A checkmark in the `Native` column means that the particular operation has C native functions written in the APIs language. A checkmark in the `Lazy` column means that the particular operation has been smartly implemented so that the developer has an effortless experience. A checkmark in the `Tests` column means that testing has been performed on that operation.

| **Operation** | **Native** | **Lazy** | **Tests** |
| :--- | :--- | :--- | :--- |
| Auth - `crypto_auth*`  functions | ✔ | ✔ | ✔ |
| AEAD -`crypto_aead*` functions | ✔ | ✔ | ✔ |
| Box - `crypto_box*` functions | ✔ | ✔ | ✔ |
| [Diffie-Hellman](https://download.libsodium.org/doc/advanced/scalar_multiplication.html) - `crypto_scalarmult*` functions | ✔ | ✔ | ✔ |
| Ed25519 to Curve25519 - `crypto_sign_ed25519` functions | ✔ | ✔ | ✔ |
| GenericHash - `crypto_generichash*` functions | ✔ | ✔ | ✔ |
| [HMAC-SHA-2](https://download.libsodium.org/doc/advanced/hmac-sha2.html) - `crypto_auth_hmacsha*` functions | ✔ | ✔ | ✔ |
| KeyDerivation - `crypto_kdf*` functions | ✔ | ✔ | ✔ |
| KeyExchange - `crypto_kx*` functions | ✔ | ✔ | ✔ |
| Padding - several padding functions | ✔ | ✔ | ✔ |
| PwHash - `crypto_pwhash*`functions | ✔ | ✔ | ✔ |
| Random - several randomisation functions | ✔ | ✔ | ✔ |
| Scrypt - `crypto_pwhash_scrypt*` functions $$^2$$  | ✔ | ✔ | ✔ |
| SecretBox - `crypto_secretbox*` functions | ✔ | ✔ | ✔ |
| SecretStream - `crypto_secretstream*` functions | ✔ | ✔ | ✔ |
| SHA256/SHA512 - `crypto_hash*` functions | ✔ | ✔ | ✔ |
| ShortHash - `crypto_shorthash*` functions | ✔ | ✔ | ✔ |
| Sign - `crypto_sign*` functions | ✔ | ✔ | ✔ |
| [Stream Ciphers](https://download.libsodium.org/doc/advanced/stream_ciphers.html) $$^1$$ - `crypto_stream*` functions | ✔ | ✔ | ✔ |
| SecureMemory - `sodium_m` functions | ✘ | ✔ | ✔ |

### Footnotes

1:  The Android variant does not have Salsa20 8 and 12 rounds and no XChaCha20.  
2: The Android variant does not have functions of the following form: `cryptoPwHashScryptSalsa208*`

Please see [this question in the FAQ](faq.md#why-are-some-functions-unavailable-on-android) for the reason as to why these functions are not available on Android.

