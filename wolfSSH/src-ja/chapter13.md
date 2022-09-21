

# APIリファレンス



このセクションでは、wolfSSH Libraryのパブリックアプリケーションプログラムインターフェイスについて説明します。



## エラーコード








### WS_ERRORCODES(ENUM)







次のAPI応答コードは、次のように定義されています。wolfssh/wolfssh/error.hで、発生する可能性のあるさまざまなタイプのエラーを説明します。



- WS_SUCCESS(0)：関数の成功


- WS_FATAL_ERROR(-1)：一般的な関数障害


- ws_bad_argument(-2)：境界のない関数引数


- WS_MEMORY_E(-3)：メモリ割り当てエラー


- ws_buffer_e(-4)：入力/出力バッファサイズエラー


- WS_PARSE_E(-5)：一般的な解析エラー


- ws_not_compiled(-6)：compiled in in in feals


- ws_overflow_e(-7)：継続すればオーバーフローします


- ws_bad_usage(-8)：悪い例の使用法


- ws_socket_error_e(-9)：ソケットエラー


- ws_want_read(-10)：io callbackはブロックエラーを読み取ります


- ws_want_write(-11)：ioコールバックはブロックエラーを書き込む


- WS_RECV_OVERFLOW_E(-12)：バッファオーバーフローを受信しました


- ws_version_e(-13)：sshの間違ったバージョンを使用してピア


- WS_SEND_OOB_READ_E(-14)：境界のないバッファーを読み取ろうとした


- ws_input_case_e(-15)：プロセス入力状態が悪い、プログラミングエラー


- WS_BAD_FILETYPE_E(-16)：悪いFiletype


- ws_unimplemented_e(-17)：機能は実装されていません


- WS_RSA_E(-18)：RSAバッファーエラー


- WS_BAD_FILE_E(-19)：悪いファイル


- WS_INVALID_ALGO_ID(-20)：無効なアルゴリズムID


- WS_DECRYPT_E(-21)：エラーを解読します


- WS_ENCRYPT_E(-22)：暗号化エラー


- WS_VERIFY_MAC_E(-23)：MACエラーを確認します


- WS_CREATE_MAC_E(-24)：MACエラーを作成します


- ws_resource_e(-25)：新しいチャネルのリソースが不十分


- ws_invalid_chantype(-26)：無効なチャネルタイプ


- WS_INVALID_CHANID(-27)：ピアが無効なチャネルIDを要求しました


- ws_invalid_username(-28)：無効なユーザー名


- WS_CRYPTO_FAILED(-29)：暗号アクションが失敗しました


- WS_INVALID_STATE_E(-30)：無効な状態


- WC_EOF(-31)：ファイルの終了


- WS_INVALID_PRIME_CURVE(-32)：ECCの無効なプライムカーブ


- WS_ECC_E(-33)：ECDSAバッファーエラー


- WS_CHANOPEN_FAILED(-34)：ピア返品チャネルオープン失敗


- WS_REKEYING(-35)：ピアとの再キーイング


- WS_CHANNEL_CLOSED(-36)：チャネル閉じた




### ws_ioerrors(enum)







これらは、ライブラリがユーザーが提供するI/Oコールバックから受け取る予定のリターンコードです。それ以外の場合、ライブラリは、I/Oアクションから読み取られたバイト数を期待しています。



- WS_CBIO_ERR_GENERAL(-1)：一般的な予期しないエラー


- ws_cbio_err_want_read(-2)：ソケットの読み取りはブロックされ、もう一度電話をかけます


- ws_cbio_err_want_write(-2)：ソケット書き込みはブロックされ、もう一度電話をかけます


- WS_CBIO_ERR_CONN_RST(-3)：接続リセット


- WS_CBIO_ERR_ISR(-4)：割り込み


- WS_CBIO_ERR_CONN_CLOSE(-5)：接続閉じたまたはエピペ


- WS_CBIO_ERR_TIMEOUT(-6)：ソケットタイムアウト "




## 初期化 /シャットダウン








### wolfSSH_Init()







**Synopsis**


**Description**


使用するためにwolfSSHライブラリを初期化します。アプリケーションごとに1回、ライブラリへの他の呼び出しの前に呼び出される必要があります。


**Return Values**


WS_SUCCESSWS_CRYPTO_FAILED


**Parameters**


None


**参照**


wolfSSH_Cleanup()



```
#include <wolfssh/ssh.h>
int wolfSSH_Init(void);
```




### wolfSSH_Cleanup()







**Synopsis**


**Description**


完了したら、wolfSSHライブラリをクリーンアップします。アプリケーションの終了前に呼び出される必要があります。電話後、図書館にこれ以上電話をかけないでください。


**Return Values**


**WS_SUCCESS**


**WS_CRYPTO_FAILED**


**Parameters**


None


