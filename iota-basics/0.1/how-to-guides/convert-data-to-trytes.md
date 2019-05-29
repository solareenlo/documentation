# データをトライトに変換する
<!-- # Convert data to trytes -->

**トランザクションフィールドの値は、トライトで表現する必要があります。データ変換を容易にするために、IOTAクライアントライブラリには、トライト、トリット、およびASCII文字との間でデータを変換するための組み込みコンバータがあります.**
<!-- **The values of transaction fields must be represented in trytes. To facilitate data conversion, the IOTA client libraries have built-in converters to convert data to/from trytes, trits, and ASCII characters.** -->

:::info:
もしトライト、トリット、3進法という用語に慣れていないのであれば、[これらのコンセプトの記事](../concepts/trinary.md)を読むことをお勧めします。
:::
<!-- :::info: -->
<!-- If you're unfamiliar with the terms trytes, trits, or trinary, we recommend that you [read about these concepts](../concepts/trinary.md). -->
<!-- ::: -->

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

1. `iota-basics`というディレクトリを作成します.
<!-- 1. Create a new directory called `iota-basics` -->

2. コマンドプロンプトで、`iota-basics`ディレクトリに移動し、[IOTAコンバータライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/converter)をインストールします。
<!-- 2. In the command prompt, change into the `iota-basics` directory, and install the [IOTA converter library](https://github.com/iotaledger/iota.js/tree/next/packages/converter) -->

    ```bash
    cd iota-basics
    npm install --save @iota/converter
    ```

3. `iota-basics`ディレクトリに`convert-to-trytes.js`という名前の新しいファイルを作成します。
<!-- 3. In the `iota-basics` directory, create a new file called `convert-to-trytes.js` -->

4. `convert-to-trytes.js`ファイルには、IOTAクライアントライブラリが必要です。
<!-- 4. In the `convert-to-trytes.js` file, require the IOTA client library -->

    ```js
    const Iota = require('@iota/converter');
    ```

5. メッセージを保持するための変数を作成します.
<!-- 5. Create a variable to hold a message -->

    ```js
    var data = "Hello World!";
    ```

6. `data`変数を`asciiToTrytes()`メソッドに渡して、メッセージをトライトに変換します。
<!-- 6. Pass the `data` variable to the `asciiToTrytes()` method to convert the message to trytes -->

    ```js
    var trytes = Converter.asciiToTrytes(data);

    console.log(`${data} converted to trytes: ${trytes}`);
    ```

7. 返されたトライトを`trytesToAscii()`メソッドに渡して、ASCII文字に変換します。
<!-- 7. Pass the returned trytes to the `trytesToAscii()` method to convert them to ASCII characters -->

    ```js
    var message = Converter.trytesToAscii(trytes);

    console.log(`${trytes} converted back to ASCII: ${message}`);
    ```

    ファイルを実行すると、変換された文字列が標準出力に表示されます。
    <!-- When you execute the file, you should see the converted strings in the output: -->

    ```console
    Hello World! converted to trytes: RBTC9D9DCDEAFCCDFD9DSCFA
    RBTC9D9DCDEAFCCDFD9DSCFA converted back to ASCII: Hello World!
    ```

:::info:
`asciiToTrytes()`メソッドは[基本的なASCII文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします。その結果、アクセントやウムラウトなどの発音区別符号やひらがなや漢字などの日本語（2バイト文字）はサポートされておらず、`INVALID_ASCII_CHARS`エラーが発生します。
:::
<!-- :::info: -->
<!-- The `asciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
<!-- ::: -->

## コードを走らせる
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックしてください。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Convert-data-to-trytes?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
