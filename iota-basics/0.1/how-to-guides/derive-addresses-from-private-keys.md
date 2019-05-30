# 秘密鍵からアドレスを作る
<!-- # Derive addresses from private keys -->

**シードは秘密鍵の導出に使用され, 次に秘密鍵はアドレスの導出およびバンドルの署名に使用されます. 暗号化ライブラリを使用して秘密鍵からアドレスを取得することで, アドレス, 秘密鍵, およびセキュリティレベルの間の関係についてよりよく理解することができます.**
<!-- **Seeds are used to derive private keys, and in turn, a private key is used to derive addresses and sign bundles. By using the cryptography library to derive addresses from private keys, you can gain a better understanding of the relationship among addresses, private keys, and security levels.** -->

:::info:
秘密鍵, サブシード, およびキーダイジェストという用語に慣れていない場合は, [アドレスと署名](../concepts/addresses-and-signatures.md)についてを読むことをお勧めします.
:::
<!-- :::info: -->
<!-- If you're unfamiliar with the terms private key, subseed, and key digest, we recommend [reading about addresses and signatures](../concepts/addresses-and-signatures.md). -->
<!-- ::: -->

## 必要条件
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

1. `iota-basics`という新しいディレクトリを作成します.
<!-- 1. Create a new directory called `iota-basics` -->

