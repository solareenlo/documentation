# 最初のゼロトークントランザクションを送信する（Node.js）
<!-- # Send your first zero-value transaction (Node.js) -->

**IOTAトークンを含まないランダムシードを使用して、ゼロトークントランザクションを送信できます。これらのトランザクションは、不変なメッセージをタングルに送信して保存したいアプリケーションに役立ちます。**
<!-- **A zero-value [transaction](../introduction/what-is-a-transaction.md) can be sent using a random seed that doesn't contain IOTA tokens. These transactions are useful for applications that want to send and store immutable messages on the Tangle.** -->

トランザクションを送信するには、以下を行います。
<!-- To send a transaction, you must do the following: -->

1. ノードに繋ぐ。
<!-- 1. Connect to a node -->
2. バンドルを作成し、バンドルをノードに送信する。
<!-- 2. Create a bundle and send it to the node -->

## 前提条件
<!-- ## Prerequisites -->

このチュートリアルを完了するには、次のものが必要です。
<!-- To complete this tutorial, you need the following: -->

* [Node.js (8+)](https://nodejs.org/en/)
* [Visual Studio Code](https://code.visualstudio.com/Download)のようなコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* インターネット接続
<!-- * An Internet connection -->

## ノードに接続する
<!-- ## Connect to a node -->

IOTAでは、トランザクションは[ノード](../introduction/what-is-a-node.md)に送信される必要があります。
<!-- In IOTA, transactions must be sent to [nodes](../introduction/what-is-a-node.md). -->

[IOTAネットワーク](../references/iota-networks.md)上のノードにバンドルを接続して送信することを選択できます。この例では、Devnetノードに接続します。
<!-- You can choose to connect and send bundles to a node on any [IOTA network](../references/iota-networks.md). In this example, we connect to a Devnet node. -->

1. コマンドプロンプトで、`iota-example`という作業ディレクトリを作成します。
<!-- 1. In the command prompt, create a working directory called `iota-example` -->

  ```bash
  mkdir iota-example
  ```

2. `iota-example`ディレクトリに移動し、IOTAクライアントライブラリをインストールします。
<!-- 2. Change into the `iota-example` directory and install the IOTA client libraries -->

  ```bash
  cd iota-example
  npm install @iota/core @iota/converter --save
  ```

  すべてうまくいけば、標準出力に次のようなものが表示されるはずです。 'npm WARN'メッセージは無視してかまいません。
  <!-- If everything went well, you should see something like the following in the output. You can ignore any 'npm WARN' messages. -->

  ```shell
  + @iota/converter@1.0.0-beta.8
  + @iota/core@1.0.0-beta.8
  added 19 packages from 10 contributors and audited 68 packages in 5.307s
  found 0 vulnerabilities
  ```

  これで、package.jsonファイルと、IOTAクライアントライブラリとその依存関係を含む`node_modules`ディレクトリができました。
  <!-- You now have a package.json file and a `node_modules` directory, which contains the IOTA client libraries and their dependencies. -->

3. `iota-example`ディレクトリに、`index.js`という新しいファイルを作成します。
<!-- 3. In the `iota-example` directory, create a new file called `index.js` -->

4. IOTAクライアントライブラリが必要です。
<!-- 4. Require the IOTA client libraries -->

  ```js
  // Require the IOTA libraries
  const Iota = require('@iota/core');
  const Converter = require('@iota/converter');
  ```
5. ノードに接続します。
<!-- 5. Connect to a node -->

  ```js
  // Create a new instance of the IOTA object
  // Use the `provider` field to specify which IRI node to connect to
  const iota = Iota.composeAPI({
  provider: 'https://nodes.devnet.iota.org:443'
  });
  ```

6. `getNodeInfo()`メソッドを呼び出して結果をコンソールに出力します。
<!-- 6. Call the `getNodeInfo()` method and print the results to the console -->

  ```js
  // Call the `getNodeInfo()` method for information about the IRI node
  iota.getNodeInfo()
  // Convert the returned object to JSON to make the output more readable
  .then(info => console.log(JSON.stringify(info, null, 1)))
  .catch(err => {
      // Catch any errors
      console.log(err);
  });
  ```

7. ファイルを保存し、コードを実行します。
<!-- 7. Save the file and run the code -->

  ```bash
  node index.js
  ```

  接続しているIRIノードに関する情報が標準出力に表示されます。
  <!-- Some information about the IRI node that you're connected to should be displayed in the output. -->

  ```json
  {
   "appName": "IRI Testnet",
   "appVersion": "1.5.6-RELEASE",
   "jreAvailableProcessors": 8,
   "jreFreeMemory": 12052395632,
   "jreVersion": "1.8.0_181",
   "jreMaxMemory": 22906667008,
   "jreTotalMemory": 16952328192,
   "latestMilestone": "FPRSBTMKOP9JTTQSHWRGMPT9PBKYWFCCFLZLNWQDFRCXDDHZEFIEDXRIJYIMVGCXYQRHSZQYCTWXJM999",
   "latestMilestoneIndex": 1102841,
   "latestSolidSubtangleMilestone": "FPRSBTMKOP9JTTQSHWRGMPT9PBKYWFCCFLZLNWQDFRCXDDHZEFIEDXRIJYIMVGCXYQRHSZQYCTWXJM999",
   "latestSolidSubtangleMilestoneIndex": 1102841,
   "milestoneStartIndex": 434525,
   "neighbors": 3,
   "packetsQueueSize": 0,
   "time": 1549482118137,
   "tips": 153,
   "transactionsToRequest": 0,
   "features": ["snapshotPruning", "dnsRefresher", "testnet", "zeroMessageQueue", "tipSolidification", "RemotePOW"],
   "coordinatorAddress": "EQQFCZBIHRHWPXKMTOLMYUYPCN9XLMJPYZVFJSAY9FQHCCLWTOLLUGKKMXYFDBOOYFBLBI9WUEILGECYM",
   "duration": 0
  }
  ```

  :::info:表示された内容に関しては以下を参照ください。
  [`getNodeInfo()` APIリファレンス](root://iri/0.1/references/api-reference.md#getnodeinfo)
  :::
  <!-- :::info:Want to know what these fields mean? -->
  <!-- [Take a look at the `getNodeInfo()` API reference](root://iri/0.1/references/api-reference.md#getnodeinfo). -->
  <!-- ::: -->

:::success:
ノードへの接続を確認しました。次にノードにトランザクションを送ることができます。
:::
<!-- :::success: -->
<!-- You've confirmed your connection to the node. Now, you can send a transaction to it. -->
<!-- ::: -->

:::info:ご自身のノードを走らせたいですか？
[Dockerコンテナー内で独自のノードを実行](../tutorials/run-your-own-iri-node.md)して、Devnetノードの代わりにそれに接続することができます。
:::
<!-- :::info:Want to run your own node? -->
<!-- You could [run your own node in a Docker container](../tutorials/run-your-own-iri-node.md) and connect to it instead of the Devnet one. -->
<!-- ::: -->

## バンドルを作成し、ノードに送信する
<!-- ## Create a bundle and send it to a node -->

ノードに接続したら、トランザクションを作成し、トランザクションを[バンドル](../introduction/what-is-a-bundle.md)にまとめて、バンドルをノードに送信できます。これを行うと、トランザクションは[タングル](../introduction/what-is-the-tangle.md)に添付されます。
<!-- When you're connected to a node, you can create a transaction, package it in a [bundle](../introduction/what-is-a-bundle.md), and send it to a node. When you do this, your transaction is attached to [the Tangle](../introduction/what-is-the-tangle.md). -->

1. `index.js`ファイルの最後に、メッセージの送信先アドレスを格納するための変数を作成します。
<!-- 1. . At the end of the `index.js` file, create a variable to store the address to which you want to send a message -->

  ```js
  const address =
  'HELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDHELLOWORLDD';
  ```

  :::info:
  このアドレスは誰にも属している必要はありません。有効であるためには、アドレスはただ81[トライト](root://iota-basics/0.1/concepts/trinary.md)で構成されている必要があります。
  :::
  <!-- :::info: -->
  <!-- This address does not have to belong to anyone. To be valid, the address just needs to consist of 81 [trytes](root://iota-basics/0.1/concepts/trinary.md). -->
  <!-- ::: -->

2. シードを格納するための変数を作成します。これは、メッセージの送信元アドレスを取得するために使用されます。
<!-- 2. Create a variable to store your seed, which will be used to derive an address from which to send the message -->

  ```js
  const seed =
  'PUEOTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX';
  ```

  :::info:
  シードには、81文字の文字コードが含まれている必要があります。シードが81文字未満の場合、ライブラリは末尾に9を追加して81文字にします。
  :::
  <!-- :::info: -->
  <!-- Seeds must contain 81 tryte-encoded characters. If a seed consists of less than 81 characters, the library will append 9s to the end of it to make 81 characters. -->
  <!-- ::: -->

3. アドレスに送信したいメッセージを作成し、それをトライトに変換します。
<!-- 3. Create a message that you want to send to the address and convert it to trytes -->

  ```js
  const message = Converter.asciiToTrytes('Hello World!');
  ```

  :::info:
  IOTAネットワークは、[トライトにエンコードされた](root://iota-basics/0.1/concepts/trinary.md)メッセージのみを受け入れます。
  <!-- IOTA networks accept only [tryte-encoded](root://iota-basics/0.1/concepts/trinary.md) messages. -->
  :::

  :::info:
  `asciiToTrytes()`メソッドは[基本的なASCII文字](https://en.wikipedia.org/wiki/ASCII#Printable_characters)のみをサポートします。その結果、アクセントやウムラウトなどの発音区別符号はサポートされず、`INVALID_ASCII_CHARS`エラーが発生します。
  :::
  <!-- :::info: -->
  <!-- The `asciiToTrytes()` method supports only [basic ASCII characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters). As a result, diacritical marks such as accents and umlauts aren't supported and result in an `INVALID_ASCII_CHARS` error. -->
  <!-- ::: -->

4. トークン量、送信するメッセージ、および送信先のアドレスを指定する転送オブジェクトを作成します。
<!-- 4. Create a transfer object that specifies the value, message to send, and the address to send it to -->

  ```js
  const transfers = [
  {
      value: 0,
      address: address,
      message: message
  }
  ];
  ```

  :::info:トランザクションフィールド
  トランザクションは[他のフィールド](root://iota-basics/0.1/references/structure-of-a-transaction.md)も含まれますが、トークン量・メッセージ・アドレスのフィールドだけでゼロトークントランザクションを送信できます。

  メッセージはトランザクションの`signatureMessageFragment`フィールドに入れられます。
  :::
  <!-- :::info:Transaction fields -->
  <!-- A transaction consists of [other fields](root://iota-basics/0.1/references/structure-of-a-transaction.md), but these ones are all you need to send a zero-value transaction. -->

  <!-- The message is put in the `signatureMessageFragment` field of the transaction. -->
  <!-- ::: -->

5. `transfers`配列を[`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers)メソッドに渡して[バンドル](../introduction/what-is-a-bundle.md)を作成します。このメソッドは、転送オブジェクトからバンドルを作成します。それから、バンドルのトライトを`sendTrytes()`メソッドに渡して、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)、[プルーフオブワーク](root://the-tangle/0.1/concepts/proof-of-work.md)を行い、バンドルを[ノード](../introduction/what-is-a-node.md)に送信します。
<!-- 5. Pass the `transfers` array to the [`prepareTransfers()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.prepareTransfers) method to construct a [bundle](../introduction/what-is-a-bundle.md). This method creates a bundle from the transfer object. Then, pass the bundle's trytes to the `sendTrytes()` method to do [tip selection](root://the-tangle/0.1/concepts/tip-selection.md), [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md), and send the bundle to the [node](../introduction/what-is-a-node.md) -->

  ```js
  iota.prepareTransfers(seed, transfers)
      .then(trytes => {
          return iota.sendTrytes(trytes, 3/*depth*/, 9/*minimum weight magnitude*/)
      })
      .then(bundle => {
      console.log(`Bundle: ${JSON.stringify(bundle, null, 1)}`)
  })
  .catch(err => {
          // Catch any errors
      console.log(err);
  });
  ```

  :::info:Depth
  `depth`引数はチップ選択に影響します。`depth`が深ければ深いほど（タングルの奥に戻るほど）、重み付けされたランダムウォークが始まります。
  :::
  <!-- :::info:Depth -->
  <!-- The `depth` argument affect tip selection. The greater the depth, the farther back in the Tangle the weighted random walk starts. -->
  <!-- ::: -->

  :::info:Minimum weight magnitude
  [`minimum weight magnitude`](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)（MWM）引数は、プルーフオブワーク（PoW）の困難さに影響を与えます。 MWMが大きいほど、PoWがより困難になります。

  すべてのIOTAネットワークはそれぞれのMWMを実施します。 Devnetでは、MWMは9ですが、MainnetではMWMは14です。小さすぎるMWMを使用すると、トランザクションは無効になります。
  :::
  <!-- :::info:Minimum weight magnitude -->
  <!-- The [`minimum weight magnitude`](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md) (MWM) argument affects the difficulty of proof of work (PoW). The greater the MWM, the more difficult the PoW. -->

  <!-- Every IOTA network enforces its own MWM. On the Devnet, the MWM is 9. But, on the Mainnet the MWM is 14. If you use a MWM that's too small, your transactions won't be valid. -->
  <!-- ::: -->

6. ファイルを保存して、コードを実行する。
<!-- 6. Save the file and run the code -->

  ```bash
  node index.js
  ```

:::success:おめでとうございます:tada:
最初のゼロトークントランザクションを送信できました。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent your first zero-value transaction. -->
<!-- ::: -->

コンソールには、送信した[バンドル](../introduction/what-is-a-bundle.md)内のトランザクションに関する情報が表示されます。
<!-- In the console, you'll see information about the transaction in the [bundle](../introduction/what-is-a-bundle.md) that you sent. -->

このトランザクションは、すべてのノードがトランザクションを台帳に入れるまでIOTAネットワークを介して伝播します。
<!-- Your transaction will propagate through the IOTA network until all the nodes have it in their ledgers. -->

## コードを実行する
<!-- ## Run the code -->

このチュートリアルのサンプルコードを実行してWebブラウザで結果を確認するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this tutorial and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/51-Send-ASCII-Data?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

:::info:トランザクションフィールドの意味は？
[トランザクションの構造](root://iota-basics/0.1/references/structure-of-a-transaction.md)
:::
<!-- :::info:Want to know what the transaction fields mean? -->
<!-- [Take a look at the structure of a transaction](root://iota-basics/0.1/references/structure-of-a-transaction.md). -->
<!-- ::: -->

## 次のステップ
<!-- ## Next steps -->

`バンドル`がネットワーク上にある（タングルに添付された）ことを確認するには、コンソールからbundleフィールドの値をコピーし、[Devnet タングルエクスプローラ](https://devnet.thetangle.org/)を開き、bundleフィールドの値を検索バーに貼り付けます。
<!-- To confirm that your bundle is on the network (attached to the Tangle), copy the value of the `bundle` field from the console, open a [Devnet Tangle explorer](https://devnet.thetangle.org/), and paste the value into the search bar. -->

自分のトランザクションがどのトランザクションでタングルに添付されているかをチェックするにはParent transactionsフィールドを見てください。
<!-- See the Parent transactions field to check which transactions your transaction is attached to in the Tangle. -->

Parent transactionsはチップ選択中に選択され、送信したトランザクションの[branchTransactionとtrunkTransactionフィールド](root://iota-basics/0.1/references/structure-of-a-transaction.md)に追加されました。
<!-- These transactions were chosen during tip selection and added to the [`branchTransaction` and `trunkTransaction` fields](root://iota-basics/0.1/references/structure-of-a-transaction.md) of your transaction. -->

:::info:より多くのトランザクションをまとめて送信したいですか？
[2つのトランザクションを含むバンドルを送信](root://iota-basics/0.1/how-to-guides/send-bundle.md)して、バンドルがどのように構成されているかを学んでみましょう。
:::
<!-- :::info:Want to send more transactions in a bundle? -->
<!-- [Send a bundle of two transactions](root://iota-basics/0.1/how-to-guides/send-bundle.md) and learn how bundles are structured. -->
<!-- ::: -->

:::info:ノードを走らせたいですか？
[ノードソフトウェアの詳細](root://iri/0.1/introduction/overview.md)を調べ、[ノードの実行に関する詳細なガイド](root://iri/0.1/how-to-guides/run-an-iri-node-on-linux.md)をご覧ください。
:::
<!-- :::info:Interested in running a node? -->
<!-- [Discover more about node software](root://iri/0.1/introduction/overview.md) and [follow an in-depth guide on running a node](root://iri/0.1/how-to-guides/run-an-iri-node-on-linux.md). -->
<!-- ::: -->
