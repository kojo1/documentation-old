

# wolfSSHのビルド



wolfSSHは、移植性を念頭に置いて書かれており、一般にほとんどのシステムで簡単にビルドできるはずです。ビルドに苦労している場合は、サポートフォーラム、https：//www.wolfssl.com/forumsを通じてサポートを求めることをためらわないか、support@wolfssl.comまで直接お問い合わせください。


このセクションでは、 *nix-like環境とWindows環境でwolfSSHをビルドする方法について説明し、非標準環境にビルドするためのガイダンスを提供します。セクション3には、開始ガイドと例があります。


AutoCONF/Automakeシステムを使用してビルドする場合、wolfSSHは単一のMakeFileを使用してライブラリのすべての部品と例をビルドします。これは、MakeFilesを再帰的に使用するよりも簡単で高速です。



## ソースコードを取得



最新の、最新バージョンはGitHub Webサイトからダウンロードできます：https：//github.com/wolfssl/wolfssh


[zipをダウンロード]ボタンをクリックするか、端末で次のコマンドを使用します。

```
$ git clone https://github.com/wolfSSL/wolfssh.git
```


## wolfSSHの依存関係



wolfSSHはWolfCryptに依存しているため、wolfSSLの構成が必要です。wolfsslはここからダウンロードできます：https：//github.com/wolfssl/wolfssl。wolfSSHに必要なwolfSSLの最も単純な構成は、次のコマンドを使用してwolfSSLのルートディレクトリからビルドできるデフォルトのビルドです。



```
$ ./autogen.sh (only if you cloned from GitHub)
$ ./configure --enable-ssh
$ make check
$ sudo make install
```

wolfSSHのキー生成関数を使用するには、wolfSSLをkeygen：` --enable-keygen`で構成する必要があります。


wolfSSLコードの大部分が望ましくない場合、wolfSSLはCryptoのみのオプションで構成できます：`--enable-cryptonly`。



## *nixの上でビルド



Linux、 *BSD、OS X、Solaris、またはその他の *nixのような環境上にビルドする場合、AutoCONFシステムを使用します。wolfSSHをビルドするには、次のコマンドを実行します。

```
$ ./autogen.sh (only if you cloned from GitHub)
$ ./configure
$ make
$ make install
```

Configureコマンドにビルドオプションを追加できます。利用可能な構成オプションのリストとそれらの目的の実行：実行されます。

```
$ ./configure --help
```

wolfSSH ライブラリーをビルドするには：

```
$ make
```

wolfSSHが正しくビルドされていることを確認するには、すべてのテストが渡されたかどうかを確認してください。



```
$ make check
```



wolfSSH ライブラリーをインストールするには：

```
$ make install
```



インストールするにはスーパーユーザーの特権が必要になる場合があります。その場合、sudoでインストールを実行します。

```
$ sudo make install
```

wolfssh/src/にあるwolfSSHライブラリのみをビルドしたい場合は、Theadditionalアイテム(例とテスト)ではなく、wolfSSHのルートディレクトリーから次のコマンドを実行できます。

```
$ make src/libwolfssh.la
```


## Windowsの上のビルド



Visual Studioプロジェクトファイルは、https：//github.com/wolfssl/wolfssh/blob/master/ide/winvs/wolfssh.slnにあります。


ソリューションファイル「wolfssh.sln」は、wolfSSHのビルドとその例とテストプログラムを促進します。このソリューションは、静的および動的な32ビットまたは64ビットライブラリのデバッグとリリースの両方のビルドを提供します。ファイルuser_settings.hをwolfSSLビルドで使用して構成する必要があります。


このプロジェクトは、wolfSSHとWolfsslのソースディレクトリが並んでインストールされており、名前にバージョン番号がないことを前提としています。



```
Projects\
wolfssh\
wolfssl\
```



ファイル`wolfssh\ide\winvs\user_settings.h`には、適切な設定を使用してwolfSSLを構成する設定が含まれています。このファイルは、ディレクトリ`wolfssh\ide\winvs`から`wolfssl\IDE\WIN`からコピーする必要があります。コピーを変更する場合は、両方のコピーを変更する必要があります。オプション`WOLFCRYPT_ONLY` wolfSSLファイルのビルドを無効にし、WolfCryptアルゴリズムのみをビルドします。toalsoはwolfsslを維持し、そのオプションを削除します。



