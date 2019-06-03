# トランザクションが確定したか確認する
<!-- # Check if a transaction is confirmed -->

**IOTAトークンを転送する前に、それらを転送するバンドルを確認する必要があります。バンドル内のトランザクションは、最後のトランザクションがマイルストーンによって参照および承認されるまで保留状態のままになります。**
<!-- **Before IOTA tokens can be transferred, the bundle that transfers them must be confirmed. Transactions in a bundle remain in a pending state until the tail transaction is referenced and approved by a milestone.** -->

:::info:
コーディネーター、マイルストーン、または確定という用語に慣れていない場合は、[タングルについて](root://the-tangle/0.1/introduction/overview.md)を読むことをお勧めします。
:::
<!-- :::info: -->
<!-- If you're unfamiliar with the terms Coordinator, milestone, or confirmation, we recommend that you [read about the Tangle](root://the-tangle/0.1/introduction/overview.md). -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには, 次のものが必要です.
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

2. コマンドプロンプトで、`iota-basics`ディレクトリに移動し、[IOTAコアライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/core)をインストールします。
<!-- 2. In the command prompt, change into the `iota-basics` directory, and install the [IOTA Core library](https://github.com/iotaledger/iota.js/tree/next/packages/core) -->

  ```bash
  cd iota-basics
  npm install --save @iota/core
  ```

3. `iota-basics`ディレクトリに、`check-confirmation.js`という名前の新しいファイルを作成します。
<!-- 3. In the `iota-basics` directory, create a new file called `check-confirmation.js` -->

4. `check-confirmation.js`ファイルでは、IOTAクライアントライブラリが必要です。
<!-- 4. In the `check-confirmation.js` file, require the IOTA client library -->

  ```js
  const Iota = require('@iota/core');
  ```

5. IOTAオブジェクトのインスタンスを作成し、`provider`フィールドを使用してノードに接続します
<!-- 5. Create an instance of the IOTA object and use the `provider` field to connect to a node -->

  ```js
  const iota = Iota.composeAPI({
  provider: 'https://nodes.devnet.iota.org:443'
  });
  ```

6. [devnet.thetangle.org](https://devnet.thetangle.org/)に行き、確定したトランザクションを見つけてください
<!-- 6. Go to [devnet.thetangle.org](https://devnet.thetangle.org/) and find a confirmed transaction -->

  :::info:確定したトランザクションが見つかりませんか?
  最新のマイルストーン欄でトランザクションハッシュをクリックし、次にブランチトランザクションハッシュをクリックします。このトランザクションはマイルストーンによって参照および承認されているため、確定済みの状態です。
  :::
  <!-- :::info:Can't find a confirmed transaction? -->
  <!-- Click a transaction hash in the Latest milestones box, then click the branch transaction hash. This transaction is referenced and approved by the milstone, so it is in a confirmed state. -->
  <!-- ::: -->

7. トランザクションハッシュを[`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion)メソッドに渡して、IRIノードの最新のソリッドサブタングルマイルストーンが承認したかどうかを確認します。
<!-- 7. Pass the transaction hash to the [`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion) method to check if the IRI node's latest solid subtangle milestone approves it -->

  ```js
  iota.getLatestInclusion(['TRANSACTION HASH'])
  .then(states => console.log(states));
  ```

  ファイルを実行すると、`true`のブーリアンを含む配列が表示されます。これは、トランザクションが確定されたことを意味します。
  <!-- When you execute the file, you should see an array that contains the `true` boolean, meaning that the transaction is confirmed. -->

  :::info:
  また、`getInclusionStates()`メソッドを使用して、選択した一連のトランザクションによってトランザクションが承認されているかどうかを確認することもできます。
  :::
  <!-- :::info: -->
  <!-- You could also use the `getInclusionStates()` method to check if a transaction is approved by an array of your own chosen transactions. -->
  <!-- ::: -->

8. [devnet.thetangle.org](https://devnet.thetangle.org)に行き、保留中のトランザクションを見つけてください.
<!-- 8. Go to [devnet.thetangle.org](https://devnet.thetangle.org) and find a pending transaction -->

  :::info:保留中のトランザクションが見つかりませんか?
  最新のトランザクション欄でトランザクションハッシュをクリックします。このトランザクションはチップなので、保留状態にあります。
  :::
  <!-- :::info:Can't find a pending transaction? -->
  <!-- Click a transaction hash in the Latest transactions box. This transaction is a tip, so it is in a pending state. -->
  <!-- ::: -->

9. トランザクションハッシュを`getLatestInclusion()`メソッドに渡して、IRIノードの最新のソリッドサブタングルマイルストーンが承認したかどうかを確認します。
<!-- 9. Pass the transaction hash to the `getLatestInclusion()` method to check if the IRI node's latest solid subtangle milestone approves it -->

  ```js
  iota.getLatestInclusion(['TRANSACTION HASH'])
  .then(states => console.log(states));
  ```

  ファイルを実行すると、`false`ブーリアンを含む配列が表示されます。これは、トランザクションがまだ確定されていないことを意味します。
  <!-- When you execute the file, you should see an array that contains the `false` boolean, meaning that the transaction is not yet confirmed. -->

## コードを走らせる
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには、緑色のボタンをクリックします。
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Check-transaction-confirmation?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
