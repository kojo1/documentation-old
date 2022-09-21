

# 序章



このマニュアルは、Wolfssh埋め込みライブラリの技術ガイドとして書かれています。Wolfsshのビルドと開始方法を説明し、ビルドオプション、機能、サポートなどの概要を提供します。


Wolfsshは、Cで記述されたSSH(Secure Shell)サーバーの実装であり、wolfSSLからも利用できるWolfCryptライブラリを使用しています。さらに、Wolfsshは、マルチプラットフォームを使用するためにゼロからビルドされています。この実装は、SSH V2仕様に基づいています。



## プロトコルの概要



SSHは、2つのピア間で多重化されたデータストリームを提供する層状のプロトコルセットです。通常、サーバー上のシェルへの接続を固定するために使用されます。ただし、2つのマシン間でファイルを安全にコピーしたり、Xディスプレイプロトコルをトンネルしたりするためにも一般的に使用されます。



## なぜwolfsshを選ぶのですか？



Wolfsshライブラリは、ANSI Cで記述され、埋め込み、RTO、およびリソース制約の環境をターゲットにした軽量のSSHV2サーバーライブラリです。これは、主にそのサイズ、速度、機能セットのためです。ロイヤリティフリーの価格設定と優れたクロスプラットフォームサポートのために、標準の操作環境でも一般的に使用されています。Wolfsshは、業界標準のSSH V2をサポートし、Poly1305、Chacha20、NTRU、SHA-3などのプログレッシブ暗号を提供しています。WolfsshはWolfCrypt Libraryを搭載しています。WolfCrypt Cryptography Libraryのバージョンは、FIPS 140-2の検証されています(証明書＃2425)。詳細については、WolfCrypt FIPS FAQにアクセスするか、fips@wolfssl.comにお問い合わせください。



### 機能






- SSH V2.0(サーバー)




- 33kbの最小フットプリントサイズ




- 1.4〜2KBの間のランタイムメモリの使用は、構成可能な受信バッファーを含まない




- 複数のハッシュ機能：SHA-1、SHA-2(SHA-256、SHA-384、SHA-512)、Blake2B、Poly




- ブロック、ストリーム、および認証された暗号：AES(CBC、CTR、GCM、CCM)、Camellia、Chacha




- 公開キーのオプション：RSA、DH、EDH、NTRU




- ECCサポート(曲線付きECDHおよびECDSA：nistp256、nistp384、nistp




- Curve25519およびEd




- クライアント認証サポート(RSAキー、パスワード)




- 単純なAPI




- PEMおよびDER証明書サポート




- ハードウェア暗号化サポート：Intel AES-NIサポート、Intel AVX1/2、RDRAND




- Rdseed、Cavium Nitroxサポート、STM32F2/F4ハードウェア暗号サポート




- フリースケールCAU/MMCAU/SEC、MicroChip PIC32MZ


