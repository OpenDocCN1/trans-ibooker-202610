```
def hash160(s):
    '''sha256 followed by ripemd160'''
    return hashlib.new('ripemd160', hashlib.sha256(s).digest()).digest()
```

Note that hashlib.sha256(s).digest does the sha256 and the wrapper around it does the ripemd160.

We can also update S256Point with hash160 and address methods:

```
class S256Point:
    ...
    def hash160(self, compressed=True):
        return hash160(self.sec(compressed))
    def address(self, compressed=True, testnet=False):
        '''Returns the address string'''
        h160 = self.hash160(compressed)
        if testnet:
            prefix = b'\x6f'
        else:
            prefix = b'\x00'
        return encode_base58_checksum(prefix + h160)
```

# Exercise 5

Find the addresses corresponding to the public keys whose private key secrets are:

+   - 5002 (use uncompressed SEC on testnet)
+   - 2020⁵ (use compressed SEC on testnet)
+   - 0x12345deadbeef (use compressed SEC on mainnet)

# WIF Format

The private key in our case is a 256-bit number. Generally, we are not going to need to serialize our secret that often, as it doesn’t get broadcast (that would be a bad idea!). That said, there are instances where you may want to transfer your private key from one wallet to another—for example, from a paper wallet to a software wallet.

For this purpose, you can use Wallet Import Format (WIF). WIF is a serialization of the private key that’s meant to be human-readable. WIF uses the same Base58 encoding that addresses use.

Here is how the WIF format is created:

+   1. For mainnet private keys, start with the prefix 0x80, for testnet 0xef.
+   2. Encode the secret in 32-byte big-endian.