# シード作成
<!-- # Create a seed -->

**[シード](../introduction/what-is-a-seed.md)はすべてのアドレスにアクセスできるユニークなパスワードです。シードには数字9と大文字のA〜Zのみを含めることができます。**
<!-- **A [seed](../introduction/what-is-a-seed.md) is your unique password that gives you access to all your addresses. Seeds can include only the number 9 and the uppercase letters A-Z.** -->

シードはアドレスを所有していることを証明し、ノードがトランザクションを検証してトランザクションを台帳に追加することを可能にします。
<!-- Your seed proves that you own an address and allows nodes to validate your transactions and append them to their ledgers. -->

シードは安全に保ちシードをバックアップしなければなりません。シードを失うと回復できません。
<!-- You must keep your seed safe and back it up. If you lose your seed, you can't recover it. -->

:::info:
[トリニティウォレットでシードを作成する](root://trinity/0.1/how-to-guides/create-an-account.md)こともできます。
:::
<!-- :::info: -->
<!-- You can also [create a seed in the Trinity wallet](root://trinity/0.1/how-to-guides/create-an-account.md). -->
<!-- ::: -->

## Linuxオペレーティングシステムでシードを作成する。
<!-- ## Create a seed on a Linux operating system -->

1. コマンドプロンプトで次の操作を行います。
<!-- 1. Do the following in a command prompt: -->

  ```bash
  cat /dev/urandom |tr -dc A-Z9|head -c${1:-81}
  ```

2. 81文字の出力をコピーしてどこかにペーストします。後でシードが必要になります。今すぐシードをバックアップするのは良い考えです。
<!-- 2. Copy and paste the 81 character output somewhere. You'll need the seed later. It's a good idea to back up your seed now. -->

## Macオペレーティングシステムでシードを作成する。
<!-- ## Create a seed on a Mac operating system -->

1. コマンドプロンプトで次の操作を行います。
<!-- 1. Do the following in a command prompt: -->

  ```bash
  cat /dev/urandom |LC_ALL=C tr -dc 'A-Z9' | fold -w 81 | head -n 1
  ```

2. 81文字の出力をコピーしてどこかにペーストします。後でシードが必要になります。今すぐシードをバックアップするのは良い考えです。
<!-- 2. Copy and paste the 81 character output somewhere. You'll need the seed later. It's a good idea to back up your seed now. -->

## Windowsオペレーティングシステムでシードを作成する。
<!-- ## Create a seed on a Windows operating system -->

1. [KeePassインストーラー](https://keepass.info/)をダウンロードします。
<!-- 1. [Download the KeePass installer](https://keepass.info/) -->

  KeePassは、高度に暗号化されたデータベースにパスワードを保存するパスワードマネージャです。データベースは、1つのマスターパスワードまたはキーファイルでロック解除できます。
  <!-- KeePass is a password manager that stores passwords in highly-encrypted databases, which can be unlocked with one master password or key file. -->

2. インストーラを開き、画面上の指示に従います。
<!-- 2. Open the installer and follow the on-screen instructions -->

3. Keepassを開き、**New**をクリックします。
<!-- 3. Open KeePass and click **New** -->

  ![A new KeePass database](../images/keypass-new.png)

4. 指示に従い、KeePassファイルをコンピューターに保存したら、空白の部分を右クリックして**Add entry**をクリックします。
<!-- 4. After you've followed the instructions and saved the KeePass file on your computer, right click the empty space and click **Add entry** -->

  ![A new KeePass entry](../images/keepass-add-entry.png)

5. **Generate a password**をクリックします。
<!-- 5. Click **Generate a password** -->

  ![Keepass password generator](../images/keypass-password-generator.png)

6. 以下のオプションのみを選択して、**OK**をクリックしてください。
<!-- 6. Select only the following options and click **OK**: -->

  * Length of generated password: 81
  * Upper-case (A, B, C, ...)
  * Also include the following characters: 9

7. **OK**をクリックしてシードを保存します。
<!-- 7. Click **OK** to save your seed -->
