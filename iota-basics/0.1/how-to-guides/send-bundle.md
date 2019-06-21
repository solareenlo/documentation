# ゼロトークントランザクションのバンドルを送信する
<!-- # Send a bundle of zero-value transactions -->

**トランザクションは、ノードに送信される前にバンドルにまとめられる必要があります。 IOTAクライアントライブラリには、転送オブジェクトからバンドルを作成する組み込み関数があります。**
<!-- **Transactions must be packaged in a bundle before being sent to a node. The IOTA client libraries have built-in functions that create bundles from transfer objects.** -->

:::info:
バンドルまたはトランザクションという用語に慣れていない場合は、[バンドルとトランザクション](../concepts/bundles-and-transactions.md)をお読みください。
:::
<!-- :::info: -->
<!-- If you're unfamiliar with the terms bundle or transaction, we recommend that you [read about bundles and transactions](../concepts/bundles-and-transactions.md). -->
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

1. `iota-basics`という新しいディレクトリを作成します。
<!-- 1. Create a new directory called `iota-basics` -->

2. コマンドプロンプトで、`iota-basics`ディレクトリに移動し、[IOTAコアライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/core)と[IOTAコンバータライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/converter)をインストールします。
<!-- 2. In the command prompt, change into the `iota-basics` directory, and install the [IOTA core library](https://github.com/iotaledger/iota.js/tree/next/packages/core) and the [IOTA converter library](https://github.com/iotaledger/iota.js/tree/next/packages/converter) -->

  ```bash
  cd iota-basics
  npm install --save @iota/core
  ```

3. `iota-basics`ディレクトリに、`send-bundle.js`という新しいファイルを作成します。
<!-- 3. In the `iota-basics` directory, create a new file called `send-bundle.js` -->

4. `send-bundle.js`ファイルに、IOTAライブラリが必要です。
<!-- 4. In the `send-bundle.js` file, require the IOTA libraries -->

  ```js
  const Iota = require('@iota/core');
  const Converter = require('@iota/converter');
  ```

5. IOTAオブジェクトのインスタンスを作成し、`provider`フィールドを使用してIRIノードに接続します。
<!-- 5. Create an instance of the IOTA object and use the `provider` field to connect to an IRI node -->

  ```js
  const iota = Iota.composeAPI({
  provider: 'https://nodes.thetangle.org:443'
  });
  ```

6. トランザクションを送信する先の2つのアドレスとシードを格納する変数を作成します。
<!-- 6. Create the variables to store a seed and two addresses to which you want to send transactions -->

  ```js
  const seed =
  'PUETTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';

  var recipientAddress1 = "CXDUYK9XGHC9DTSPDMKGGGXAIARSRVAFGHJOCDDHWADLVBBOEHLICHTMGKVDOGRU9TBESJNHAXYPVJ9R9";

  var recipientAddress2 = "CYJV9DRIE9NCQJYLOYOJOGKQGOOELTWXVWUYGQSWCNODHJAHACADUAAHQ9ODUICCESOIVZABA9LTMM9RW";
  ```

  :::info:
  シードを使用するコードはすべてクライアント側で実行されます。シードが使用中のデバイスから離れることはありません。
  :::
  <!-- :::info: -->
  <!-- Any code that uses a seed is executed on the client side. Your seed never leaves your device. -->
  <!-- ::: -->

7. 送信するトランザクションごとに`転送`オブジェクトを1つ作成します。`アドレス`フィールドは、送信先のアドレスを含みます。
<!-- 7. Create one `transfer` object for each transaction that you want to send. The `address` field contains the address to which the transaction will be sent. -->

  ```js
  var transfer1 = {
  'address': recipientAddress1,
  'value': 0,
  'message': Converter.asciiToTrytes('Hello, this is my first message'),
  'tag': 'SENDABUNDLEOFTRANSACTIONS'
  };

  var transfer2 = {
  'address': recipientAddress2,
  'value': 0,
  'message': Converter.asciiToTrytes('Hello, this is my second message'),
  'tag': 'SENDABUNDLEOFTRANSACTIONS'
  };
  ```

  :::info:
  `asciiToTrytes()`メソッドは[基本的なASCII文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします。その結果、アクセントやウムラウトなどの発音区別符号やひらがなや漢字などの日本語（2バイト文字）はサポートされておらず、`INVALID_ASCII_CHARS`エラーが発生します.
  :::
  <!-- :::info: -->
  <!-- The `asciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
  <!-- ::: -->

8. `転送`配列を[`prepareTransfers`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers)メソッドに渡してバンドルを作成します。このメソッドは、転送オブジェクトからバンドルを作成します。次に、バンドルのトライトを`sendTrytes()`メソッドに渡して、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)、[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)を行い、バンドルを[ノード](root://getting-started/0.1/introduction/what-is-a-node.md)へ送信します。
<!-- 8. Pass the `transfers` array to the [`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers) method to construct a [bundle](root://getting-started/0.1/introduction/what-is-a-bundle.md). This method creates a bundle from the transfer object. Then, pass the bundle's trytes to the `sendTrytes()` method to do [tip selection](root://the-tangle/0.1/concepts/tip-selection.md), [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md), and send the bundle to the [node](root://getting-started/0.1/introduction/what-is-a-node.md). -->

  ```js
  iota.prepareTransfers(seed, [transfer1, transfer2])
  .then(function(trytes){
      return iota.sendTrytes(trytes, 3, 9);
  })

  .then(results => console.log(JSON.stringify(results, ['hash', 'currentIndex', 'lastIndex', 'bundle', 'trunkTransaction', 'branchTransaction'], 1)));
  ```

  この例では、結果の配列はJSONに変換され、トランザクションハッシュ、バンドル情報、および親トランザクションだけが出力に表示されるようにフィルタ処理されています。
  <!-- In this example, the resulting array is converted to JSON and filtered so that only the transaction hash, bundle information, and parent transactions are displayed in the output. -->

  :::info:
  トランザクションを再添付するためには、`prepareTransfers()`メソッドから返されたトライトを保存する必要があります。
  :::
  <!-- :::info: -->
  <!-- To be able to reattach a transaction, you should save the trytes that are returned from the `prepareTransfers()` method. -->
  <!-- ::: -->

  トランクトランザクションとブランチトランザクションは、親トランザクションと呼ばれます。
  <!-- Trunk and branch transactions are called parent transactions. -->

  [バンドル内のすべてのトランザクションは、それらの`trunkTransaction`フィールドの値によって接続されています](../references/structure-of-a-bundle.md)。トランザクション0の`trunkTransaction`ハッシュは、トランザクション1のトランザクションハッシュ（`hash`）と同じであることがわかります。
  <!-- [All transactions in a bundle are connected through the value of their `trunkTransaction` fields](../references/structure-of-a-bundle.md). You should see that the `trunkTransaction` hash of transaction 0 is the same as the transaction hash (`hash`) of transaction 1. -->


  ```json
  [
    {
      "hash": "9FVWBYVGTMDYPIYGMEHWQSZF9CDWHRADQNYIEJARTMXFSBTSAIFJPM9PNILLLBYIKSMIIDUOVSBWZ9999",
      "currentIndex": 0,
      "lastIndex": 1,
      "bundle": "TKLFNBRZCDUUOYBPHDIKKGSSVKLQINECAZHEKHJBPXZYXVXCDCLXZDQGUXTSZVWJVYABICHESIXXXLZU9",
      "trunkTransaction": "JIKDFORYEREMFYHLJHZGARNECTUUYYKSIVILDMEAPDYYXCVZHPVRJQDHKKWXMYGTUHBRBVYJXKTNA9999",
      "branchTransaction": "IRWPFAQQHSPRL9QBEQRSUSVEAAHQCNILEHJNUYZEQCQBFFLEV9FSGJQH9DZNKCHCOKGMAIXAUDBZZ9999"
    },
    {
      "hash": "JIKDFORYEREMFYHLJHZGARNECTUUYYKSIVILDMEAPDYYXCVZHPVRJQDHKKWXMYGTUHBRBVYJXKTNA9999",
      "currentIndex": 1,
      "lastIndex": 1,
      "bundle": "TKLFNBRZCDUUOYBPHDIKKGSSVKLQINECAZHEKHJBPXZYXVXCDCLXZDQGUXTSZVWJVYABICHESIXXXLZU9",
      "trunkTransaction": "IRWPFAQQHSPRL9QBEQRSUSVEAAHQCNILEHJNUYZEQCQBFFLEV9FSGJQH9DZNKCHCOKGMAIXAUDBZZ9999",
      "branchTransaction": "JPFAFQLMVHJYDWLPZUBKRWQIPYXUJUORQPYKBOLKRLAQKRDKVYWYZRQQEFSARZRPNZTGQANOIATT99999"
    }
  ]
  ```

9. 1番目のトランザクションの詳細を見るには、1番目のトランザクションのハッシュをコピーして[devnet.thetangle.org](https://devnet.thetangle.org/)に貼り付けます。これらの詳細は、Webサイトが接続しているノードから供給されています。
<!-- 9. To see details about your first transaction, copy the hash of the first transaction and paste it into [devnet.thetangle.org](https://devnet.thetangle.org/). These details have been sourced from the nodes that the website is connected to. -->

  ![Transaction in a Tangle explorer](../images/tangle-explorer.PNG)

10. 2番目のトランザクションの詳細を表示するには、`親トランザクション`までスクロールしてトランクハッシュをクリックします。
<!-- 10. To see details about your second transaction, scroll down to 'Parent transactions' and click the Trunk hash -->

  ![Trunk transaction in a Tangle explorer](../images/tangle-explorer-trunk.PNG)

## コードを走らせる
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックしてください。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Send-bundle?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
