# 最初のゼロトークントランザクションを送信する（トリニティ）
<!-- # Send your first zero-value transaction (Trinity) -->

**トリニティは、IOTAネットワークでデータとIOTAトークンを転送できるユーザーインターフェースを備えたモバイルおよびデスクトップアプリケーションです。 IOTAトークンを含まないランダムシードを使用して、ゼロトークントランザクションを送信できます。これらのトランザクションは、不変なメッセージをタングルに送信して保存するのに役立ちます。**
<!-- **Trinity is a mobile and desktop application with a user interface that allows you to transfer data and IOTA tokens in an IOTA network. A zero-value transaction can be sent using a random seed that doesn't contain IOTA tokens. These transactions are useful for sending and storing immutable messages on the Tangle.** -->

トリニティでは次のことが可能です。
<!-- Trinity allows you to do the following: -->

* シードを保存し、シードにアクセスするための、パスワードで保護されたアカウントを作成します。
* 残高と取引履歴を見れます。
* トランザクションを送受信します。
<!-- * Create a password-protected account to store and access your seeds -->
<!-- * Read your balance and transaction history -->
<!-- * Send and receive transactions -->

![Trinity home page](../images/trinity-home.jpg)

---

1. [トリニティのダウンロードとインストール](https://trinity.iota.org/)
<!-- 1. [Download and install Trinity](https://trinity.iota.org/) -->

2. トリニティを開く。
<!-- 2. Open Trinity -->

3. トリニティでシードを作成するには、**はい、シードが必要です**をクリックします。既にシードを持っている場合、またはトリニティの外部で[シードを作成](../tutorials/create-a-seed.md)した場合は、**いいえ、持っています**をクリックします。
<!-- 3. To create a seed in Trinity, click **Yes, I need a seed**. If you already have a seed, or if you [created a seed outside of Trinity](../tutorials/create-a-seed.md), click **No, I have one**. -->

  ![Seed options](../images/trinity-seed.png)

4. シードのアカウント名を入力してください。
<!-- 4. Enter an account name for your seed -->

  ![Account name](../images/trinity-account-name.png)

5. シードを記録してログインパスワードを入力するオプションを選択します。
<!-- 5. Select an option to record your seed and enter a login password -->

6. ログインしたら、**アカウント** > **アカウント管理** > **アドレスの表示**に移動して、シードのアドレスのリストを表示します。
<!-- 6. After you've logged in, go to **Account** > **Account management** > **View addresses** to see a list of your seed's addresses. -->

  ![Account addresses](../images/trinity-view-addresses.png)

  :::info:
  新しいアドレスを取得するには、ホーム画面の**受信**をクリックしてください。
  :::
  <!-- :::info: -->
  <!-- To derive a new address, click **Receive** on the homepage. -->
  <!-- ::: -->

7. 自分のアドレスの1つをコピーして**送信**をクリックし、そのアドレスを**受信者アドレス**フィールドに貼り付けます。
<!-- 7. Copy one of your addresses, click **Send**, and paste the address into the RECIPIENT ADDRESS field -->

8. メッセージフィールドに'Hello world！'と入力してください。
<!-- 8. Enter 'Hello world!' in the MESSAGE field -->

  ![Hello world message](../images/trinity-hello-world.png)

9. 金額フィールドが0であることを確認して、**送金**をクリックしてください。
<!-- 9. Make sure that the AMOUNT field is 0 and click **Send** -->

トランザクションはトランザクション履歴に表示されます。トランザクションをクリックして詳細を表示します。
<!-- Your transaction will appear in your transaction history. Click the transaction to display its details. -->

![Transaction history](../images/trinity-receive-message.png)

:::success:おめでとうございます:tada:
公開されているIOTAネットワークでメッセージを自分に送信しました。友達にメッセージを送信できるように、友達のアドレスの1つを尋ねてみませんか。
:::
<!-- :::success:Congratulations :tada: -->
<!-- You've just sent yourself a message that's now public on the IOTA network. Why not ask your friends for one of their addresses so you can send them messages. -->
<!-- ::: -->
