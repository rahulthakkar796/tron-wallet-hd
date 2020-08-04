# tron-wallet-hd
##### Tron HD wallet to generate offline private keys, mnemonic seeds and addresses.
### Installation

```shell
npm install tron-wallet-hd
```

### Usage

```js
const hdWallet = require('tron-wallet-hd');
```

### Methods

**generateMnemonic() :  Generates a string consisting of a random 12-word seed and returns it.**

```js
const seed = hdWallet.generateMnemonic();
```

**validateMnemonic(mnemonic) :  Checks if seed is a valid 12-word seed according to the <a href="https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki" traget="_blank">BIP39</a> specification and returns a boolean value.**

```js
const seed = "hello"
const isValidSeed = hdWallet.validateMnemonic(seed);
console.log(isValidSeed) //false
```


**generateAccountsWithMnemonic(mnemonic, index) :
 Generates public and private key-value pair using the given mnemonic seed and index. it's an asynchronous method.**

  * mnemonic: mnemonic seed to generate addresses.
 * index (optional):Number of accounts to generate, Default value is 5.


```js
const seed = hdWallet.generateMnemonic();
const accounts = await hdWallet.generateAccountsWithMnemonic(seed,2);
```

**getAccountAtIndex(mnemonic,index) : Returns public and private key-value pair stored at the specified index using the given mnemonic seed,  it's an asynchronous method.**
* mnemonic: mnemonic seed to get the key-value pair.
* index: index value at which key-value pair is stored. Default value is 0.

```js
const seed = "your mnemonic seed;
const account = await hdWallet.getAccountAtIndex(seed,1);
```

**validatePrivateKey(privateKey) : Validates the given private key and returns a boolean value.**

```js
const pk= "your private key";
const isValidPK = hdWallet.validatePrivateKey(pk);
```
**getAccountFromPrivateKey(privateKey) : Validates the given private key and returns tron address associated with the given private key. it's an asynchronous method.**

```js
const pk= "your private key";
const address = await getAccountFromPrivateKey(pk);
```

**validateAddress(address) : Validates the given tron address and returns a boolean value. Supports both base58 and hex format.**
```js
const isValidAddress1 = hdWallet.validateAddress("41c7cfb121ffac3fbf8c4dd48e17b055cae2ed1314");
console.log(isValidAddress1) //true

const isValidAddress2 = hdWallet.validateAddress("TRquwhRa9Eiva1ri1D6CmtxuNv9R6PPXGD");
console.log(isValidAddress2) //true

const isValidAddress3 = hdWallet.validateAddress("TRquwhRa9Eioabbabbbbqqwwwznaqwrtzxzbzbbz");
console.log(isValidAddress3) //false
```









