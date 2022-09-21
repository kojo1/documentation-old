

# 入門



wolfSSHをダウンロードしてビルドした後、ライブラリの使用を示すための自動化されたテストとサンプルプログラムがあります。



## テスト




### wolfSSHユニットテスト



wolfSSHユニットテストは、APIを検証するために使用されます。陽性と陰性の両方のテストケースが実行されます。このテストは手動で実行でき、さらにMAKEおよびMAKE Checkコマンドなどの他の自動化されたプロセスの一部として実行されます。


すべての例とテストは、テストツールが証明書とキーを見つけることができるように、wolfSSHホームディレクトリから実行する必要があります。


ユニットテストを手動で実行するには：

```
$ ./tests/unit.test
```

また

```
$ make check (when using autoconf)
```




### テストメモ



リポジトリをクローニングした後、テストのプライベートキーを必ず読んでください - ユーザーのみで、SSH_Clientはそれを行うように指示します。

```
$ chmod 0600 ./keys/gretel-key-rsa.pem ./keys/hansel-key-rsa.pem \
             ./keys/gretel-key-ecc.pem ./keys/hansel-key-ecc.pem
```



EcheCoserverの例に対する認証は、パスワードまたは公開鍵で実行できます。パスワードを使用するには、コマンドラインを使用します。

```
$ ssh_client -p 22222 USER@localhost
```



ユーザーとパスワードのペアがある場所：

```
jill:upthehill
jack:fetchapail
```



公開鍵認証を使用するには、コマンドラインを使用します。

```
$ ssh_client -i ./keys/USER-key-TYPE.pem -p 22222 USER@localhost
```



_user_はグレーテルまたはハンセルであり、タイプはRSAまたはECCです。


Echoserverには、Wsuserauthコールバック関数にいくつかの偽のアカウントがあります。(Jack、Jill、Hansel、およびGretel)シェルサポートが有効になっている場合、それらの偽のアカウントは機能しません。システムのPassWDファイルには存在しません。ユーザーは認証されますが、システムに存在しないため、サーバーは誤ります。Echoserverのパスワードまたは公開鍵リストに独自のユーザー名を追加できます。そのアカウントは、ユーザーがeChoServerを実行しているユーザーの特権を使用して、エコーザーによって開始されたシェルにログインされます。



## 例




### wolfSSH Echoサーバー



エコーザーはウルフスシュの主力です。もともとは、缶詰アカウントのいずれかを認証することができ、入力された文字を繰り返すことができました。シェルサポートを有効にすると、後のセクションを参照してください。ユーザーシェルを生成できます。マシン上の実際のユーザー名と、資格情報を検証するために更新されたユーザー認証コールバック関数が必要です。エコーザーは、SCPおよびSFTP接続を処理することもできます。ターミナルランから：



```
    $ ./examples/echoserver/echoserver -f
```



オプション`-f`は、エコーのみのモードを有効にします。別のターミナルランから：



```
    $ ssh_client jill@localhost -p 22222
```



パスワードを求められたら、「Upthehill」を入力します。サーバーは、クライアントに缶詰を送信します。



```
    wolfSSH Example Echo Server
```



クライアントに入力された文字は、サーバーによって画面にエコーされます。文字が2回エコーされた場合、クライアントはローカルエコーを有効にします。


Echoサーバーは適切な端末ではないため、CR/LFの翻訳は予想どおりに機能しません。


次のコントロール文字は、エコーザーでの特別なアクションをトリガーします。



- CTRL-C：接続を終了します。


- CTRL-E：セッション統計を印刷します。


- CTRL-F：新しいキー交換をトリガーします。



エコーザーツールは、次のコマンドラインオプションを受け入れます。

```
    -1             exit after a single (one) connection
    -e             expect ECC public key from client
    -E             use ECC private key
    -f             echo input
    -p <num>       port to accept on, default 22222
    -N             use non-blocking sockets
    -d <string>    set the home directory for SFTP connections
    -j <file>      load in a public key to accept from peer
```




### wolfSSHクライアント



クライアントは、SSHサーバーへの接続を確立します。最も単純なモードでは、文字列「Hello、wolfSSH！」を送信します。サーバーに、応答を印刷してから終了します。擬似端末オプションを使用すると、クライアントは実際の臨界になります。


クライアントツールは、次のコマンドラインオプションを受け入れます。

```
    -h <host>      host to connect to, default 127.0.0.1
    -p <num>       port to connect on, default 22222
    -u <username>  username to authenticate as (REQUIRED)
    -P <password>  password for username, prompted if omitted
    -e             use sample ecc key for user
    -i <filename>  filename for the user's private key
    -j <filename>  filename for the user's public key
    -x             exit after successful connection without doing
                   read/write
    -N             use non-blocking sockets
    -t             use psuedo terminal
    -c <command>   executes remote command and pipe stdin/stdout
    -a             Attempt to use SSH-AGENT
```




