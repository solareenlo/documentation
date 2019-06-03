# アドレス生成
<!-- # Create an address -->

**アドレスは複数回IOTAトークンを引き出してはいけません。アドレスからIOTAトークンを引き出す場合は、インデックスをインクリメントするか別のセキュリティレベルを使用して新しいトークンを作成する必要があります。**
<!-- **Addresses must not be withdrawn from more than once. If you withdraw IOTA tokens from an address, you must create a new one by incrementing the index or using a different security level.** -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには、次のものが必要です。
<!-- To complete this guide, you need the following: -->

* [Node.js (8+)](https://nodejs.org/en/)
* [Visual Studio Code](https://code.visualstudio.com/Download)のようなコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* インターネット接続
<!-- * An Internet connection -->

---

1. `iota-basics`という新しいディレクトリを作成します。
<!-- 1. Create a new directory called `iota-basics` -->

2. コマンドラインで、`iota-basics`ディレクトリに移動し、[IOTAコアライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/core)をインストールします。
<!-- 2. In the command line, change into the `iota-basics` directory, and install the [IOTA core library](https://github.com/iotaledger/iota.js/tree/next/packages/core) -->

  ```bash
  cd iota-basics
  npm install --save @iota/core
  ```

3. `iota-basics`ディレクトリに、`create-address.js`という名前の新しいファイルを作成します。
<!-- 3. In the `iota-basics` directory, create a new file called `create-address.js` -->

4. `create-address.js`ファイルで、IOTAライブラリが必要です。
<!-- 4. In the `create-address.js` file, require the IOTA libraries -->

  ```js
  const Iota = require('@iota/core');
  ```

5. IOTAオブジェクトのインスタンスを作成し、`provider`フィールドを使用してノードに接続します。
<!-- 5. Create an instance of the IOTA object and use the `provider` field to connect to a node -->

  ```js
  const iota = Iota.composeAPI({
    provider: 'https://nodes.devnet.iota.org:443'
  });
  ```

6. シードを格納するための変数を作成します。
<!-- 6. Create a variable to store a seed -->

  ```js
  const seed =
  'PUETTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';
  ```

  :::info:
  シードを使用するコードはすべてクライアント側で実行されます。シードがデバイスを離れることはありません。
  :::
  <!-- :::info: -->
  <!-- Any code that uses a seed is executed on the client side. Your seed never leaves your device. -->
  <!-- ::: -->

7. アドレスを作成するために、`seed`変数を`getNewAddress()`メソッドに渡します。
<!-- 7. Pass the `seed` variable to the `getNewAddress()` method to create an address -->

  ```js
  iota.getNewAddress(seed, {index: 0, security: 2})
    .then(address => console.log(address));
  ```

  ファイルを実行すると、アドレスが表示されます。スクリプトをもう一度実行すると、同じシード、インデックス、およびセキュリティレベルから派生しているため、同じアドレスが表示されます。
  <!-- When you execute the file, you should see an address. If you execute the script again, you'll see the same address because its derived from the same seed, index and security level. -->

`getNewAddress()`メソッドのインデックスとセキュリティレベルの引数を変更して、別のアドレスを作成してください。
<!-- Try changing the index and security level arguments in the `getNewAddress()` method to create a different address. -->

## コードを走らせる
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックしてください。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Create-an-address?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

:::warning:
インデックスまたはセキュリティレベルを指定せずに`getNewAddress()`メソッドを呼び出すと、ライブラリはまだIOTAトークンが取り出されていないアドレスを返します。

これを行うために、ライブラリは、そのアドレスを使用する入力トランザクションを見つけるようにノードに要求します。

一部のノードは台帳からのトランザクションを刈り取るため、インデックスやセキュリティレベルを省略することはお勧めしません。結果として、ライブライはアドレスが使われているかどうかを常に知るわけではありません。
:::
<!-- :::warning: -->
<!-- If you call the `getNewAddress()` method without the index or security level, the library will return an address from which you haven't yet withdrawn. -->
<!--  -->
<!-- To do this, the library asks the node to find input transactions that use the address. -->
<!--  -->
<!-- We don't recommend omitting the index or security level because some nodes prune transactions from their ledgers. As a result, the library won't always know if an address is spent. -->
<!-- ::: -->