**参照**


wolfSSH_Init()



```
#include <wolfssh/ssh.h>
int wolfSSH_Cleanup(void);
```




## 出力機能のデバッグ








### wolfSSH_Debugging_ON()







**Synopsis**


**Description**


ランタイム中にデバッグロギングを有効にします。ビルド時にデバッグが無効になっている場合、何もしません。


**Return Values**


None


**Parameters**


None


**参照**


wolfSSH_Debugging_OFF()



```
#include <wolfssh/ssh.h>
void wolfSSH_Debugging_ON(void);
```




### wolfSSH_Debugging_OFF()







**Synopsis**


**Description**


ランタイム中にデバッグロギングを無効にします。ビルド時にデバッグが無効になっている場合、何もしません。


**Return Values**


None


**Parameters**


None


**参照**


wolfSSH_Debugging_ON()



```
#include <wolfssh/ssh.h>
void wolfSSH_Debugging_OFF(void);
```




## コンテキスト関数








### wolfSSH_CTX_new()







**Synopsis**


**Description**


wolfSSHコンテキストオブジェクトを作成します。このオブジェクトは構成し、wolfSSHセッションオブジェクトの工場として使用できます。


**Return Values**


** wolfssh_ctx ***  - 割り当てられたwolfssh_ctxオブジェクトまたはnullへのポインターを返します


**Parameters**


** SIDE **  - クライアントサイド(実装なし)またはサーバーサイドを示します**ヒープ**  - メモリ割り当てに使用するヒープへのポインター


**参照**


wolfSSH_wolfSSH_CTX_free()



```
#include <wolfssh/ssh.h>
WOLFSSH_CTX* wolfSSH_CTX_new(byte side , void* heap );
```




### wolfSSH_CTX_free()







**Synopsis**


**Description**


wolfsshコンテキストオブジェクトを扱います。


**Return Values**


None


**Parameters**


** CTX **  -  wolfSSHセッションの初期化に使用されるwolfSSHコンテキスト


**参照**


wolfSSH_wolfSSH_CTX_new()



```
#include <wolfssh/ssh.h>
void wolfSSH_CTX_free(WOLFSSH_CTX* ctx );
```




### wolfSSH_CTX_SetBanner()





**Synopsis**


**Description**


ユーザーが表示できるバナーメッセージを設定します。


**Return Values**


WS_BAD_ARGUMENTWS_SUCCESS


**Parameters**


** SSH- ** wolfSSHセッションへのポインター** NewBanner **  - バナーメッセージテキスト。



```
#include <wolfssh/ssh.h>
int wolfSSH_CTX_SetBanner(WOLFSSH_CTX* ctx , const char*
newBanner );
```




### wolfSSH_CTX_UsePrivateKey_buffer()







**Synopsis**


**Description**


この関数は、秘密キーバッファをSSHコンテキストにロードします。ファイルの代わりにバッファーを入力として呼び出されます。バッファは、** insz **の** in **引数によって提供されます。引数**フォーマット**バッファのタイプを指定します：** wolfssh_format_asn1 **または** wolfssl_format_pem **(現時点では不明瞭)。


**Return Values**


** ws_successws_bad_argument **  - 少なくとも1つのパラメーターは無効です** ws_bad_filetype_e **  - 間違った形式** ws_unimplemented_e **  - 実装されていない** ws_memory_e **RSAキーをデコードできません** WS_BAD_FILE_E **  - バッファーを解析できません


**Parameters**


**ctx**  -  wolfsshコンテキストへのポインター** in **  - ロードする秘密鍵を含むバッファ** insz **  - 入力バッファーのサイズ**フォーマット**  - 入力バッファー


**参照**


wolfSSH_UseCert_buffer()wolfSSH_UseCaCert_buffer()



```
#include <wolfssh/ssh.h>
int wolfSSH_CTX_UsePrivateKey_buffer(WOLFSSH_CTX* ctx ,
const byte* in , word32 inSz , int format );
```




## SSHセッション関数








### wolfSSH_new()







**Synopsis**


**Description**


wolfSSHセッションオブジェクトを作成します。提供されたwolfSSHコンテキストで初期化されます。


**Return Values**


** wolfssh ***  - 割り当てられたwolfsshオブジェクトまたはnullにポインターを返します


**Parameters**


** CTX **  -  wolfSSHセッションの初期化に使用されるwolfSSHコンテキスト


**参照**


wolfSSH_free()



```
#include <wolfssh/ssh.h>
WOLFSSH* wolfSSH_new(WOLFSSH_CTX* ctx );
```




### wolfSSH_free()







**Synopsis**


**Description**


wolfSSHセッションオブジェクトを扱います。


**Return Values**


None


**Parameters**


** ssh **  - 契約するセッション


**参照**


wolfSSH_new()



