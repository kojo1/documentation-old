

# wolfSSH SFTPのビルドと使用




## wolfSSH sftpをビルドします



WolfsslはすでにwolfSSHで使用されるようにビルドされていると想定されています。WolfsSLのビルドの指示を見るには、第2章をご覧ください。


SFTP使用をサポートしてwolfSSHをビルドするには、オートオトールでビルドされた場合に、Enable-SFTPを使用するか、AutoToolsなしでビルドする場合はMacro WOLFSSH_SFTPを定義します。この例は次のとおりです。

```
./configure --enable-sftp && make
```

デフォルトでは、get and putコマンドの読み取りと書き込みを処理するための内部バッファサイズが1024バイトに設定されます。この値は、アプリケーションがより少ないリソースを消費する必要がある場合、またはより大きなバッファが望ましい場合に上書きすることができます。デフォルトサイズをオーバーライドするには、コンパイル時間にマクロ`WOLFSSH_MAX_SFTP_RW`を定義します。設定の例は次のとおりです。



```
./configure --enable-sftp
C_EXTRA_FLAGS=’WOLFSSH_MAX_SFTP_RW=2048
```




## wolfSSH SFTPアプリの使用



SFTPサーバーとクライアントアプリケーションは、wolfSSHにバンドルされています。両方のアプリケーションは、SFTPサポートを使用してwolfSSHライブラリをビルドするときにAutoToolsによってビルドされます。サーバーアプリケーションはExamples/ Echoserver/にあり、Echoserverと呼ばれます。クライアントアプリケーションはWolfSFTP/ Client/にあり、WolfSFTPと呼ばれます。


着信SFTPクライアント接続を処理するサーバーを起動する例は次のとおりです。

```
./examples/echoserver/echoserver
```

コマンドがRoot wolfSSHディレクトリから実行されている場合。これにより、SSH接続とSFTP接続の両方を処理できるサーバーが起動します。


特定のユーザー名でクライアントを起動します：

```
$ ./wolfsftp/client/wolfsftp -u <username>
```

テストを実行するデフォルトの「ユーザー名：パスワード」は、「Jack：Fetchapail」または「Jill：Upthehill」のいずれかです。デフォルトのポートは22222です。


接続後に「ヘルプ」を入力することで、サポートされているコマンドの完全なリストを見ることができます。

```
wolfSSH sftp> help

Commands :
    cd  <string>                      change directory
    chmod <mode> <path>               change mode
    get <remote file> <local file>    pulls file(s) from server
    ls                                list current directory
    mkdir <dir name>                  creates new directory on server
    put <local file> <remote file>    push file(s) to server
    pwd                               list current path
    quit                              exit
    rename <old> <new>                renames remote file
    reget <remote file> <local file>  resume pulling file
    reput <remote file> <local file>  resume pushing file
    <crtl + c>                        interrupt get/put cmd
```



別のシステムに接続する例はあります

```
src/wolfssh$ ./examples/sftpclient/wolfsftp -p 22 -u user -h 192.168.1.111
```


