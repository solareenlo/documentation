# IOTAネットワーク
<!-- # IOTA networks -->

** IOTAネットワークは、互いに隣接し合うノードから構成されています。**
<!-- **An IOTA network consists of nodes that are mutually connected to neighbors.** -->

IOTAには、以下の[無許可型ネットワーク](../introduction/what-is-dlt.md)があります。
<!-- IOTA has the following [permissionless networks](../introduction/what-is-dlt.md): -->
* **メインネット:** IOTAトークン
* **デブネット:** デブネットトークン（無料）
* **スパムネット:** スパムネットトークン（無料）
<!-- * **Mainnet:** IOTA token -->
<!-- * **Devnet:** Devnet token (free) -->
<!-- * **Spamnet:** Spamnet token (free) -->

すべての無許可型ネットワークは、ノード、クライアント、およびコーディネーターで構成されています。
<!-- All permissionless networks consist of nodes, clients, and the Coordinator. -->

:::info:
許可制（プライベート）ネットワーク上でアプリケーションを作成してテストしたい場合は、[コンパス](root://compass/0.1/introduction/overview.md)と呼ばれるオープンソースのコーディネーターコードのインスタンスを実行することで行うことができます。
:::
<!-- :::info: -->
<!-- If you want to create and test an application on a permissioned (private) network, you can do so by running an instance of the open-source Coordinator code called [Compass](root://compass/0.1/introduction/overview.md). -->
<!-- ::: -->

## メインネット
<!-- ## Mainnet -->

暗号通貨交換からIOTAトークンを購入すると、購入したIOTAトークンはメインネットで有効になります。
<!-- When you buy IOTA tokens from a cryptocurrency exchange, those tokens are valid on the Mainnet. -->

:::info:
暗号通貨取引所は、IOTAトークンをMIOTA（1,000,000）と呼ばれるメガIOTAの単位で販売しています。

[IOTAトークンの単位](root://iota-basics/0.1/references/units-of-iota-tokens.md)について詳しく学んでみましょう。
:::
<!-- :::info: -->
<!-- Cryptocurrency exchanges sell IOTA tokens in denominations of Mega IOTA, which is also written as MIOTA (1,000,000). -->
<!--  -->
<!-- Learn more about [units of IOTA tokens](root://iota-basics/0.1/references/units-of-iota-tokens.md). -->
<!-- ::: -->

![Mainnet configuration](../images/mainnet-configuration.png)

### Minimum weight magnitude

Mainnet上のトランザクションが有効であるためには、14の[minimum weight magnitude](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)（MWM）を使用しなければなりません。
<!-- Transactions on the Mainnet must use a [minimum weight magnitude](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md) (MWM) of 14 to be valid. -->

## デブネット
<!-- ## Devnet -->

デブネットはメインネットのコピーです。
<!-- The Devnet is a copy of the Mainnet. -->

このネットワークでは、アプリケーションをテストして、無料のデブネットトークンを使用するPoCを構築できます。
<!-- On this network, you can test your applications and build proof of concepts that use free Devnet tokens. -->

:::info:無料のデブネットトークンを受け取る
[蛇口ウェブサイト](https://faucet.devnet.iota.org)は指定されたアドレスに1Ki（1000）のデブネットトークンを送ります。
:::
<!-- :::info:Receive free Devnet tokens -->
<!-- The [faucet website](https://faucet.devnet.iota.org) sends 1Ki (1000) Devnet tokens to your specified address. -->
<!-- ::: -->

![Devnet Configuration](../images/devnet-configuration.png)

### Minimum weight magnitude

デブネット上のトランザクションが有効になるには、9の[minimum weight magnitude](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)（MWM）を使用する必要があります。メインネットと比較して、このMWMはプルーフオブワーク（PoW）が完了するのにかかる時間を短縮します。
<!-- Transactions on the Devnet must use a [minimum weight magnitude](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md) (MWM) of 9 to be valid. Compared to the Mainnet, this MWM reduces the time it takes for [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md) (PoW) to be completed. -->

### IRIノード

IOTA財団はデブネット上で以下のパブリックIRIノードをホストしています。
<!-- We host the following public IRI nodes on the Devnet: -->

#### ロードバランサノード
<!-- #### Load balancer node -->

このエンドポイントを使用すると、デブネット上でIRIノードを実行している高可用性プロキシサーバーにアクセスできます。
<!-- This endpoint gives you access to a high-availability proxy server, which is running an IRI node on the Devnet. -->

ロードバランサを使用してトランザクションを送信し、IRIノードから台帳に関する情報を要求します。
<!-- Use the load balancer for sending transactions and requesting information about the ledger from the IRI node. -->

**URL:** https://nodes.devnet.iota.org:443

#### ZMQノード

このエンドポイントを使用すると、デブネット上のIRIノードの[ZMQ](root://iri/0.1/concepts/zero-message-queue.md)にアクセスできます。
<!-- This endpoint gives you access to the [zero message queue](root://iri/0.1/concepts/zero-message-queue.md) of an IRI node on the Devnet. -->

ZMQノードを使用して、IRIノード内のイベントにサブスクライブします。
<!-- Use the ZMQ node to subscribe to events in an IRI node. -->

**URL:** tcp://zmq.testnet.iota.org:5556

#### PoWノード

このエンドポイントを使用すると、プルーフオブワークを行うことができるIRIノードにアクセスできます。
<!-- This endpoint gives you access to an IRI node that can do proof of work. -->

PoWノードを使用して小型デバイスの電力を節約します。
<!-- Use the PoW node to save power on small devices. -->

**URL:** https://powbox.devnet.iota.org

## スパムネット
<!-- ## Spamnet -->

スパムネットは、トランザクションをスパムするアプリケーション用です。
<!-- The Spamnet is for applications that spam transactions. -->

このネットワークでは、アプリケーションをテストして、無料のスパムネットトークンを使用するPoCを構築できます。
<!-- On this network, you can test your applications and build proof of concepts that use free Spamnet tokens. -->

:::info:無料のスパムネットトークンを受け取る
[蛇口ウェブサイト](https://faucet.spamnet.iota.org)は指定されたアドレスに1Ki（1000）のスパムネットトークンを送ります。
:::
<!-- :::info:Receive free Spamnet tokens -->
<!-- The [faucet website](https://faucet.spamnet.iota.org) sends 1Ki (1000) Spamnet tokens to your specified address. -->
<!-- ::: -->

![Spamnet configuration](../images/spamnet-topology.png)

### Minimum weight magnitude

スパムネット上のトランザクションが有効になるには、7の[minimum weight magnitude](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md)（MWM）を使用する必要があります。 メインネットと比較して、このMWMはプルーフオブワーク（PoW）が完了するのにかかる時間を短縮します。
<!-- Transactions on the Spamnet must use a [minimum weight magnitude](root://iota-basics/0.1/concepts/minimum-weight-magnitude.md) (MWM) of 7 to be valid. Compared to the Mainnet, this MWM reduces the time it takes for [proof of work](root://the-tangle/0.1/concepts/proof-of-work.md) (PoW) to be completed. -->

### IRIノード

IOTA財団はスパムネット上で以下のパブリックIRIノードをホストしています。
<!-- We host the following public IRI nodes on the Spamnet: -->

#### ロードバランサノード

このエンドポイントにより、スパムネット上でIRIノードを実行している高可用性プロキシサーバーにアクセスできます。
<!-- This endpoint gives you access to a high-availability proxy server, which is running an IRI node on the Spamnet. -->

ロードバランサを使用してトランザクションを送信し、IRIノードから台帳に関する情報を要求します。
<!-- Use the load balancer for sending transactions and requesting information about the ledger from the IRI node. -->

**URL:** https://nodes.spamnet.iota.org:443

#### ZMQノード

このエンドポイントを使用すると、スパムネット上のIRIノードの[ZMQ](root://iri/0.1/concepts/zero-message-queue.md)にアクセスできます。
<!-- This endpoint gives you access to the [zero message queue](root://iri/0.1/concepts/zero-message-queue.md) of an IRI node on the Spamnet. -->

ZMQノードを使用して、IRIノード内のイベントにサブスクライブします。
<!-- Use the ZMQ node to subscribe to events in an IRI node. -->

**URL:** tcp://zmq.spamnet.iota.org:5556