```
#include <wolfssh/ssh.h>
void wolfSSH_free(WOLFSSH* ssh );
```




### wolfSSH_set_fd()







**Synopsis**


**Description**


提供されたファイル記述子をSSHオブジェクトに割り当てます。SSHセッションでは、デフォルトのI/OコールバックでネットワークI/Oのファイル記述子を使用します。


**Return Values**



#### WS_SUCCESS



WS_BAD_ARGUMENT  - パラメーターの1つは無効です


**Parameters**


** ssh **  -  fd ** fd **を設定するセッション - セッションで使用されるソケットのファイル記述子


**参照**


wolfSSH_get_fd()



```
#include <wolfssh/ssh.h>
int wolfSSH_set_fd(WOLFSSH* ssh , int fd );
```




### wolfSSH_get_fd()







**Synopsis**


**Description**


この関数は、SSH接続の入力/出力機能として使用されるファイル記述子(** fd **)を返します。通常、これはソケットファイルの記述子になります。


**Return Values**


** int **  - ファイル記述子** ws_bad_arguement **


**Parameters**


**ssh** -  SSLセッションへのポインタ。


**参照**


wolfSSH_set_fd()



```
#include <wolfssh/ssh.h>
int wolfSSH_get_fd(const WOLFSSH* ssh );
```




## データハイウォーターマーク機能








### wolfSSH_SetHighwater()





**Synopsis**


**Description**


SSHセッションのHighwaterマークを設定します。


**Return Values**


WS_SUCCESSWS_BAD_ARGUMENT


**Parameters**


** ssh- ** wolfsshセッションへのポインター** highwater **  - 高水セキュリティマークを示すデータ



```
#include <wolfssh/ssh.h>
int wolfSSH_SetHighwater(WOLFSSH* ssh , word32 highwater );
```




### wolfSSH_GetHighwater()





**Synopsis**


**Description**


高水セキュリティマークを返します


**Return Values**


** word32 **  - 高水セキュリティマーク。


**Parameters**


** SSH- ** wolfSSHセッションへのポインター



```
#include <wolfssh/ssh.h>
word32 wolfSSH_GetHighwater(WOLFSSH* ssh );
```




### wolfSSH_SetHighwaterCb()





**Synopsis**


**Description**


wolfssh_sethighwatercb関数は、SSHセッションの高水セキュリティマークと、高水コールバックを設定します。


**Return Values**


none


**Parameters**


** CTX **  -  wolfSSHセッションの初期化に使用されるwolfSSHコンテキスト。** Highwater **  -  Highwater Security Mark。



```
#include <wolfssh/ssh.h>
void wolfSSH_SetHighwaterCb(WOLFSSH_CTX* ctx , word32 highwater ,
WS_CallbackHighwater cb );
```




### wolfSSH_SetHighwaterCtx()





**Synopsis**


**Description**


wolfssh_sethighwaterctx関数は、指定されたコンテキストの高水セキュリティマークを設定します。


**Return Values**


none


**Parameters**


** ssh- ** wolfsshセッションへのポインター**ctx**  - ウルフスシュのコンテキストでの高水セキュリティマークへのポインター。



```
#include <wolfssh/ssh.h>
void wolfSSH_SetHighwaterCtx(WOLFSSH* ssh, void* ctx);
```




### wolfSSH_GetHighwaterCtx()





**Synopsis**


**Description**


wolfssh_gethighwaterctx()は、SSHセッションからHighWaterCtxセキュリティマークを返します。


**Return Values**


**Void*** -highwaterセキュリティマーク** null **  -  wolfsshオブジェクトにエラーがある場合。


**Parameters**


** ssh- ** wolfsshオブジェクトへのポインター



```
#include <wolfssh/ssh.h>
void wolfSSH_GetHighwaterCtx(WOLFSSH* ssh );
```




## エラーチェック








### wolfSSH_get_error()







**Synopsis**


**Description**


wolfSSHセッションオブジェクトのエラーセットを返します。


**Return Values**


WS_ERRORCODES(ENUM)


**Parameters**


**ssh** -  wolfSSHオブジェクトへのポインタ


**参照**


wolfSSH_get_error_name()



```
#include <wolfssh/ssh.h>
int wolfSSH_get_error(const WOLFSSH* ssh );
```




### wolfSSH_get_error_name()







**Synopsis**


**Description**


wolfSSHセッションオブジェクトに設定されたエラーの名前を返します。


**Return Values**


** const char ***  - エラー名文字列


**Parameters**


**ssh** -  wolfSSHオブジェクトへのポインタ


**参照**


wolfSSH_get_error()



```
#include <wolfssh/ssh.h>
const char* wolfSSH_get_error_name(const WOLFSSH* ssh );
```




### wolfSSH_ErrorToName()





**Synopsis**


**Description**


パラメーターのエラー番号で呼び出されたときにエラーの名前を返します。


