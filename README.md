# toorPIA Remote Toolkit
toolkit for utilizing toorpia remotely

## How to Install

```bash
$ git clone https://github.com/toorpia/remote_toolkit.git
$ cd remote_toolkit
$ pip install .
```

## How to Use

[`examples/simple.ipynb`](https://github.com/toorpia/remote_toolkit/blob/main/examples/simple.ipynb) を参考にしてください。


## Requirement

本モジュールを利用するユーザは、toorPIA分析パッケージが稼働するサーバーへパスワードなしで(公開鍵暗号方式で) ssh 接続できる権限を有し、かつ、サーバーにおいてdocker groupに属していることが必要となります。

なお、パスワードなしでssh接続を行うには、一般には公開鍵認証方式を使用しますが、その設定手順は以下の通りです。

1. 鍵ペアの生成:
まず、クライアントマシンでSSH鍵ペア（公開鍵と秘密鍵）を生成します。これは、次のコマンドを実行して行います。

    ```
    $ ssh-keygen
    ```
    これにより、デフォルトでは `~/.ssh/id_rsa`（秘密鍵）と `~/.ssh/id_rsa.pub`（公開鍵）が生成されます。必要に応じて、鍵のタイプや保存場所を指定できます。

2. 公開鍵のアップロード:

    次に、生成された公開鍵を、接続先のサーバーにアップロードします。これには、`scp`コマンドや`rsync`コマンドなどを使用できます。例えば、以下のコマンドを使用して公開鍵をサーバーにコピーします。
    ```
    $ scp ~/.ssh/id_rsa.pub user@remote_host:/tmp/id_rsa.pub
    ```
    ここで、`user`はリモートホストのユーザー名、`remote_host`はリモートホストのアドレスです。

3. リモートホストでの公開鍵の設定:

    リモートホストで、アップロードした公開鍵を `~/.ssh/authorized_keys` ファイルに追加します。これにより、公開鍵認証が有効になります。次のコマンドを実行して、公開鍵を追加します。
    ```
    $ mkdir -p ~/.ssh
    $ cat /tmp/id_rsa.pub >> ~/.ssh/authorized_keys
    $ chmod 600 ~/.ssh/authorized_keys
    $ rm /tmp/id_rsa.pub
    ```

4. パスワード認証の無効化（オプション）:
    以下は、特に必要がないですが、念のため。

    リモートホストの `/etc/ssh/sshd_config` ファイルを編集して、パスワード認証を無効にすることができます。これにより、公開鍵認証のみが許可されます。
    ```
    PasswordAuthentication no
    ```
    ファイルを編集したら、SSHデーモンを再起動して変更を適用します。
    ```
    $ sudo service ssh restart
    ```
    これで、パスワードなしでSSH接続ができるようになります。ただし、秘密鍵を安全に保管し、不正アクセスがないように注意してください。
