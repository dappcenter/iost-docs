---
id: Smart-Contract-Development-Toolkit
title: Scaf: 素晴らしいスマートコントラクト開発ツールキット
sidebar_label: Scaf: 素晴らしいスマートコントラクト開発ツールキット
---

## 機能

Scaffoldは、開発者のために、IOSTブロックチェーン用のJavaScriptによるスマートコントラクトを書くのを補助します。主な機能は次のとおりです。

- 必要な構造を持ったDappプロジェクトの初期化
- 容易なコントラクトの初期化とコントラクトへの関数の追加とテストの追加
- コントラクトのテストのためのシステム関数(ブロックチェーン関数やストレージ関数、も含んでいます)の適切な組み込み
- コントラクトをコンパイルし、コントラクトの妥当性をチェックし、ブロックチェーンに直接アップロードできるABIファイルの作成
- コントラクトのテストケースの実行

## インストールとセットアップ

始める前に、nodeとnpmがインストールされていることを確認してください。

1. `git clone git@github.com:iost-official/dapp.git`

2. `cd dapp`

3. `sudo npm install`

4. `sudo npm link`

## コマンド

scafコマンドを入力すると`ヘルプ`が表示されます。

```console
usr@Tower [master]:~/nodecode/dapp$ scaf
Usage: scaf <cmd> [args]

Commands:
  scaf new <name>      create a new DApp in current directory
  scaf add <item>      add a new [contract|function]
  scaf compile <name>  compile contract
  scaf test <name>     test contract

Options:
  --version   Show version number                                      [boolean]
  -h, --help  Show help                                                [boolean]

Not enough non-option arguments: got 0, need at least 1
usr@Tower [master]:~/nodecode/dapp$ scaf add
Usage: scaf add <item> [args]

Commands:
  scaf add contract <name>                  create a smart contract class
  scaf add func <con_name> <func_name>      add a function to a contract class
  [param...]
  scaf add test <con_name> <test_name>      add a test for a contract class

Options:
  --version           Show version number                              [boolean]
  -h, --help, --help  Show help                                        [boolean]

Not enough non-option arguments: got 0, need at least 1
```

## Hello BlockChain
### 新規プロジェクトの作成

```
scaf new <contract_name>
```

プロジェクトがカレントディレクトリに作成され、構造が初期化されます。

```console
usr@Tower [master]:~/nodecode/dapp$ scaf new helloBlockChain
make directory: helloBlockChain
make directory: helloBlockChain/contract
make directory: helloBlockChain/abi
make directory: helloBlockChain/test
make directory: helloBlockChain/libjs

usr@Tower [master]:~/nodecode/dapp$ ls helloBlockChain/
abi  contract  libjs  test
```

### コントラクトの追加

```
cd <contract_name>
scaf add contract <contract_name>
```

`add <item>` コマンドは、プロジェクトのディレクトリで実行してください。コントラクトファイル`helloContract.js`とABIファイル`helloContract.json`が次のように初期化された状態で生成されます。

```js
usr@Tower [master]:~/nodecode/dapp$ cd helloBlockChain/

usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ scaf add contract helloContract
create file: ./contract/helloContract.js
create file: ./abi/helloContract.json

usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ cat contract/helloContract.js
const rstorage = require('../libjs/storage.js');
const rBlockChain = require('../libjs/blockchain.js');
const storage = new rstorage();
const BlockChain = new rBlockChain();

class helloContract
{
    constructor() {
    }
    init() {
    }
};
module.exports = helloContract;

usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ cat abi/helloContract.json
{
    "lang": "javascript",
    "version": "1.0.0",
    "abi": [
    ]
}
```

### 関数の追加

```
scaf add func <contract_name> <function_name> [type0] [parameter0] [type1] [parameter1] ...
```

上のコマンドで、`hello`関数が`helloContract`クラスに追加されます。`string p0`は、`hello`関数が`string`型のパラメータを１つだけ持ち、 名前が`p0`であることを示しています。

パラメータの型は、['string', 'number', 'bool', 'json']のいずれかでなくてはなりません。

`hello(p0)`関数と関連するABI情報が`helloContract.js`と`helloContract.json`に追加されます。

```console
usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ scaf add func helloContract hello string p0
add function hello() to ./contract/helloContract.js
add abi hello to ./abi/helloContract.json
{
    "name": "hello",
    "args": [
        "string"
    ]
}
```

では、`contract/helloContract.js`を編集して、`hello(p0) {}`関数を実装します。

```js
hello(p0) {
  console.log(BlockChain.transfer("a", "b", 100));
  console.log(BlockChain.blockInfo());
  console.log("hello ", p0);
}
```

`hello(p0)`関数は、`BlockChain.transfer()`と`BlockChain.blockInfo()`の２つのシステム関数の結果がコンソールに表示します。

システム関数はモックなので、常に同じ有効な結果を返します。

### テストの追加

```
scaf add test <contract_name> <test_name>
```

このコマンドは、test1という名前のテストを`helloContract`コントラクトに追加します。１つの`require`ステートメントだけを持つ`helloContract_test1.js`が`test/`内に作成されます。

```console
usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ scaf add test helloContract test1
create file: ./test/helloContract_test1.js

usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ cat test/helloContract_test1.js
var helloContract = require('../contract/helloContract.js');
```
Now edit test/helloContract_test1.js
```js
usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ cat test/helloContract_test1.js
var helloContract = require('../contract/helloContract.js');

var ins0 = new helloContract();
ins0.hello("iost");
```

### テストの実行

```
scaf test <contract_name>
```

This command will run all the tests of specific contract.

```console
usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ scaf test helloContract
test ./test/helloContract_test1.js
transfer  a b 100
0
{"parent_hash":"0x00","number":10,"witness":"IOSTwitness","time":1537000000}
hello  iost
```

### コントラクトのコンパイル

```
scaf compile <contract_name>
```

このコマンドは、コントラクトファイルをコンパイルし、`build/`内にABIを作成します。このファイルは、IOSTブロックチェーンにアップロードできます。

```console
usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ scaf compile helloContract
compile ./contract/helloContract.js and ./abi/helloContract.json
compile ./abi/helloContract.json successfully
generate file ./build/helloContract.js successfully

usr@Tower [master]:~/nodecode/dapp/helloBlockChain$ find build/
build/
build/helloContract.js
build/helloContract.json
```
