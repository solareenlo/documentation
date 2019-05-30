# セキュリティレベル
<!-- # Security levels -->

**以下の表には, 秘密鍵とアドレスのペアで使用できるセキュリティレベルが表示されています. セキュリティレベルが高ければ高いほど, 送信されたアドレスの署名は大きくなり, より総当たり攻撃に対して安全になります.**
<!-- **This table displays the possible security levels of a private key and address pair. The greater the security level, the larger and more secure the signature of a spent address is against brute force attacks.** -->

セキュリティレベルが高いほど, トランザクションに署名するためにより多くの計算を行わなければならず, 署名を含めるにはバンドル内により多くのトランザクションが必要になることを意味します. その結果, 低電力デバイスはセキュリティレベル2を使用したいと思うかもしれませんが, 大規模企業はセキュリティレベル3を使用したいと思うかもしれません.
<!-- A greater security level means that more computations must be done to sign a transaction and that more transactions are needed in a bundle to contain the signature. As a result, low-powered devices may want to use security level 2, whereas a large-scale company may want to use security level 3. -->

| **セキュリティレベル** | **秘密鍵/署名の長さ** |
| :-------------- | :-------------------------- |
| 1              | 2,187（非推奨）|
| 2              | 4,374（トリニティで使用）|
| 3              | 6,561（最も安全）|

<!-- | **Security Level** | **Private key/signature length**                   | -->
<!-- | :-------------- | :-------------------------- | -->
<!-- | 1              | 2,187 (not recommended)| -->
<!-- | 2              | 4,374 (used by Trinity)         | -->
<!-- | 3              | 6,561 (most secure)           | -->