**Return Values**


** const char ***  - エラー文字列の名前


**Parameters**


** err **  - エラーのint値



```
#include <wolfssh/ssh.h>
const char* wolfSSH_ErrorToName(int err );
```




## I/Oコールバック








### wolfSSH_SetIORecv()





**Synopsis**


**Description**


この関数は、wolfSSLの受信コールバックを登録して、入力データを取得します。


**Return Values**


None


**Parameters**


**ctx**  -  SSHコンテキストへのポインター** CB **  -  WolfSSHコンテキストの受信コールバックとして登録される機能** CTX **。この関数の署名は、上記の概要セクションに示すように、それに従う必要があります。



```
#include <wolfssh/ssh.h>
void wolfSSH_SetIORecv(WOLFSSH_CTX* ctx , WS_CallbackIORecv cb );
```




### wolfSSH_SetIOSend()





**Synopsis**


**Description**


この関数は、wolfsSLの送信コールバックを登録して出力データを書き込みます。


**Return Values**


None


**Parameters**


**ctx**  -  wolfsshコンテキストへのポインター** cb **  -  wolfsshコンテキストの送信コールバックとして登録される機能**ctx**。この関数の署名は、上記の概要セクションに示すように、それに従う必要があります。



```
#include <wolfssh/ssh.h>
void wolfSSH_SetIOSend(WOLFSSH_CTX* ctx , WS_CallbackIOSend cb );
```




### wolfSSH_SetIOReadCtx()





**Synopsis**


**Description**


この関数は、SSHセッションのコンテキストを受信コールバック関数を登録します。


**Return Values**


None


**Parameters**


** ssh **  -  wolfSSHオブジェクトへのポインター**ctx**  -  sshセッション(** ssh **)に登録されるコンテキストへのポインタは、callbackfunctionを受信します。



```
#include <wolfssh/ssh.h>
void wolfSSH_SetIOReadCtx(WOLFSSH* ssh , void* ctx );
```




### wolfSSH_SetIOWriteCtx()





**Synopsis**


**Description**


この関数は、SSHセッションの送信コールバック関数のコンテキストを登録します。


**Return Values**


None


**Parameters**


** ssh **  -  wolfSSHセッションへのポインター。



```
#include <wolfssh/ssh.h>
void wolfSSH_SetIOWriteCtx(WOLFSSH* ssh , void* ctx );
```




### wolfSSH_GetIOReadCtx()





**Synopsis**


**Description**


この関数は、WOLFSSH構造体のIOReadCtxメンバーを返します。


**Return Values**


**Void*** -wolfSSH構造のIOReadCtxメンバーへのポインタ。


**Parameters**


**ssh** -  wolfSSHオブジェクトへのポインタ



```
#include <wolfssh/ssh.h>
void* wolfSSH_GetIOReadCtx(WOLFSSH* ssh );
```




### wolfSSH_GetIOWriteCtx()





**Synopsis**


**Description**


この関数は、wolfSSH構造のIOWriteCtxメンバーを返します。


**Return Values**


**Void***  -  wolfSSH構造のIOWriteCtxメンバーへのポインタ。


**Parameters**


**ssh** -  wolfSSHオブジェクトへのポインタ



```
#include <wolfssh/ssh.h>
void* wolfSSH_GetIOWriteCtx(WOLFSSH* ssh );
```




## ユーザ認証








### wolfSSH_SetUserAuth()





**Synopsis**


**Description**


wolfssh_setuserauth()関数は、コンテキストがnullに等しくない場合、thecurrent wolfsshコンテキストのユーザー認証を設定するために使用されます。


**Return Values**


None


**Parameters**


**ctx**  -  wolfsshのコンテキストへのポインター** cb **  - ユーザー認証のために関数を呼び戻す



```
#include <wolfssh/ssh.h>
void wolfSSH_SetUserAuth(WOLFSSH_CTX* ctx ,
WS_CallbackUserAuth cb )
```




### wolfSSH_SetUserAuthCtx()





**Synopsis**


**Description**


wolfssh_setuserauthctx()関数は、SSHセッションでユーザーオーテンションコンテキストの値を設定するために使用されます。


**Return Values**


None


**Parameters**


** ssh **  -  wolfSSHオブジェクトへのポインター**userAuthCtx**  - ユーザー認証コンテキストへのポインター



```
#include <wolfssh/ssh.h>
void wolfSSH_SetUserAuthCtx(WOLFSSH* ssh , void*
userAuthCtx )
```




### wolfSSH_GetUserAuthCtx()





**Synopsis**


**Description**


wolfSSH_GetUserAuthCtx()関数は、userAuthCtxコンテキストへのポインターを返すために使用されます。


**Return Values**


**Void***  - ユーザー認証コンテキストへのポインター** null **  -  sshがnullに等しい場合に戻ります


