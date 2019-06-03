# 保留中のバンドルを確定させる
<!-- # Confirm a pending bundle -->

**マイルストーンによって承認されるためには, チップ選択時にトランザクションが選択される必要があります. これは, 古いトランザクションよりも新しいトランザクションを優先します. したがって, バンドルが保留状態に長く固執しているほど, 確定される可能性は低くなります. バンドルが確定される可能性を高めるためには, 状況に応じてトランザクションを再添付して促進することが必要です.**
<!-- **To be approved by a milestone, a transaction must be selected during tip selection, which favors new transactions over old ones. Therefore, the longer a bundle is stuck in a pending state, the less likely it is to be confirmed. To increase the chances of a bundle being confirmed, you can reattach and promote it, depending on the circumstances.** -->

:::info:
再添付, 再ブロードキャスト, または促進という用語に慣れていない場合は, [これらのコンセプトについて](../concepts/reattach-rebroadcast-promote.md)を読むことをお勧めします.
:::
<!-- :::info: -->
<!-- If you're unfamiliar with the terms reattach, rebroadcast, or promote, we recommend that you [read about these concepts](../concepts/reattach-rebroadcast-promote.md). -->
<!-- ::: -->

## 前提条件
<!-- ## Prerequisites -->

このガイドを完成するには, 次のものが必要です.
<!-- To complete these guides, you need the following: -->

