# node-dukpt

## Derived Unique Key Per Transaction (DUKPT) Encryption with NodeJS

Criptografia de chave exclusiva derivada por transação (DUKPT) com NodeJS

Esta é a implementação NodeJS de DUKPT com base na implementação vanilla javascript da criptografia / descriptografia ** IDTech ** DUKPT. Este módulo fornece criptografia Dukpt usando esquemas 3DES ou AES.

> Observe que a criptografia / descriptografia AES atualmente é compatível apenas com as versões 6.x.x e superiores do NodeJS devido a algumas limitações que serão abordadas em breve em uma próxima versão.

### Instalação

```
npm install dukpt --save
```
### Usando DUKPT

Inicialize o DUKPT fornecendo BDK e KSN:

```
const Dukpt = require('dukpt');

const encryptionBDK = '0123456789ABCDEFFEDCBA9876543210';
const ksn = 'FFFF9876543210E00008';
const keyMode = 'datakey'; // optional: defaults to 'datakey'
const plainTextCardData = '%B5452310551227189^DOE/JOHN      ^08043210000000725000000?';

const dukpt = new Dukpt(encryptionBDK, ksn);
```
Após a inicialização, você pode usar `dukptEncrypt` e `dukptDecrypt` métodos para 
criptografar/descriptografar dados usando DUKPT.

#### Encrypting `ascii` data

Using 3DES,

```
const options = {
	inputEncoding: 'ascii', 
	outputEncoding: 'hex',
	encryptionMode: '3DES'
};
const encryptedCardData3Des = dukpt.dukptEncrypt(plainTextCardData, options);
```
or with AES,

```
const options = {
	inputEncoding: 'ascii', 
	outputEncoding: 'hex',
	encryptionMode: 'AES'
};
const encryptedCardDataAes = dukpt.dukptEncrypt(plainTextCardData, options);
```

#### Encrypting `hex` data

Using 3DES,

```
const options = {
	inputEncoding: 'hex',
	outputEncoding: 'hex',
	encryptionMode: '3DES'
};
const encryptedCardData3Des = dukpt.dukptEncrypt(plainTextCardData, options);
```
or using AES,

```
const options = {
	inputEncoding: 'hex',
	outputEncoding: 'hex',
	encryptionMode: 'AES'
};
const encryptedCardDataAes = dukpt.dukptEncrypt(plainTextCardData, options);
```

#### Decrypting data with `ascii` output encoding

```
const options = {
	outputEncoding: 'ascii',
	decryptionMode: '3DES',
	trimOutput: true
};

const decryptedCardData = dukpt.dukptDecrypt(encryptedCardData, options);
```
#### Decrypting data with `hex` output encoding

```
const options = {
	outputEncoding: 'hex',
	decryptionMode: '3DES',
	trimOutput: true
};

const decryptedCardData = dukpt.dukptDecrypt(encryptedCardData, options);
```

## API

### constructor Dukpt(bdk, ksn, [keyMode])

#### bdk

Base derivation key (BDK) for initialization

#### ksn

Key serial number (KSN) for initialization

> _See [here](https://en.wikipedia.org/wiki/Derived_unique_key_per_transaction) for more information on BDK and KSN_

#### keyMode
default: 'datakey'

Key mode for deriving session key from initial pin encryption key (IPEK). Possible values are:

- `datakey` (default)
- `pinkey`
- `mackey`

#### Dukpt.prototype.dukptEncrypt(plainTextCardData, options) and Dukpt.prototype.dukptDecrypt(encryptedCardData, options)

#### options

You can use options object to provide additional options for the DUKPT encryption/decryption. This object is **optional** and, if you don't provide it, encryption/decryption will use the default values shipped with it. 

Following listed are the available options.

Option | Possible Values | Default Value | Description
------------ | ------- | ------------- | --------------
`outputEncoding` | `ascii`, `hex` | For encryption `hex`, for decryption `ascii` | Specify output encoding of encryption/decryption
`inputEncoding` | `ascii`, `hex` | For encryption `ascii`, for decryption `hex` | Specify encoding of the input data for encryption/decryption
`trimOutput` (for decryption only) | `true`, `false` | `false` | Specify whether to strip out null characters from the decrypted output
`encryptionMode` (for encryption only) | `3DES`/`AES` | `3DES`/`AES` | Specify encryption scheme for dukpt
`decryptionMode` (for decryption only) | `3DES`/`AES` | `3DES`/`AES` | Specify decryption scheme for dukpt

### Tests
Tests can be run using gulp as follows:

```
gulp test
```

#### Roadmap

- [x] Support for DUKPT Encryption/Decryption with 3DES
- [x] Support for DUKPT Encryption/Decryption with AES (Node v6.x.x and above only)


