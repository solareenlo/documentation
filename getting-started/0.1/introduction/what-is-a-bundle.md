# バンドルとは？

バンドルは、データを送信したり、ノードにIOTAトークンを特定のアドレスから他のアドレスに転送するように指示する1つ以上のトランザクショングループです。バンドル内の各トランザクションの運命は、残りの部分に依存します。つまりすべてのトランザクションが有効か、またはどれも無効かです。
<!-- **A bundle is a group of one or more transactions, which send data or instruct a node to transfer IOTA tokens from certain addresses to others. The fate of each transaction in a bundle depends on the rest. Either all transactions are valid or none of them are.** -->

バンドルの構造は、head、body、およびtailから構成されます。tailはインデックス0、headはバンドル内の最後のトランザクションです。
<!-- The structure of a bundle consists of a head, a body, and a tail, where the tail is index 0 and the head is the last transaction in the bundle. -->

tailから始まるすべてのトランザクションは、次のインデックスを持つトランザクションへの参照によって接続されます。たとえば、tailトランザクションはトランザクション1に接続され、トランザクション1はトランザクション2に接続され、以下同様に続きます。これらの接続により、ノードはバンドルを再構築してその内容を検証できます。
<!-- All transactions, starting from the tail, are connected by reference to the one with the next index. For example, the tail transaction is connected to transaction 1, which is connected to transaction 2, and so on. These connections allow nodes to reconstruct bundles and validate their contents. -->

タングルにトランザクションを添付するために、headトランザクションはタングル内の他の2つのバンドルのtailに接続されます。 （tailトランザクションとbodyトランザクションは、2つのtailの1つにも接続されています。）この2つのtailトランザクションは、[チップ選択](root://the-tangle/0.1/concepts/tip-selection.md)時にノードによって選択されます。
<!-- To attach transactions to the Tangle, the head transaction is connected to the tails of two other bundles in the Tangle. (The tail and body transactions are connected to one of those tails as well.) These tail transactions are selected by nodes during [tip selection](root://the-tangle/0.1/concepts/tip-selection.md). -->

## IOTAトークンを転送するバンドルの例
<!-- ## Example of a bundle that transfers IOTA tokens -->

IOTAトークンを転送するには、バンドルに少なくとも1つの入力トランザクションと1つの出力トランザクションが含まれている必要があります。これらのバンドルは転送バンドルと呼ばれます。
<!-- To transfer IOTA tokens, a bundle must contain at least one input and one output transaction. These bundles are called transfer bundles. -->

今回の例では、受取人Aに100Miを送信したいとし、トークンが3つのアドレス0, 1, 2に別れているとします。
<!-- In this example, you want to send 100Mi to recipient A, and your balance is distributed among three addresses: -->

* **アドレス 0:** 20Mi
* **アドレス 1:** 30Mi
* **アドレス 2:** 55Mi

受取人Aに100Miを送信するには、以下のトランザクションを作成して転送バンドルとしてノードに送信する必要があります。
<!-- To send 100Mi to recipient A, you must create the following transactions and send them to a node as a transfer bundle: -->

* **入力トランザクション：**私のアドレス0, 1, 2から100Miを出金し、私がその3つのアドレスを所有していることを検証するために署名をチェックするトランザクション
<!-- * **Input transaction:** Withdraw 100Mi from my address and check the signature to verify that I own it -->
* **出力トランザクション：**受取人Aのアドレスへ100Miの入金をするトランザクション
<!-- * **Output transaction:** Deposit 100Mi to the recipient's address -->
* **出力トランザクション：**アドレス2から残りの5Miをアドレス3に入金するトランザクション
<!-- * **Output transaction:** Deposit the remaining 5Mi from address 2 into address 3 -->

:::danger:
[1度以上出金したアドレスからは出金してはいけません。](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse)したがって、転送バンドルでは、出金されるアドレスの残りのトークンを新しいアドレスに入金するために、追加の出力トランザクションが必要になります。
:::
<!-- :::danger: -->
<!-- [You must not withdraw from an address more than once](root://iota-basics/0.1/concepts/addresses-and-signatures.md#address-reuse). So, a transfer bundle may require an extra output transaction to deposit the remaining balance of a withdrawn address into a new address. -->
<!-- ::: -->

[バンドルの詳細を学んでみましょう。](root://iota-basics/0.1/concepts/bundles-and-transactions.md)