**Parameters**


**ssh** -  wolfSSHオブジェクトへのポインタ



```
#include <wolfssh/ssh.h>
void* wolfSSH_GetUserAuthCtx(WOLFSSH* ssh )
```




## ユーザー名を設定します








### wolfSSH_SetUsername()





**Synopsis**


**Description**


SSH接続に必要なユーザー名を設定します。


**Return Values**


WS_BAD_ARGUMENTWS_SUCCESSWS_MEMORY_E


**Parameters**


** SSH- ** wolfSSHセッションへのポインター**ユーザー名**  -  SSH接続の入力ユーザー名。



```
#include <wolfssh/ssh.h>
int wolfSSH_setUsername(WOLFSSH* ssh , const char* username );
```




## 接続関数




### wolfSSH_accept()







**Synopsis**


**Description**


wolfssh_acceptはサーバー側で呼び出され、SSHクライアントがSSHハンドシェイクを開始するのを待ちます。


wolfssl_accept()は、ブロッキングI/OノンブロッキングI/Oの両方で機能します。基礎となるI/Oが非ブロッキングである場合、wolfSSH_accept()は、基礎となるI/OがwolfSSH_acceptのニーズを満たして握手を続けることができなかったときに戻ります。この場合、wolfssh_get_error()への呼び出しは、** ws_want_read **または** ws_want_write **のいずれかを生成します。


次に、呼び出しプロセスは、データが読み取られ、wolfSSHが中断されたところからピックアップする場合、wolfSSH_acceptへの呼び出しを繰り返す必要があります。非ブロッキングソケットを使用する場合、何も実行する必要はありませんが、select()を使用して必要な条件を確認できます。


基礎となるI/Oがブロックしている場合、wolfssh_accept()は、握手が終了したか、エラーが発生した場合にのみ戻ります。


**Return Values**


** ws_success **  - 関数が成功しました。** ws_bad_argument **  - パラメーター値はnullでした。


**Parameters**


**ssh** -  wolfSSHセッションへのポインタ


**参照**


wolfSSH_stream_read()



```
#include <wolfssh/ssh.h>
int wolfSSH_accept(WOLFSSH* ssh);
```




### wolfSSH_connect()





**Synopsis**


**Description**


この関数はクライアント側で呼び出され、サーバーを使用してSSHの握手を開始します。この関数が呼ばれると、基礎となる通信チャネルはすでにビーンセットアップされています。


wolfssh_connect()は、ブロッキングI/OノンブロッキングI/Oの両方で動作します。Underlying I/Oが非ブロッキングである場合、wolfssh_connect()は、基礎となるi/ocがwolfssh_connectのニーズを満たさない場合に戻り、握手を続けます。このケースでは、wolfssh_get_error()への呼び出しは、** ws_want_read **または** ws_want_write **のいずれかを生成します。次に、呼び出しプロセスは、基礎となるI/Oの準備ができていて、wolfSSHがITLEFTをオフにするときに、Callolfssh_Connect()を繰り返す必要があります。非ブロッキングソケットを使用する場合、必要な条件を確認するために使用される()select()select()は何もする必要はありません。


基礎となるI/Oがブロックしている場合、wolfssh_connect()は、ハンドシェイクが終了したか、エラーが発生した後にのみ戻ります。


**Return Values**


** WS_BAD_ARGUMENTWS_FATAL_ERRORWS_SUCCESS **  - コールが成功した場合にこれは返されます。


**Parameters**


**ssh** -  wolfSSHセッションへのポインター



```
#include <wolfssh/ssh.h>
int wolfSSH_connect(WOLFSSH* ssh);
```




### wolfSSH_shutdown()





**Synopsis**


**Description**


SSHチャネルを閉じて切断します。


**Return Values**


** ws_bad_argument **  - パラメーターがnull **WS_SUCCES**の場合に返されます - すべてが正しくシャットダウンされているときに戻ります


**Parameters**


** SSH- ** wolfSSHセッションへのポインター



```
#include <wolfssh/ssh.h>
int wolfSSH_shutdown(WOLFSSH* ssh);
```




### wolfSSH_stream_read()







**Synopsis**


**Description**


wolfssh_stream_readは、内部復号化されたデータstreambufferから** bufsz ** bytesまで読み取ります。バイトは内部バッファーから削除されます。


wolfssh_stream_read()は、ブロッキングI/oと非ブロッキングI/Oの両方で動作します。Underlying I/Oが非ブロッキングの場合、wolfSSH_stream_read()は、wolfsshssh_stream_readのニーズを満たすことができなかったときに戻ります。このケースでは、wolfssh_get_error()への呼び出しは、** ws_want_read **または** ws_want_write **のいずれかを生成します。呼び出しプロセスは、データが読み取られ、wolfSSHがITLEFTをオフにする場所をピックアップする場合、Callolfssh_stream_readを繰り返す必要があります。非ブロッキングソケットを使用する場合、必要な条件を確認するために使用される()select()select()は何もする必要はありません。