### Windowsにビルドするためのユーザーマクロ



ソリューションは、ユーザーマクロを使用して、wolfSSLライブラリとヘッダーの位置を示すことです。すべてのパスは、wolfSSL64ソリューションのデフォルトのビルド宛先に設定されます。ユーザーMacro WolfCryptDirは、ライブラリを見つけるためのベースパスとして使用されます。最初は`..\..\..\..\wolfssl`に設定されています。たとえば、APIテストプロジェクトの追加のディレクトリ値は`$(wolfCryptDir)`に設定されています。


wolfcryptdirパスは、プロジェクトファイルに関連している必要があります。これはすべて1つのディレクトリです



```
wolfssh/wolfssh.vcxproj
unit-test/unit-test.vcxproj
```

他のユーザーマクロは、さまざまなビルドのwolfSSLライブラリが見つかるディレクトリです。したがって、ユーザーmacro 'wolfcryptdllrelease64'は最初に次のように設定されています。

```
$(wolfCryptDir)\x64\DLL Release
```

この値は、Echoserverの64ビットDLLリリースビルドのデバッグ環境で使用されます。

```
PATH=$(wolfCryptDllRelease64);%PATH%
```

エコーザーをデバッガーから実行すると、そのディレクトリにwolfSSL DLLが見つかります。



## 標準以外の環境にビルド



公式にはサポートされていませんが、特に組み込みおよびクロスコンパイルされたシステムを使用して、非標準環境でwolfSSHをビルドしたいユーザーを支援しようとしています。以下は、これを始めることに関するいくつかのメモです。



1. ソースファイルとヘッダーファイルは、wolfSSHダウンロードパッケージと同じディレクトリ構造にとどまる必要があります。


2. 一部のビルドシステムでは、wolfSSHヘッダーファイルがどこにあるかを明示的に知る必要があるため、それを指定する必要がある場合があります。それらは<wolfSSH_Root>/wolfsshディレクトリにあります。通常、ヘッダーの問題を解決するためのパスを含めるパスに<wolfSSH_Root>ディレクトリを追加できます。


3. wolfSSHは、構成プロセスがBig Endianを検出しない限り、デフォルトで小さなエンディアンシステムになります。標準以外の環境でビルドするユーザーはConfigureプロセスを使用していないため、BIG_ENDIAN_ORDERをBIG Endianシステムを使用する場合は定義する必要があります。


4. ライブラリをビルドして、問題が発生した場合はお知らせください。サポートが必要な場合は、support@wolfssl.comまでお問い合わせください。




## クロスコンパイル

埋め込まれたプラットフォーム上の多くのユーザーは、環境のためにコンパイルをクロスします。ライブラリをコンパイルする最も簡単な方法は、Configureシステムを使用することです。MakeFileを生成し、wolfSSHをビルドするために使用できます。


クロスコンパイルの場合、次のような構成するホストを指定する必要があります。

```
$ ./configure --host=arm-linux
```

また、使用するコンパイラ、リンカーなどを指定する必要がある場合があります。

```
$ ./configure --host=arm-linux CC=arm-linux-gcc AR=arm-
linux-ar
RANLIB=arm-linux
```

クロスコンピレーション用にwolfSSHを正しく構成した後、ライブラリをビルドおよび設置するための標準的なAutoCONFプラクティスに従うことができるはずです。



```
$ make
$ sudo make install
```

WolfsSLをクロスコンパイルするための追加のヒントやフィードバックがある場合は、info@wolfssl.comでお知らせください。



## カスタムディレクトリにインストールします



WolfsSLのカスタムインストールディレクトリをセットアップするには、以下を使用してください。

```
$ ./configure --prefix=~/wolfSSL
$ make
$ make install
```

これにより、ライブラリは〜/wolfssl/libに配置され、内集に〜/wolfssl/includeが含まれます。wolfSSHのカスタムインストールディレクトリを設定し、カスタムWolfsSLライブラリを指定し、ディレクトリを含めるには、以下を使用してください。

```
$ ./configure  --prefix=~/wolfssh  --libdir=~/wolfssl/lib  --includedir=~/wolfssl/include
$ make
$ make install
```

上のパスが実際の場所に一致することを確認してください。

