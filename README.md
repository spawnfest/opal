# Opal

A PGP encryption and decryption library via NIF bindings in [sequoia](https://crates.io/crates/sequoia-openpgp)

## Usecase/Example

In order to create [secure financial systems]((https://www.precisely.com/blog/data-security/pci-compliance-standards-pci-dss)),
it is recommended data at rest is encrypted with AES, which is provided by the excellent
 [erlang crypto stdlib](https://www.erlang.org/doc/man/crypto.html#crypto_one_time_aead-6).

For data travelling over a network, PGP is preffered. This library tries to provide a simple api library for this.

For example from Circle's [documentation](https://developers.circle.com/docs/accept-card-payments-online):

```javascript
import { createMessage, encrypt, readKey } from 'openpgp'

// Object to be encrypted
interface CardDetails {
 number?: string,    // required when storing card details
 cvv?: string        // required when cardVerification is set to cvv
}

// Encrypted result
interface EncryptedValue {
 encryptedData: string,
 keyId: string
}
 
const pciEncryptionKey = await getPCIPublicKey()
 
/**
* Encrypt card data function
*/
return async function(dataToEncrypt: CardDetails): Promise<EncryptedValue> {
 const decodedPublicKey = await readKey({ armoredKey: atob(publicKey) })
  const message = await createMessage({ text: JSON.stringify(dataToEncrypt) })
  return encrypt({
    message,
    encryptionKeys: decodedPublicKey,
  }).then((ciphertext) => {
    return {
      encryptedMessage: btoa(ciphertext),
      keyId,
    }
  })
}
```

can now be achieved with a liveview.

TODO: test liveview example

```elixir

```

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed
by adding `opal` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:opal, "~> 0.1.0"}
  ]
end
```

Documentation can be generated with [ExDoc](https://github.com/elixir-lang/ex_doc)
and published on [HexDocs](https://hexdocs.pm). Once published, the docs can
be found at <https://hexdocs.pm/opal>.