基礎となるI/Oがブロックされている場合、WolfSSH_STREAM_READ()は、データがISABaibleまたはエラーが発生した場合にのみ返されます。


**Return Values**


**> 0 **  - 成功を読み取るバイト数** 0 **  - クリーン接続のシャットダウンまたはASocketによって引き起こされるソケット障害で返されます。*ws_eof **  - ストリームの終了に達したときに戻ります


**Parameters**


**ssh** -  wolfSSHセッションへのポインタ



```
#include <wolfssh/ssh.h>
int wolfSSH_stream_read(WOLFSSH* ssh ,
byte* buf , word32 bufSz );
```



** buf **  -  wolfssh_stream_read()がデータを配置する場所** bufsz **  - バッファのサイズ


**参照**


wolfSSH_accept()wolfSSH_stream_send()





### wolfSSH_stream_send()







**Synopsis**


**Description**


wolfssh_stream_sendは** bufsz ** bufからsshストリームデータバッファーへのバイトを書き込みます。内部バッファーから除去されます。


wolfssh_stream_send()は、ブロッキングI/oと非ブロッキングI/Oの両方で動作します。Underlying I/Oが非ブロッキングである場合、wolfsshi/oがwolfssh_stream_sendのニーズを満たすことができなかった場合、wolfssh_stream_send()が戻ります。この場合、wolfssh_get_error()がcallto ws_want_read **または** ws_want_write **のいずれかを生成します。その後、Socketが送信し、wolfSSHが中断したところから拾うときに、wolfssh_stream_sendへの呼び出しを繰り返す必要があります。非ブロッキングソケットを使用する場合、何もする必要はありませんが、select()を使用して必要なコンディションを確認できます。


基礎となるI/Oがブロックしている場合、wolfSSH_stream_send()は、データハスが送信されたときにのみ返されます。


**Return Values**


**> 0 **  - 成功すると書かれたバイト数** 0 **  - クリーン接続のシャットダウンまたはソケットエラーのいずれかによって引き起こされるソケット障害で返されます。 - エラーがありました、詳細については、wolfssh_get_error()に電話してください** ws_bad_argument **パラメーターのいずれかがnullの場合


**Parameters**


** ssh **  -  wolfsshセッションへのポインター** buf **  -  buffer wolfssh_stream_send()



```
#include <wolfssh/ssh.h>
int wolfSSH_stream_send(WOLFSSH* ssh , byte* buf , word32
bufSz );
```



** bufsz **  - バッファのサイズ


**参照**


wolfSSH_accept()wolfSSH_stream_read()





### wolfSSH_stream_exit()





**Synopsis**


**Description**


この関数は、SSHストリームを終了するために使用されます。


**Return Values**


** ws_bad_argument **  - パラメーター値がnull ** ws_success **の場合は返されます - 機能が成功した場合は返されます


**Parameters**


**ssh** -  wolfSSHセッションへのポインター**ステータス**  -  SSH接続のステータス



```
#include <wolfssh/ssh.h>
int wolfSSH_stream_exit(WOLFSSH* ssh, int status);
```




### wolfSSH_TriggerKeyExchange()





**Synopsis**


**Description**


キー交換プロセスをトリガーします。割り当てられたハンドシェイクインフォのパケットを準備して送信します。


**Return Values**


** ws_bad_arguement **  -  if ** ssh ** is null ** ws_success **


**Parameters**


**ssh** -  wolfSSHセッションへのポインタ



```
#include <wolfssh/ssh.h>
int wolfSSH_TriggerKeyExchange(WOLFSSH* ssh );
```




## テスト機能








### wolfSSH_GetStats()





**Synopsis**


**Description**


更新** txcount **、** rxcount **、** seq **、および** peerseq **それぞれの** ssh ** sessionstatistics。


**Return Values**


none


**Parameters**


** ssh **  -  wolfSSHセッションへのポインター** txcount **  -  ** ssh **セッションで転送された総バイトが保存される場所アドレス。** seq **  - パケットシーケンス番号は最初に0であり、すべてのパケット後にインクリメントされます



```
#include <wolfssh/ssh.h>
void wolfSSH_GetStats(WOLFSSH* ssh , word32* txCount , word32*
rxCount ,
word32* seq , word32* peerSeq )
```




### wolfSSH_KDF()





**Synopsis**


**Description**


これは、APIテストがキー派生の既知の回答テストを行うことができるように使用されます。


キー導出関数は、ソースキーイング材料** k **および** h **に基づいて対称的な** key **を導出します。ここで、** k **はdiffie-hellmanの共有秘密であり、** h **は最初のキー交換中に生成されたハンドシェイクのハッシュです。** keyid **および** hashid **によって指定されている複数のタイプのキーが導出される可能性があります。



