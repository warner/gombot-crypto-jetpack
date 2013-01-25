This package provides encryption functions for the Gombot password manager experiment, for use in a Addon-SDK (jetpack) -style addon.

`require("gombot-crypto-jetpack").kdf(email, password)` returns a Promise that resolves to a "keys" object when the KDF (Key Derivation Function) finishes running. This takes aabout 2.5 of seconds on a new 2012-era laptop.

`require("gombot-crypto-jetpack").encrypt(keys, string)` returns a Promise that resolves to a base64-encoded string of encrypted+MACed ciphertext. This requires the "keys" object returned by the KDF function. It runs very quickly. It requires a string, so you should JSON-encode any data structures prior to calling it.

`require("gombot-crypto-jetpack").decrypt(keys, ciphertext)` returns a Promise that resolves to a string. It requires the "keys" object from the KDF function, and a base64-encoded string as returned by `encrypt`. It also runs very quickly. The Promise will be rejected if the ciphertext has been corrupted (i.e. the MAC does not validate) or if the internal version identifier is not recognized.

TODO: `require("gombot-crypto-jetpack").addEntropy(string, numBits)` can be called before invoking `encrypt()`, with a string of random data. This will be mixed into the SJCL entropy pool to provide a properly unpredictable IV. Internal randomness mixed with externally-provided entropy (perhaps from a trusted server) provides safety against failures in just one of the sources.