* [Node.js (8+)](https://nodejs.org/en/)
* [Visual Studio Code](https://code.visualstudio.com/Download)のようなコードエディタ
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* インターネット接続
<!-- * An Internet connection -->
* 保留中のバンドルの末尾のトランザクションハッシュ（[`currentIndex` 0](../references/structure-of-a-bundle.md)）. [ゼロトークントランザクションのバンドルを送信する](../how-to-guides/send-bundle.md)の記事に従って作成することができます.
<!-- * A tail transaction hash ([`currentIndex` 0](../references/structure-of-a-bundle.md)) from any pending bundle. You can create one by following the ['Send a bundle of zero-value transactions' article](../how-to-guides/send-bundle.md) -->

---

バンドルがチップ選択中に選択されるには古すぎる場合や, 二重支払い(矛盾したサブタングル)のような無効な状態にあるタングルの一部に添付されている場合など, さまざまな理由でバンドルが保留状態のままになることがあります.
<!-- A bundle can be stuck in a pending state for many reasons, for example if it's too old to be selected during tip selection or if it's attached to a part of the Tangle that leads to an invalid state such as a double-spend (inconsistent subtangle). -->

保留中のトランザクションが確定される可能性を高めるために, 再添付または促進することができます.
<!-- To increase the chances of a pending transaction being confirmed, you can either reattach or promote it. -->

このガイドでは, 30秒ごとに次のことを行うスクリプトを作成します.
<!-- In this guide, you'll create a script that does the following every 30 seconds: -->

* 末尾のトランザクションが確定されたかどうかを確認する
<!-- * Check if the tail transaction has been confirmed -->
* トランザクションがまだ保留中の場合は, 促進または再添付を行う
<!-- * If the transaction is still pending, promote or reattach it -->

## ファイルとディレクトリを作成する
<!-- ## Create the file and directory -->

このガイドのサンプルコードはIOTAコアライブラリを使用しています. このライブラリを使用するには, 作業ディレクトリを作成して`npm`パッケージマネージャを使用してインストールする必要があります.
<!-- The sample code for this guide uses the IOTA core library. To use this library, you must create a working directory and install it using the `npm` package manager. -->

1. `iota-basics`という新しいディレクトリを作成します.
<!-- 1. Create a new directory called `iota-basics` -->

2. `iota-basics`ディレクトリに移動し, [IOTAコアライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/core)をインストールします.
<!-- 2. Change into the `iota-basics` directory, and install the [IOTA core library](https://github.com/iotaledger/iota.js/tree/next/packages/core) -->

  ```bash
  cd iota-basics
  npm install --save @iota/core
  ```

3. `iota-basics`ディレクトリに, `pending-to-confirmed.js`という名前の新しいファイルを作成します.
<!-- 3. In the `iota-basics` directory, create a new file called `pending-to-confirmed.js` -->

## タイマー関数を作成する
<!-- ## Create a timer function -->

バンドルの確定にかかる時間を知りたい場合は, タイマー関数を作成してください.
<!-- If you want to know how long it took for a bundle to be confirmed, create a timer function. -->

1. `pending-to-confirmed.js`ファイルでは, IOTAライブラリが必要です.
<!-- 1. In the pending-to-confirmed.js file, require the IOTA library -->

  ```js
  const Iota = require('@iota/core');
  ```

2. 確定したい保留中のバンドルの末尾のトランザクションハッシュと再添付された時のバンドルの末尾のトランザクションハッシュを格納するための配列変数を作成します.
<!-- 2. Create an array variable to store the tail transaction hash of the pending bundle that you want to confirm as well as the tail transaction hashes of any future reattached bundles -->

  ```js
  const tails = ["tail transaction hash"];
  ```

3. タイマーの秒数を格納するための変数を作成します.
<!-- 3. Create a variable to store the number of seconds for the timer -->

  ```js
  var seconds = 0;
  ```

4. バンドル確定されたかを測定するためのタイマーを設定します. 毎秒, タイマーは`seconds`変数を1ずつ増やします.
<!-- 4. Set the timer to measure how long it takes for the bundle to be confirmed. Every second, the timer will increment the `seconds` variable by one. -->

  ```js
  var timer = setInterval(stopWatch, 1000);
  function stopWatch (){
  seconds++
  }
  ```

## バンドルを自動促進および自動再添付する機能を作成する
<!-- ## Create a function to auto-promote and auto-reattach bundles -->

バンドルを促進して再添付するには, 末尾のトランザクションハッシュをクライアントライブラリの関連する関数に渡す必要があります.
<!-- To promote and reattach a bundle, you need to pass its tail transaction hash to the relevant function in the client library. -->

[isPromotable](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.isPromotable)メソッドは, 末尾のトランザクションが一貫していることと, 最新の6マイルストーンより前に末尾のトランザクションがタングルに添付されていないことを確認します.
<!-- The [`isPromotable()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.isPromotable) method checks if the tail transaction is consistent and was not attached to the Tangle before the most recent 6 milestones. -->

末尾のトランザクションが促進可能であれば, [`encourageTransaction()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.promoteTransaction)メソッドは末尾のトランザクションを促進します.
<!-- If the tail transaction is promotable, the [`promoteTransaction()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.promoteTransaction) method promotes it. -->

末尾のトランザクションが促進できない場合は, [`replayBundle()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.replayBundle)メソッドがバンドルを再添付し, 後で確定されたかどうかを確認するために, 新しい再添付されたバンドルの末尾のトランザクションハッシュが`tails`配列に追加されます.
<!-- If the tail transaction isn't promotable, the [`replayBundle()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.replayBundle) method reattaches the bundle, then the new reattached bundle's tail transaction hash is added to the `tails` array so that it can be checked for confirmation later on. -->

```js
function autoPromoteReattach (tail) {
  iota.isPromotable(tail)
    .then(promote => promote
    ? iota.promoteTransaction(tail, 3, 14)
        .then(()=> {
            console.log(`Promoted transaction hash: ${tail}`);
        })
    : iota.replayBundle(tail, 3, 14)
        .then(([reattachedTail]) => {
            const newTailHash = reattachedTail.hash;

            console.log(`Reattached transaction hash: ${tail}`);

            // Keep track of all reattached tail transaction hashes to check for confirmation
            tails.push(newTailHash);
        })
    )
    .catch((error)=>{
         console.log(error);
    });
}
```

## 定期的に確定を確認する機能を作成する
<!-- ## Create a function to check for confirmation at regular intervals -->

末尾のトランザクション配列が確定しかたどうかを定期的にチェックできるようにするには, `setInterval()`関数に渡すことができる関数が必要です.
<!-- To be able to check the array of tail transactions for confirmation at regular intervals, you need a function that can be passed to a `setInterval()` function. -->

[`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion)メソッドは, 配列内の末尾のトランザクションのいずれかが確定されたかどうかを確認します. いずれかのトランザクションが確定された場合, このメソッドは`true`を返します.
<!-- The [`getLatestInclusion()`](https://github.com/iotaledger/iota.js/blob/next/api_reference.md#module_core.getLatestInclusion) method checks if any of the the tail transactions in the array have been confirmed. If any of the transactions have been confirmed this method returns `true`. -->

`tail`変数は, 最新の再添付を促進または再添付できるように, 最新の末尾のトランザクションを配列に格納します.
<!-- The `tail` variable stores the last tail transaction in the array so that the latest reattachment can be promoted or reattached. -->

末尾のトランザクションがまだ確定されていない場合は, `tail`変数が[`autoPromoteReattach()`](#create-a-function-to-auto-promote-and-auto-reattach-bundles)関数に渡されます.
<!-- If none of the tail transactions have been confirmed yet, the `tail` variable is passed to the [`autoPromoteReattach()`](#create-a-function-to-auto-promote-and-auto-reattach-bundles) function. -->

末尾のトランザクションが確定された場合, 確定に要した分数とともにコンソールに記録されます.
<!-- If a tail transaction has been confirmed, it's logged to the console along with the number of minutes it took to confirm. -->

```js
function autoConfirm(tails){
console.log(tails);
    iota.getLatestInclusion(tails)
        .then(states => {
            // Check that none of the transactions have been confirmed
            if (states.indexOf(true) === -1) {
                // Get latest tail hash
                const tail = tails[tails.length - 1]
                autoPromoteReattach(tail);
            } else {
                console.log(JSON.stringify(states,null, 1));
                clearInterval(interval);
                clearInterval(timer);
                var minutes = (seconds / 60).toFixed(2);
                var confirmedTail = tails[states.indexOf(true)];
                console.log(`Confirmed transaction hash in ${minutes} minutes: ${confirmedTail}`);
                return;
            }
        }).catch(error => {
            console.log(error);
        }
    );
}
```


## コードを走らせる
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには, 緑色のボタンをクリックします.
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

このサンプルコードを実行する前に, 保留中の末尾のトランザクションハッシュを見つけ, それを`tails`配列に格納します.
<!-- Before you run this sample code, find a pending tail transaction hash and store it in the `tails` array. -->

:::info:保留中のトランザクションが見つかりませんか?
[devnet.thetangle.org](https://devnet.thetangle.org)に行き, Latest transactions欄でトランザクションハッシュをクリックします. このトランザクションはチップなので, 保留状態にあります.
:::
<!-- :::info:Can't find a pending transaction? -->
<!-- Go to [devnet.thetangle.org](https://devnet.thetangle.org) and click a transaction hash in the Latest transactions box. This transaction is a tip, so it is in a pending state. -->
<!-- ::: -->

<iframe height="500px" width="100%" src="https://repl.it/@jake91/Confirm-pending-bundle?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

:::info:
このサンプルコードは, 完了までに数分かかることがあります.  `Started autoConfirm() function => undefined`と表示された場合, コードはバックグラウンドで実行されています. コードが終了するまで待ちます. コンソールにメッセージが表示されるはずです.
:::
<!-- :::info: -->
<!-- This sample code may take a few minutes to complete. If you see `Started autoConfirm() function => undefined`, the code is running in the background. Wait until the code finishes. You should see messages appear in the console. -->
<!-- ::: -->
