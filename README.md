# tron-wallet-hd
##### Tron HD wallet to generate offline private keys, mnemonic seeds and addresses.
### Installation

```
npm install tron-wallet-hd
```
Sample recommended usage:

```js
// the seed is stored encrypted by a user-defined password
var password = prompt('Enter password for encryption', 'password');

keyStore.createVault({
  password: password,
  // seedPhrase: seedPhrase, // Optionally provide a 12-word seed phrase
  // salt: fixture.salt,     // Optionally provide a salt.
                             // A unique salt will be generated otherwise.
  
}, function (err, ks) {

  // Some methods will require providing the `pwDerivedKey`,
  // Allowing you to only decrypt private keys on an as-needed basis.
  // You can generate that value with this convenient method:
  ks.keyFromPassword(password, function (err, pwDerivedKey) {
    if (err) throw err;

    // generate five new address/private key pairs
    // the corresponding private keys are also encrypted
    ks.generateNewAddress(pwDerivedKey, 5);
    var addr = ks.getAddresses();

    ks.passwordProvider = function (callback) {
      var pw = prompt("Please enter password", "Password");
      callback(null, pw);
    };
  });
});

```

## `keystore` Function definitions

These are the interface functions for the keystore object. The keystore object holds a 12-word seed according to [BIP39][] spec. From this seed you can generate addresses and private keys, and use the private keys to sign transactions.

### Usage

```js
const hdWallet = require('tron-wallet-hd');
const keyStore=hdWallet.keyStore;
```

### Methods
### `keystore.createVault(options, callback)`

This is the interface to create a new lightwallet keystore.

#### Options

* password: (mandatory) A string used to encrypt the vault when serialized.
* seedPhrase: (mandatory) A twelve-word mnemonic used to generate all accounts.
* salt: (optional) The user may supply the salt used to encrypt & decrypt the vault, otherwise a random salt will be generated.
### `keystore.keyFromPassword(password, callback)`

This instance method uses any internally-configured salt to return the appropriate `pwDerivedKey`.

Takes the user's password as input and generates a symmetric key of type `Uint8Array` that is used to encrypt/decrypt the keystore.

### `keystore.isDerivedKeyCorrect(pwDerivedKey)`

Returns `true` if the derived key can decrypt the seed, and returns `false` otherwise.

### `keystore.generateRandomSeed([extraEntropy])`

Generates a string consisting of a random 12-word seed and returns it. If the optional argument string `extraEntropy` is present the random data from the Javascript RNG will be concatenated with `extraEntropy` and then hashed to produce the final seed. The string `extraEntropy` can be something like entropy from mouse movements or keyboard presses, or a string representing dice throws.

### `keystore.isSeedValid(seed)`

Checks if `seed` is a valid 12-word seed according to the [BIP39][] specification.

### `keystore.generateNewAddress(pwDerivedKey, [num])`

Allows the vault to generate additional internal address/private key pairs.

The simplest usage is `ks.generateNewAddress(pwDerivedKey)`.

Generates `num` new address/private key pairs (defaults to 1) in the keystore from the seed phrase, which will be returned with calls to `ks.getAddresses()`.

### `keystore.deserialize(serialized_keystore)`

Takes a serialized keystore string `serialized_keystore` and returns a new keystore object.

### `keystore.serialize()`

Serializes the current keystore object into a JSON-encoded string and returns that string.

### `keystore.getAddresses()`

Returns a list of hex-string addresses currently stored in the keystore.

### `keystore.getSeed(pwDerivedKey)`

Given the pwDerivedKey, decrypts and returns the users 12-word seed.

### `keystore.exportPrivateKey(address, pwDerivedKey)`

Given the derived key, decrypts and returns the private key corresponding to `address`.

<br>
<hr>

## `utils` Function definitions
### Usage

```js
const hdWallet = require('tron-wallet-hd');
const utils=hdWallet.utils;
```

**generateMnemonic() :  Generates a string consisting of a random 12-word seed and returns it.**

```js
const seed = utils.generateMnemonic();
```

**validateMnemonic(mnemonic) :  Checks if seed is a valid 12-word seed according to the <a href="https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki" traget="_blank">BIP39</a> specification and returns a boolean value.**

```js
const seed = "hello"
const isValidSeed = utils.validateMnemonic(seed);
console.log(isValidSeed) //false
```


**generateAccountsWithMnemonic(mnemonic, index) :
 Generates public and private key-value pair using the given mnemonic seed and index. it's an asynchronous method.**

  * mnemonic: mnemonic seed to generate addresses.
 * index (optional):Number of accounts to generate, Default value is 5.


```js
const seed = utils.generateMnemonic();
const accounts = await utils.generateAccountsWithMnemonic(seed,2);
```

**getAccountAtIndex(mnemonic,index) : Returns public and private key-value pair stored at the specified index using the given mnemonic seed,  it's an asynchronous method.**
* mnemonic: mnemonic seed to get the key-value pair.
* index: index value at which key-value pair is stored. Default value is 0.

```js
const seed = "your mnemonic seed";
const account = await utils.getAccountAtIndex(seed,1);
```

**validatePrivateKey(privateKey) : Validates the given private key and returns a boolean value.**

```js
const pk= "your private key";
const isValidPK = utils.validatePrivateKey(pk);
```
**getAccountFromPrivateKey(privateKey) : Validates the given private key and returns tron address associated with the given private key. it's an asynchronous method.**

```js
const pk= "your private key";
const address = await utils.getAccountFromPrivateKey(pk);
```

**validateAddress(address) : Validates the given tron address and returns a boolean value. Supports both base58 and hex format.**
```js
const isValidAddress1 = utils.validateAddress("41c7cfb121ffac3fbf8c4dd48e17b055cae2ed1314");
console.log(isValidAddress1) //true

const isValidAddress2 = utils.validateAddress("TRquwhRa9Eiva1ri1D6CmtxuNv9R6PPXGD");
console.log(isValidAddress2) //true

const isValidAddress3 = utils.validateAddress("TRquwhRa9Eioabbabbbbqqwwwznaqwrtzxzbzbbz");
console.log(isValidAddress3) //false
```