### wolfSSH Portfwd



PORTFWDツールは、SSHサーバーへの接続を確立し、ローカルポート転送のリスナーを設定するか、リモートポート転送のリスナーを要求します。接続の後、ツールは終了します。


PORTFWDツールは、次のコマンドラインオプションを受け入れます。

```
    -h <host>      host to connect to, default 127.0.0.1
    -p <num>       port to connect on, default 22222
    -u <username>  username to authenticate as (REQUIRED)
    -P <password>  password for username, prompted if omitted
    -F <host>      host to forward from, default 0.0.0.0
    -f <num>       host port to forward from (REQUIRED)
    -T <host>      host to forward to, default to host
    -t <num>       port to forward to (REQUIRED)
```




### wolfssh scpclient



SCPCLIENTであるWOLFSCPは、SSHサーバーとCopitESTHEがローカルマシンから指定されたファイルへの接続を確立します。


SCPClientツールは、次のコマンドラインオプションを受け入れます。

```
    -H <host>      host to connect to, default 127.0.0.1
    -p <num>       port to connect on, default 22222
    -u <username>  username to authenticate as (REQUIRED)
    -P <password>  password for username, prompted if omitted
    -L <from>:<to> copy from local to server
    -S <from>:<to> copy from server to local
```




# wolfssh sftpclient



SFTPCLIENT、WolfSFTPは、SSHサーバーへの接続を確立し、ディレクトリナビゲーション、ファイルの取得と配置、作成と削除などを発表します。


SFTPClientツールは、次のコマンドラインオプションを受け入れます。

```
    -h <host>      host to connect to, default 127.0.0.1
    -p <num>       port to connect on, default 22222
    -u <username>  username to authenticate as (REQUIRED)
    -P <password>  password for username, prompted if omitted
    -d <path>      set the default local path
    -N             use non blocking sockets
    -e             use ECC user authentication
    -l <filename>  local filename
    -r <filename>  remote filename
    -g             put local filename as remote filename
    -G             get remote filename as local filename
```




### wolfSSHサーバー



このツールは場所ホルダーです。





## SCP



wolfSSHには、SCPのサーバー側のサポートが含まれています。これには、「サーバー」から「サーバーへ」のファイルをコピーすることと、「サーバー」からファイルをコピーすることが含まれます。Bothsingleファイルと再帰ディレクトリのコピーは、defaultsendでサポートされ、コールバックを受信します。


wolfSSHをSCPサポートでコンパイルするには、`--enable-scp`ビルドオプションを使用して定義`WOLFSSL_SCP`を使用します。

```
    $ ./configure --enable-scp
    $ make
```





wolfSSLのサンプルサーバーは、単一のSCPリクエストを受け入れるように設定されており、wolfSSHライブラリをコンパイルするときにデフォルトでコンパイルされています。Exampleサーバーを起動するには、実行してください。

```
$ ./examples/server/server
```

標準のSCPコマンドは、クライアント側で使用できます。以下は、使用しているSSHクライアントを表す`scp`が表れています。


デフォルトの例を使用して、単一のファイルをサーバーにコピーするには、「ジル」を使用して：

```
$ scp -p 22222 <local_file> jill@127.0.0.1：<Remote_Path>
```

同じ単一のファイルをサーバーにコピーしますが、タイムスタンプとインバーボースモードを使用します。

```
$ scp -v -p -p 22222 <local_file> jill@127.0.0.1：<remote_path>
```

ディレクトリをサーバーに再帰的にコピーするには：

```
$ scp -p 22222 -r <local_dir> jill@127.0.0.1：<Remite_Dir>
```

サーバーからローカルクライアントに単一のファイルをコピーするには：

```
$ scp -p 22222 jill@127.0.0.1：<Remite_File> <Local_Path>
```

サーバーからローカルクライアントにディレクトリを再帰的にコピーするには：

```
$ scp -p 22222 -r jill@127.0.0.1：<Remite_Dir> <Local_Path>

```

## シェルサポート



wolfSSHのサンプルEchoserverは、ログインしようとするユーザー向けのシェルをフォークできるようになりました。これは現在、LinuxとMacOでのみテストされています。ファイルechoserver.cを変更するには、ユーザー認証コールバックにユーザーの資格情報があるか、ユーザー認証コールバックを変更して提供されたパスワードを確認する必要があります。


wolfSSHをシェルサポートでコンパイルするには、-Enable-Shellビルドオプションを使用するか、wolfssh_shellを定義します。

```
$ ./configure --enable-shell
$ make
```



デフォルトでは、エコーザーはシェルを起動しようとします。エコーテスト動作を使用するには、エコーザーにコマンドラインオプション-Fを与えます。

```
$ ./examples/echoserver/echoserver -f
```
