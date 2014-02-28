bip38
=====

A JavaScript component that adheres to the [BIP38](https://github.com/bitcoin/bips/blob/master/bip-0038.mediawiki) standard to secure your crypto currency private keys. Fully compliant with Node.js and the browser (via Browserify).


Why?
----

BIP38 is a standard process to encrypt crypto currency private keys that is imprevious to brute force attacks thus protecting the user.


Usage
-----

### Installation

    npm install --save bip38


### Example

```js
var Bip38 = require('bip38');

var bip38 = new Bip38();
bip38.generateAddress = function(privateKeyBytes) {
  /* code to generate public address */
  return {address: address, compressed: compressed};
}

var encryptedKey = bip38.encrypt(key, passphrase, 'buffer');
var wifKey = bip38.decrypt(encryptedKey, passphrase, 'wif');


bip38.encrypt({key: key, passhprase: passhphrase, keyType: 'wif', scryptParams: {N: 16384, r: 8, p: 8}});
```

API
---

### Bip38()

Constructor that creates a new `Bip38` instance.


### addressVersion

A field that accepts an object for the address version. This easily allows you to support altcoins. Defaults to Bitcoin values.


**example:**

```js
var Bip38 = require('bip38');

var privateKeyWif = '5KN7MzqK5wt2TP1fQCYyHBtDrXdJuXbUzm4A9rKAteGu3Qi5CVR';

var bip38 = new Bip38();
bip38.addressVersion = {private: 0x80, public: 0x0};
bip38.encrypt(privateKeyWif, "super-secret"});
```

### scryptParams

A field that accepts an object with the follow properties: `N`, `r`, and `p` to control the [scrypt](https://github.com/cryptocoinjs/scryptsy). The
BIP38 standard suggests `N = 16384`, `r = 8`, and `p = 8`. However, this may yield unacceptable performance on a mobile phone. If you alter these parameters, it wouldn't be wise to suggest to your users that your import/export encrypted keys are BIP38 compatible. If you do, you may want to alert them of your parameter changes.

**example:**

```js
bip38.scryptParams = {N: 8192, r: 8, p: 8};
```


### encrypt(wif, passphrase)

A method that encrypts the private key. `wif` is the string value of the wallet import format key. `passphrase` the passphrase to encrypt the key with. 


Returns the encrypted string.

**example**:

```js
var Bip38 = require('bip38');

var privateKeyWif = '5KN7MzqK5wt2TP1fQCYyHBtDrXdJuXbUzm4A9rKAteGu3Qi5CVR';

var bip38 = new Bip38();
var encrypted = bip38.encrypt(privateKeyWif, 'TestingOneTwoThree');
console.log(encrypted); // => 6PRVWUbkzzsbcVac2qwfssoUJAN1Xhrg6bNk8J7Nzm5H7kxEbn2Nh2ZoGg
```


### decrypt(encryptedKey, passhprase)

A method that decrypts the encrypted string. `encryptedKey` is the string value of the encrypted key. `passphrase` is the passphrase to decrypt the key with.


```js
var Bip38 = require('bip38');

var encryptedKey = '6PRVWUbkzzsbcVac2qwfssoUJAN1Xhrg6bNk8J7Nzm5H7kxEbn2Nh2ZoGg';

var bip38 = new Bip38();
var privateKeyWif = bip38.decrypt(encryptedKey, 'TestingOneTwoThree'});
console.log(privateKeyWif); // =>  '5KN7MzqK5wt2TP1fQCYyHBtDrXdJuXbUzm4A9rKAteGu3Qi5CVR'
```



References
----------
- https://github.com/bitcoin/bips/blob/master/bip-0038.mediawiki
- https://github.com/pointbiz/bitaddress.org/issues/56 (Safari 6.05 issue)
- https://github.com/casascius/Bitcoin-Address-Utility/tree/master/Model
- https://github.com/nomorecoin/python-bip38-testing/blob/master/bip38.py
- https://github.com/pointbiz/bitaddress.org/blob/master/src/ninja.key.js 