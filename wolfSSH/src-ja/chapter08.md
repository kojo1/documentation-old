

# ポート転送




## ポート転送を備えたwolfSSHのビルド



WolfsslはすでにwolfSSHで使用されるようにビルドされていると想定されています。WolfsSLのビルドの指示を見るには、第2章を見る。


オートオトールを使用してビルドの場合、ポート転送の使用をサポートしてwolfSSHをビルドするには、自動車なしでビルドの場合はMacro `WOLFSSH_FWD`を定義します。この例はそうです

```
./configure --enable-fwd && make
```


## wolfSSHポート転送の例アプリを使用します



PORTFWDのサンプルツールは、「Direct-TCPIP」スタイルチャネルを作成します。これらの指示では、ポート転送が有効になっている状態で、OpenSSHのサーバーがバックグラウンドで実行されていることを想定しています。この例では、wolfSSLクライアントのポートをアプリケーションとしてサーバーに転送します。すべてのプログラムが異なる端子で同じマシンで実行されると想定しています。



```
src/wolfssl$ ./examples/server/server
src/wolfssh$ ./examples/portfwd/portfwd -p 22 -u <username> \
             -f 12345 -t 11111
src/wolfssl$ ./examples/client/client -p 12345
```



デフォルトでは、wolfSSL Serverはポート11111に耳を傾けます。クライアントは、ポート12345に接続しようとするように設定されています。ポートFWDはユーザー「ユーザー名」としてログインし、ポート12345でリスナーを開き、ポート11111でサーバーに接続します。クライアントとサーバーの間を行き来します。「こんにちは、wolfSSL！」


PORTFWDのソースは、wolfSSHのポート転送サポートのセットアップと使用方法に関する例を示しています。


エコーザーは、ローカルおよびリモートポートの転送を処理します。次のコマンドラインのいずれかを使用して、SSHツールに接続します。どこからでもSSHコマンドラインのいずれかを実行できます。



```
src/wolfssl$ ./examples/server/server
src/wolfssh$ ./examples/echoserver/echoserver
anywhere 1$ ssh -p 22222 -L 12345:localhost:11111 jill@localhost
anywhere 2$ ssh -p 22222 -R 12345:localhost:11111 jill@localhost
src/wolfssl$ ./examples/client/client -p 12345
```


これにより、前の例のように、wolfSSLクライアントとサーバー間のポート転送が可能になります。
