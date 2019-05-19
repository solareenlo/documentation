# タングルとは？

**タングルは、トランザクション間の接続によって形成されるデータ構造です。 タングル内のトランザクションは、保留中または確定済みの2つの状態のいずれかになります。**
<!-- **The Tangle is the data structure that's formed by the connections among transactions. Transactions in the Tangle can be in one of two states: Pending or confirmed.** -->

トランザクションの検証基準の1つは、それぞれのトランザクションが前の2つのトランザクションを直接参照しなければならないということです。トランザクション間を接続して、トランザクションをタングルに_添付する_のはこれらの参照です。
<!-- One of the validation criteria of a transaction is that each one must directly reference two previous ones. It's these references that connect transactions and _attach_ them to the Tangle. -->

この参照モデルは、各トランザクションが1つの頂点を表す、一種の[有向非巡回グラフ](https://en.wikipedia.org/wiki/Directed_acyclic_graph)（DAG）を形成します。
<!-- This referencing model forms a type of [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (DAG), in which each transaction represents a vertex. -->

![A directed acyclic graph](../images/dag.png)

この図では、トランザクション5はトランザクション6によって直接参照され、トランザクション5はトランザクション3を直接参照するため、トランザクション3はトランザクション6によって間接的に参照されます。
<!-- In this diagram, transaction 5 is **directly** referenced by transaction 6, and because transaction 5 directly references transaction 3, transaction 3 is **indirectly** referenced by transaction 6. -->

IOTAトークンは、バンドル内のすべてのトランザクションが確定されるまでは転送できません。保留状態から確定済み状態に移行するには、ノードはトランザクションの状態について合意に達する必要があります。現時点では、ノードはマイルストーンによって直接的または間接的に参照されるトランザクションについてのみ合意に達します。
<!-- IOTA tokens can't be transferred until all transactions in a bundle are confirmed. To go from a pending state to a confirmed state, nodes must reach consensus on the state of a transaction. At the moment, nodes reach a consensus on transactions that are directly or indirectly referenced by a milestone. -->

[コーディネーター、チップ選択、およびタングルについての詳細を学んでみましょう。](root://the-tangle/0.1/introduction/overview.md)
<!-- [Learn more about the Coordinator, tip selection, and the Tangle](root://the-tangle/0.1/introduction/overview.md). -->

