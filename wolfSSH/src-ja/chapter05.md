

# wolfSSHユーザー認証コールバック



wolfSSHは、ライブラリが埋め込まれている環境に関係なく、サーバーに接続するユーザーを認証できる必要があります。ルックアップは、テキストファイル、データベース、またはアプリケーションにハードコード化されたパスワードまたはRSAパブリックキーを使用して実行する必要があります。


wolfSSHは、ユーザー認証メッセージと要求された認証タイプで提供されるパスワードまたは公開鍵のユーザー名を受信するコールバックフックを提供します。その後、コールバック関数は適切な検索を実行し、返信します。コールバックを提供する必要があります。


コールバックは、いくつかの失敗のいずれかまたは成功のいずれかを返すはずです。ライブラリは、ロギングの目的を除いてすべての障害を同じように扱います。つまり、ユーザー認証失敗メッセージを再試行するクライアントに返します。


パスワードの検索の場合、プレーンテキストパスワードがコールバック関数に与えられます。ユーザー名とパスワードをチェックする必要があり、それらが一致する場合、成功が返されます。成功すると、SSHの握手はすぐに続きます。現時点では、パスワードの変更はサポートされていません。


公開鍵の検索の場合、クライアントからの公開鍵ブロブがコールバック関数に与えられます。公開鍵は、有効なクライアントパブリックキーのサーバーのリストに対してチェックされます。提供された公開鍵がそのユーザーの既知の公開鍵と一致する場合。wolfSSHライブラリは、RFC4252§7で説明されているプロセスに従って、ユーザー認証署名の実際の検証を実行します。


一般的に、パブリックキーの場合、サーバーは、SSH-Keygenユーティリティによって生成されたユーザーのパブリックキーを保存するか、公開鍵の指紋を保存します。ユーザーのこの値は比較されるものです。クライアントは、セッションIDの署名と、秘密キーを使用してユーザー認証要求メッセージを提供します。サーバーは、公開鍵を使用してこの署名を検証します。



## コールバック関数プロトタイプ

ユーザー認証コールバック関数のプロトタイプは次のとおりです。

```
int UserAuthCb(byte authType , const WS_UserAuthData*
authData , void* ctx );
```

この関数プロトタイプはタイプです。



```
WS_CallbackUserAuth
```

パラメーター`authType`は次のとおりです。



```
WOLFSSH_USERAUTH_PASSWORD
```

また

```
WOLFSSH_USERAUTH_PUBLICKEY
```

パラメーターであるAuthDataは、認証データへのポインターです。


ws_userauthdataの説明については、セクション5.4を参照してください


パラメーター** ctx **は、アプリケーション定義コンテキストです。wolfSSHは、コンテキスト内のデータについて何もしておらず、何も知りません。コールバック関数へのコンテキストポインターのみを提供します。



## コールバック関数認証タイプ定数



以下は、`authType`パラメーターのユーザー認証コールバック関数に渡された値です。確認する認証データのタイプに関してコールバック関数をガイドします。システムは、異なるユーザーにパスワードまたは公開鍵のいずれかを使用できます。



```
WOLFSSH_USERAUTH_PASSWORD
WOLFSSH_USERAUTH_PUBLICKEY
```




## コールバック関数返品コード定数



以下は、コールバック関数がライブラリに戻るものとするリターンコードです。障害コードは、何も行われておらず、コールバックがチェックを行うことができなかったことを示しています。


無効なコードは、ユーザー認証が拒否されている理由を示しています。



```
invalid username
invalid password
invalid public key
```

ライブラリは、クライアントに_success_または_failure_のみを示しています。特定の障害タイプは、ロギングにのみ使用されます。



```
WOLFSSH_USERAUTH_SUCCESS
WOLFSSH_USERAUTH_FAILURE
WOLFSSH_USERAUTH_INVALID_USER
WOLFSSH_USERAUTH_INVALID_PASSWORD
WOLFSSH_USERAUTH_INVALID_PUBLICKEY
```




## コールバック関数データ型



クライアントデータは、`WS_UserAuthData`と呼ばれる構造のコールバック関数に渡されます。メッセージ内のデータへのポインターが含まれています。一般的なフィールドはこの構造にあります。メソッド固有のフィールドは、ユーザー認証データの構造の結合にあります。



```
typedef struct WS_UserAuthData {
    byte authType ;
    byte* username ;
    word32 usernameSz ;
    byte* serviceName ;
    word32 serviceNameSz ; n
    union {
        WS_UserAuthData_Password password ;
        WS_UserAuthData_PublicKey publicKey ;
    } sf;
} WS_UserAuthData;
```




### パスワード



`username`および`usernameSz`パラメーターは、クライアントが提供するユーザー名とオクテットのサイズです。


`password`および`passwordSz`フィールドは、クライアントのパスワードとオクテットのサイズです。


クライアントが提供する場合は設定されている間、パラメーター`hasNewPassword`、`newPassword`、および`newPasswordSz`は使用されません。現時点でパスワードを変更するようクライアントに指示するメカニズムはありません。



```
typedef struct WS_UserAuthData_Password {
    uint8_t* password ;
    uint32_t passwordSz ;
    uint8_t hasNewPasword ;
    uint8_t* newPassword ;
    uint32_t newPasswordSz ;
} WS_UserAuthData_Password;
```




### 公開鍵



wolfSSHは、複数の公開鍵アルゴリズムをサポートします。publicKeyTypeメンバーは、使用されるアルゴリズム名を指しています。


パブリックキーフィールドは、クライアントが提供する公開鍵ブロブを指しています。


公開鍵のチェックには、1つまたは2つのステップがあります。まず、`hasSignature`フィールドが設定されていない場合、署名はありません。`username`と`publicKey`のみが予想されます。このステップは、クライアントの構成に応じてオプションであり、無効なユーザーを使用して費用のかかる公開鍵操作を行うことを節約できます。次に、`hasSignature`フィールドが設定され、クライアントの署名のフィールドポイントが署名されます。再び`username`および`publicKey`をチェックする必要があります。wolfSSHは署名を自動的にチェックします。


各フィールドは、オクテットサイズ値です。



```
typedef struct WS_UserAuthData_PublicKey {
    byte* publicKeyType;
    word32 publicKeyTypeSz;
    byte* publicKey;
    word32 publicKeySz;
    byte hasSignature;
    byte* signature;
    word32 signatureSz;
} WS_UserAuthData_PublicKey;
```