```
Initial IV client to server: keyId=A
Initial IV server to client: keyId=B
Encryption key client to server: keyId=C
Encryption key server to client: keyId=D
Integrity key client to server: keyId=E
Integrity key server to client:keyId=F
```

**Return Values**


WS_SUCCESSWS_CRYPTO_FAILED


**Parameters**


** hashid **  - キーイング材料を生成するためのハッシュのタイプ。E.G。(wc_hash_type_shaおよびwc_hash_type_sha256)** keyid **  - 文字a -f **キーを作成するキーを示す



```
#include <wolfssh/ssh.h>
int wolfSSH_KDF(byte hashId , byte keyId , byte* key , word32
keySz ,
const byte* k , word32 kSz , const byte* h , word32
hSz ,
const byte* sessionId , word32 sessionIdSz );
```



** keysz **  - 必要なサイズ** keyk **  -  diffie-hellmanキーエクスチェンジからの共有秘密** ksz **  - 共有秘密のサイズ(** k **)** h **  - ハッシュキーエクスチェンジ中に生成された握手** hsz **  - ハッシュのサイズ(** h **)** sessionid **  - 最初の** h **の一意の識別子計算。** sessionidsz **  - サイズのサイズ** sessionid **





## セッション関数








### wolfSSH_GetSessionType()





**Synopsis**


**Description**


wolfssh_getSessionType()は、セッションの種類を返すために使用されます


**Return Values**


WOLFSSH_SESSION_UNKNOWNWOLFSSH_SESSION_SHELLWOLFSSH_SESSION_EXECWOLFSSH_SESSION_SUBSYSTEM


**Parameters**


** SSH- ** wolfSSHセッションへのポインター



```
#include <wolfssh/ssh.h>
WS_SessionType wolfSSH_GetSessionType(const WOLFSSH* ssh );
```




### wolfSSH_GetSessionCommand()





**Synopsis**


**Description**


この関数は、セッションの現在のコマンドを返すために使用されます。


**Return Values**


** const char ***  - コマンドへのポインター


**Parameters**


** SSH- ** wolfSSHセッションへのポインター



```
#include <wolfssh/ssh.h>
const char* wolfSSH_GetSessionCommand(const WOLFSSH* ssh );
```




## ポート転送機能








### wolfSSH_ChannelFwdNew()





**Synopsis**


**Description**


wolfSSHセッションにTCP/IP転送チャネルを設定します。SSHセッションが接続され、認証された場合、ポート_hostport_のaddress_host_のインターフェイスにローカルリスナーが作成されます。そのリスナーの新しい接続があれば、SSHサーバーへの新しいChannelRequestをトリガーして、ポート_hostport_で_host_への接続を確立します。


**Return Values**


** wolfssh_chan ***  - エラーまたは新しいチャンネルレコードでnull


**Parameters**


** ssh **  -  wolfssh session ** host **  - バインドリスナーのホストアドレス**ホストポート**  - リスナーを結合するホストポート**起源**  - 原産地接続のIPアドレス** OriginPort **  - ポート番号 - ポート番号発生する接続の



```
#include <wolfssh/ssh.h>
WOLFSSH_CHANNEL* wolfSSH_ChannelFwdNew(WOLFSSH* ssh ,
const char* host , word32 hostPort ,
const char* origin , word32 originPort );
```




### wolfSSH_ChannelFree()





**Synopsis**


**Description**


チャネル_Channel_に割り当てられたメモリをリリースします。チャネルは、セッションのチャネルリストから削除されます。


**Return Values**


** int **  - エラーコード


**Parameters**


**チャンネル**  -  wolfsshチャンネルを無料で



```
#include <wolfssh/ssh.h>
int wolfSSH_ChannelFree(WOLFSSH_CHANNEL* channel );
```




### wolfSSH_worker()





**Synopsis**


**Description**


wolfSSH Worker機能は接続をベビーシットし、データが受信されると処理します。SSHセッションにはセッションの簿記メッセージがたくさんあり、これにより自動的にケアがあります。特定のチャネルのデータが受信されると、労働者はデータをチャネルに配置します。(function wolfssh_stream_read()dosmuchも同じですが、単一のチャネルの受信データも返します。)wolfssh_worker()は次のアクションを実行します。



1. _outputbuffer_に保留中のデータを送信しようとします。


2. セッションのソケットに_doreceive()_を呼び出します。


3. 特定のチャネルのデータが受信された場合、データを返して通知を受け取り、

チャネルID。


**Return Values**


** int **  - エラーまたはステータス** ws_channel_rxd **  - チャネルでデータが受信され、IDが設定されています


**Parameters**