2. `iota-basics`ディレクトリに移動し, [IOTAコアライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/core), [IOTAコンバータライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/converter), および[IOTA署名ライブラリ](https://github.com/iotaledger/iota.js/tree/next/packages/signing)をインストールします.
<!-- 2. Change into the `iota-basics` directory, and install the [IOTA core library](https://github.com/iotaledger/iota.js/tree/next/packages/core), the [IOTA converter library](https://github.com/iotaledger/iota.js/tree/next/packages/converter), and the [IOTA signing library](https://github.com/iotaledger/iota.js/tree/next/packages/signing) -->

  ```bash
  cd iota-basics
  npm install --save @iota/core
  npm install --save @iota/converter
  npm install --save @iota/signing
  ```

3. `iota-basics`ディレクトリに, `create-private-key-address.js`という新しいファイルを作成します.
<!-- 3. In the `iota-basics` directory, create a new file called `create-private-key-address.js` -->

4. `create-private-key-address.js`ファイルには, 先ほどインストールした3つのIOTAライブラリが必要です.
<!-- 4. In the `create-private-key-address.js` file, require the IOTA libraries -->

  ```js
  const Iota = require('@iota/core');
  const Sign = require('@iota/signing');
  const Converter = require('@iota/converter');
  ```

5. `subseed`メソッドに, トライトに変換したシードとインデックスを渡してサブシードを導出します.
<!-- 5. Derive a subseed by passing a seed in trits and an index to the `subseed()` method -->

  ```js
  const seed = "PUETTSEITFEVEWCWBTSIZM9NKRGJEIMXTULBACGFRQK9IMGICLBKW9TTEVSDQMGWKBXPVCBMMCXWMNPDX";

  var subseed = Sign.subseed(Converter.trytesToTrits(seed), 0 /*index*/);
  ```

  :::info:
  シードを使用するコードはすべてクライアント側で実行されます. シードがご使用中のデバイスから離れることはありません.
  :::
  <!-- :::info: -->
  <!-- Any code that uses a seed is executed on the client side. Your seed never leaves your device. -->
  <!-- ::: -->

6. `key`メソッドに同じサブシードと異なるセキュリティレベルを渡して, 3つのセキュリティレベルそれぞれに対して1つの秘密鍵を導出します.
<!-- 6. Derive one private key for each of the three security levels by passing the same subseed and a different security level to the `key()` method -->

  ```js
  var privateKey1 = Sign.key(subseed, 1 /*security level*/);

  console.log('Private key length for security level 1: ' + Converter.tritsToTrytes(privateKey1).length);

  var privateKey2 = Sign.key(subseed, 2 /*security level*/);

  console.log('Private key length for security level 2: ' + Converter.tritsToTrytes(privateKey2).length);

  var privateKey3 = Sign.key(subseed, 3 /*security level*/);

  console.log('Private key length for security level 3: ' + Converter.tritsToTrytes(privateKey3).length);
  ```

  ファイルを実行すると, 各秘密鍵の長さがトライトで表示されます.
  <!-- When you execute the file, you should see the length of each private key in trytes: -->

  ```console
  Private key length for security level 1: 2187

  Private key length for security level 2: 4374

  Private key length for security level 3: 6561
  ```

  :::info:
  セキュリティレベルの詳細については[こちら](../references/security-levels.md).
  :::
  <!-- :::info: -->
  <!-- [Find out more about security levels](../references/security-levels.md). -->
  <!-- ::: -->

7. それぞれの秘密鍵を`digest`メソッドに渡して, 秘密鍵ごとにキーダイジェストを導出します.
<!-- 7. Derive the key digests for each private key by passing each one to the `digests()` method -->

  ```js
  var privateKey1Digests = Sign.digests(privateKey1);

  console.log(`Total key digests for security level 1: ` + Converter.tritsToTrytes(privateKey1Digests).length/81);

  var privateKey2Digests = Sign.digests(privateKey2);

  console.log(`Total key digests for security level 2: ` + Converter.tritsToTrytes(privateKey2Digests).length/81);

  var privateKey3Digests = Sign.digests(privateKey3);

  console.log(`Total key digests for security level 3: ` + Converter.tritsToTrytes(privateKey3Digests).length/81);
  ```

  ファイルを実行すると, 各秘密鍵のキーダイジェスト量がわかります.
  <!-- When you execute the file, you should see the amount of key digests for each private key: -->

  ```console
  Total key digests for security level 1: 1

  Total key digests for security level 2: 2

  Total key digests for security level 3: 3
  ```

  :::info:
  キーダイジェストの詳細については[こちら](../concepts/addresses-and-signatures.md).
  :::
  :::info:
  [Find out more about key digests](../concepts/addresses-and-signatures.md).
  :::

8. ダイジェストを`address`メソッドに渡して, 各秘密鍵のアドレスを導出します.
<!-- 8. Derive an address for each private key by passing the digests to the `address()` method -->

  ```js
  var privateKey1Address = Sign.address(privateKey1Digests);

  console.log('Address with security level 1: ' + Converter.tritsToTrytes(privateKey1Address));

  var privateKey2Address = Sign.address(privateKey2Digests);

  console.log('Address with security level 2: ' + Converter.tritsToTrytes(privateKey2Address));

  var privateKey3Address = Sign.address(privateKey3Digests);

  console.log('Address with security level 3: ' + Converter.tritsToTrytes(privateKey3Address));
  ```

  ファイルを実行すると, 各セキュリティレベルのアドレスが表示されます.
  <!-- When you execute the file, you should see the addresses for each security level: -->

  ```console
  Address with security level 1: ZWENNY9JOIQRJIRHV9PCQMCHKBXVZTTKMVRSZSKQNQCQCTZMTMUPEWE9DPCVBVZOVGFFI9JYLTIFXGJAX
  Address with security level 2: ECMHBSFPVUWHSUXZBXTWSKNMBGNTW9GAFVJUUSSJYFBOKHNFJBPEKJNMQMCSAIBXVUJNQKUBFUXPEIY9B
  Address with security level 3: LJGSYD9N9JEAQ9AVN9BJCAOW9LFVZGFHOXFVFVLQEBKVZFGBIDJJIRK9FBJUKRS9VMUXTCXBRIOOEMQJ9
  ```

9. 同じアドレスがIOTAコアライブラリから返されることを確認するために, 以下を実行します.
<!-- 9. To check that the same addresses would be returned from the IOTA core library, do the following: -->

  ```js
  console.log(Iota.generateAddress(seed, 0 /*index*/, 1 /*security level*/));
  console.log(Iota.generateAddress(seed, 0 /*index*/, 2 /*security level*/));
  console.log(Iota.generateAddress(seed, 0 /*index*/, 3 /*security level*/));
  ```

  ステップ8からのアドレスと同じアドレスが標準出力に表示されます.
  <!-- You should see the same addresses in the output as those from step 8. -->

:::success:おめでとうございます:tada:
IOTAコアライブラリの内部で, アドレスは特定のインデックスとセキュリティレベルを持つ秘密鍵から派生していることを証明しました.
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've proven that, under the hood of the IOTA core library, addresses are derived from private keys with a certain index and security level. -->
<!-- ::: -->

## コードを走らせる
<!-- ## Run the code -->

このガイドのサンプルコードを実行してWebブラウザに結果を表示するには, 緑色のボタンをクリックしてください.
<!-- Click the green button to run the sample code in this guide and see the results in the web browser. -->

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Derive-addresses-from-private-keys?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