** ssh **  -  wolfSSHセッションへのポインター** id **  -  ID値を保存するための場所へのポインター



```
#include <wolfssh/ssh.h>
int wolfSSH_worker(WOLFSSH* ssh , word32* channelId );
```




### wolfSSH_ChannelGetId()





**Synopsis**


**Description**


チャネルが与えられた場合、チャネルのIDまたはピアIDを返します。


**Return Values**


** int **  - エラーコード


**Parameters**


**チャンネル**  - チャンネルへのポインター** ID **  -  ID値を保存するための場所へのポインター**ピア**  - 自己(私のチャンネルID)またはピア(私のピアチャネルID)



```
#include <wolfssh/ssh.h>
int wolfSSH_ChannelGetId(WOLFSSH_CHANNEL* channel ,
word32* id , byte peer );
```




### wolfSSH_ChannelFind()





**Synopsis**


**Description**


セッション_SSH_が与えられた場合、_ID_に関連付けられたチャネルを見つけます。


**Return Values**


** wolfssh_channel ***  - チャンネルへのポインター、IDがリストにない場合はnull


**Parameters**


** ssh **  -  wolfssh session ** id **  - チャンネルID **ピア**  - 自己(私のチャンネルID)またはピア(私のピアのチャンネルID)のいずれか



```
#include <wolfssh/ssh.h>
WOLFSSH_CHANNEL* wolfSSH_ChannelFind(WOLFSSH* ssh ,
word32 id , byte peer );
```




### wolfSSH_ChannelRead()





**Synopsis**


**Description**


チャネルオブジェクトからデータをコピーします。


**Return Values**


** int **  - バイト読み取り**> 0 **  - 成功して読み取るバイト数** 0 **  - クリーン接続のシャットダウンまたはasocketエラーのいずれかでソケット障害の原因を返します。詳細についてはwolfssh_get_error()*WS_FATAL_ERROR **  - 他にもエラーがありました。wolfSSH_get_Error()Formoreの詳細を呼び出しました


**Parameters**


**チャンネル**  -  wolfSSH Channelへのポインター** buf **  -  wolfssh_channelreadがデータを配置するバッファ** bufsz **  - バッファーのサイズ



```
#include <wolfssh/ssh.h>
int wolfSSH_ChannelRead(WOLFSSH_CHANNEL* channel ,
byte* buf , word32 bufSz );
```




### wolfSSH_ChannelSend()





**Synopsis**


**Description**


指定されたチャネルを介してピアにデータを送信します。データはチャネルデータメッジにパッケージ化されています。これにより、ピアソケットからできるだけ多くのデータが送信されます。Moretoが送信されている場合、_wolfSSH_Worker()_への呼び出しは、チャンネルのピアのより多くのデータを送信し続けます。


**Return Values**


** int **  - 送信されたバイト**> 0 **  - 成功時に送信されるバイト数** 0 **  - クリーン接続のシャットダウンまたはASocketエラーのいずれかでソケット障害の原因を返します。*WS_FATAL_ERROR **  - 他のエラーがありました。wolfSSH_get_Error()フォルモールの詳細を呼び出しました


**Parameters**


**チャンネル**  -  wolfSSH Channelへのポインター** buf **  - バッファーwolfssh_channelsend()は** bufsz **  - バッファーのサイズを送信します



```
#include <wolfssh/ssh.h>
int* wolfSSH_ChannelSend(WOLFSSH_CHANNEL* channel ,
const byte* buf , word32 bufSz );
```




### wolfSSH_ChannelExit()





**Synopsis**


**Description**


チャネルを終了し、ピアに密接なメッセージを送信すると、チャネルがアセルドされています。これはチャネルを解放せず、チャネルリストに残ります。閉鎖後、チャンネルでデータを送信することはできませんが、データを受信できる可能性があります。(現時点では、EOFを送信し、閉じ、チャネルを削除します。)


**Return Values**


** int **  - エラーコード


**Parameters**


**チャンネル**  -  wolfSSHセッションチャネル



```
#include <wolfssh/ssh.h>
int wolfSSH_ChannelExit(WOLFSSH_CHANNEL* channel );
```




### wolfSSH_ChannelNext()





**Synopsis**


**Description**


_SSH_のチャンネルリストの_Channel_の後に次のチャンネルを返します。_Channel_がnullの場合、_ssh_のチャネルリストのFirstChannelが返されます。


**Return Values**


** wolfssh_channel ***  - 最初のチャンネル、次のチャネル、またはnullへのポインター


**Parameters**


** ssh **  -  wolfsshセッション**チャンネル**  -  wolfsshセッションチャネル



```
#include <wolfssh/ssh.h>
WOLFSSH_CHANNEL* wolfSSH_ChannelFwdNew(WOLFSSH* ssh ,
WOLFSSH_CHANNEL* channel );
```




