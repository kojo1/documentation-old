

# wolfSSL SFTP APIリファレンス




## 接続関数








### wolfSSH_SFTP_accept()







**Synopsis:**


**Description:**


クライアントからの着信接続要求を処理する機能。


**Return Value**


成功にws_sftp_completeを返します。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_accept(WOLFSSH* ssh );
```


```
WOLFSSH* ssh;
```


```
//create new WOLFSSH structure
...
```


```
if (wolfSSH_SFTP_accept(ssh) != WS_SUCCESS) {
//handle error case
}
```



**参照：**


wolfSSH_SFTP_free(), wolfSSH_new(), wolfSSH_SFTP_connect()



### wolfSSH_SFTP_connect()







**Synopsis:**


**Description:**


SFTPサーバーへの接続を開始するための機能。


**Return Value**


** ws_sftp_complete：**成功。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター


**Example:**


**参照：**


wolfSSH_SFTP_accept(), wolfSSH_new(), wolfSSH_free()



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_connect(WOLFSSH* ssh );
```


```
WOLFSSH* ssh;
```


```
//after creating a new WOLFSSH structrue
```


```
wolfSSH_SFTP_connect(ssh);
```




### wolfSSH_SFTP_negotiate()







**Synopsis:**


**Description:**


クライアントからの着信接続のいずれかを処理するか、サーバーにaconnectionリクエストを送信する機能。これは、接続のどの側面に依存します。これは、どのアクションが実行されるかに設定されています。


**Return Value**


成功にWS_SUCCESSを返します。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター


**Example:**


**参照：**


wolfSSH_SFTP_free()



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_negotiate(WOLFSSH* ssh)
```


```
WOLFSSH* ssh;
```


```
//create new WOLFSSH structure with side of connection
set
....
```


```
if (wolfSSH_SFTP_negotiate(ssh) != WS_SUCCESS) {
//handle error case
}
```



wolfSSH_new(), wolfSSH_SFTP_connect(), wolfSSH_SFTP_accept()





## プロトコルレベル関数








### wolfSSH_SFTP_RealPath()







**Synopsis:**


**Description:**


RealPathパケットをピアに送信する機能。ファイルの名前がfrompeerから返されます。


**Return Value**


成功時にWS_SFTPNAME構造にポインターを返し、エラー時にnullします。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**dir**  - 実際のパスを取得するためのディレクトリ /ファイル名


**Example:**



```
#include <wolfssh/wolfsftp.h>
WS_SFTPNAME* wolfSSH_SFTP_RealPath(WOLFSSH* ssh , char*
dir );
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



```
WOLFSSH* ssh ;
```


```
//set up ssh and do sftp connections
...
```


```
if (wolfSSH_SFTP_read( ssh ) != WS_SUCCESS) {
//handle error case
}
```




### wolfSSH_SFTP_Close()







**Synopsis:**


**Description:**


ピアに密接なパケットを送信する機能。


**Return Value**


**WS_SUCCESS** 成功。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインタ
**handle**  - 閉じようとするハンドル
**handleSz**  - ハンドルバッファーのサイズ


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_Close(WOLFSSH* ssh , byte* handle , word32
handleSz );
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



```
WOLFSSH* ssh;
byte handle[HANDLE_SIZE];
word32 handleSz=HANDLE_SIZE;
```


```
//set up ssh and do sftp connections
...
```


```
if (wolfSSH_SFTP_Close(ssh, handle, handleSz) !=
WS_SUCCESS) {
//handle error case
}
```




### wolfSSH_SFTP_Open()







**Synopsis:**


**Description:**


開いたパケットをピアに送信する機能。これにより、バッファーのサイズでハンドルズを設定し、ピアから結果のハンドルを取得し、バッファハンドルに配置します。


OPENの利用可能な理由：wolfssh_fxf_readwolfssh_fxf_writewolfssh_fxf_appendwolfssh_fxf_creatwolfs_fxf_truncwolfssh_fxf_excl


**Return Value**


**WS_SUCCESS**成功。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**dir**  - 開くファイルの名前
**reason**  - ファイルを開く理由
**atr**  - ファイルの初期属性
**handle** - Open からの結果のハンドル
**handlesz** - 結果のハンドルのサイズ


```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_Open(WOLFSSH* ssh , char* dir , word32
reason ,
WS_SFTP_FILEATRB* atr , byte* handle , word32* handleSz ) ;
```



**Example:**


**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



```
WOLFSSH* ssh ;
char name[NAME_SIZE];
byte handle[HANDLE_SIZE];
word32 handleSz=HANDLE_SIZE;
WS_SFTP_FILEATRB atr;
```


```
//set up ssh and do sftp connections
...
```


```
if (wolfSSH_SFTP_Open( ssh , name , WOLFSSH_FXF_WRITE |
WOLFSSH_FXF_APPEND | WOLFSSH_FXF_CREAT , & atr , handle ,
& handleSz )
!= WS_SUCCESS) {
//handle error case
}
```




### wolfSSH_SFTP_SendReadPacket()



**Synopsis:**


**Description:**


読み取りパケットをピアに送信する機能。バッファハンドルには、WolfSSH_SFTP_OPENへの以前の呼び出しが含まれている必要があります。読み取りから得られたバイトは、「out」バッファーにプレースされます。


**Return Value**


成功して読み取られたバイト数を返します。負の値は失敗時に返されます。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**handle**  -  **handlesz**から読み取ろうとするハンドル - ハンドルバッファーのサイズ
**ofst**  - オフセットから読み始める
**out** - 読み取りの結果を保持するバッファ
**outsz**  -  outバッファのサイズ


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_SendReadPacket(WOLFSSH* ssh , byte*
handle , word32
handleSz , word64 ofst , byte* out , word32 outSz );
```



**参照：**


wolfSSH_SFTP_SendWritePacket(), wolfSSH_SFTP_Open()



```
WOLFSSH* ssh;
byte handle[HANDLE_SIZE];
word32 handleSz=HANDLE_SIZE;
byte out[OUT_SIZE];
word32 outSz=OUT_SIZE;
word32 ofst=0;
int ret;
```


```
//set up ssh and do sftp connections
...
//get handle with wolfSSH_SFTP_Open()
```


```
if ((ret=wolfSSH_SFTP_SendReadPacket(ssh, handle,
handleSz, ofst,
out, outSz)) < 0) {
//handle error case
}
//ret holds the number of bytes placed into out buffer
```




### wolfSSH_SFTP_SendWritePacket()







**Synopsis:**


**Description:**


機能パケットをピアに送信する機能。バッファハンドルには、以前のコールTowolfSSH_SFTP_OPEN()の結果を含める必要があります。


**Return Value**


成功に記載されているバイト数を返します。負の値は障害時に返されます。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター**handle**  -  ** handlesz **から読み取ろうとするハンドル - ハンドルバッファーのサイズ** ofst **  - オフセット** out*から読み始める* - 執筆のためにピアに送信するバッファ** outsz **  - バッファーのサイズ


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_SendWritePacket(WOLFSSH* ssh , byte*
handle , word32
handleSz , word64 ofst , byte* out , word32 outSz );
```



**参照：**


wolfSSH_SFTP_SendReadPacket(), wolfSSH_SFTP_Open()



```
WOLFSSH* ssh;
byte handle[HANDLE_SIZE];
word32 handleSz=HANDLE_SIZE;
byte out[OUT_SIZE];
word32 outSz=OUT_SIZE;
word32 ofst=0;
int ret;
```


```
//set up ssh and do sftp connections
...
//get handle with wolfSSH_SFTP_Open()
```


```
if ((ret=wolfSSH_SFTP_SendWritePacket(ssh, handle,
handleSz, ofst,
out,outSz)) < 0) {
//handle error case
}
//ret holds the number of bytes written
```




### wolfSSH_SFTP_STAT()







**Synopsis:**


**Description:**


統計パケットをピアに送信する機能。これにより、ファイルordirectoryの属性が得られます。ファイルまたは属性が存在しない場合、ピアが返され、この機能がエラー値を回転させます。


**Return Value**


**WS_SUCCESS**成功。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター**dir**   -  ** aTr **の属性を取得するファイルまたはディレクトリのnull終了名 - 結果の属性がこの構造に設定されます


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_STAT(WOLFSSH* ssh , char* dir ,
WS_SFTP_FILEATRB* atr);
```



**参照：**


wolfSSH_SFTP_LSTAT(), wolfSSH_SFTP_connect()



```
WOLFSSH* ssh;
byte name[NAME_SIZE];
int ret;
WS_SFTP_FILEATRB atr;
```


```
//set up ssh and do sftp connections
...
```


```
if ((ret=wolfSSH_SFTP_STAT(ssh, name, &atr)) < 0) {
//handle error case
}
```




### wolfSSH_SFTP_LSTAT()



**Synopsis:**


**Description:**


LSTATパケットをピアに送信する機能。これにより、ファイルordirectoryの属性が得られます。これにより、統計パケットがシンボリックリンクに従わないシンボリックリンクに従います。ファイルまたは属性が存在しない場合、ピアが返され、結果としてこの関数はリクルーン語のエラー値を返します。


**Return Value**


成功に関するWS_SUCCESS。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**dir**  - 属性を取得するファイルまたはディレクトリのnull終了名
**atr** - 結果の属性がこの構造に設定されます


Example:



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_LSTAT(WOLFSSH* ssh , char* dir ,
WS_SFTP_FILEATRB* atr );
```



**参照：**


wolfSSH_SFTP_STAT(), wolfSSH_SFTP_connect()



```
WOLFSSH* ssh;
byte name[NAME_SIZE];
int ret;
WS_SFTP_FILEATRB atr;
```


```
//set up ssh and do sftp connections
...
```


```
if ((ret=wolfSSH_SFTP_LSTAT(ssh, name, &atr)) < 0) {
//handle error case
}
```




### wolfSSH_SFTPNAME_free()



**Synopsis:**


**Description:**


単一のws_sftpnameノードを無料で解放する機能。このノードがノードのアリストの真ん中にある場合、リストは壊れることに注意してください。


**Return Value**


None


**Parameters:**


** name **  - 無料になる構造


**Example:**


**参照：**



```
#include <wolfssh/wolfsftp.h>
```


### void wolfssh_sftpname_free(ws_sftpnmae* name);




```
WOLFSSH* ssh;
WS_SFTPNAME* name;
```


```
//set up ssh and do sftp connections
...
name=wolfSSH_SFTP_RealPath(ssh, path);
if (name != NULL) {
wolfSSH_SFTPNAME_free(name);
}
```



wolfSSH_SFTPNAME_list_free


wolfSSH_SFTPNAME_list_free()






**Synopsis:**


**Description:**


リスト内のすべてのWS_SFTPNAMEノードを解放する機能。


**Return Value**


None


**Parameters:**


** name **  - 無料になるリストの責任者


**Example:**



```
#include <wolfssh/wolfsftp.h>
void wolfSSH_SFTPNAME_list_free(WS_SFTPNMAE* name );
```



**参照：**


wolfSSH_SFTPNAME_free()



```
WOLFSSH* ssh;
WS_SFTPNAME* name;
```


```
//set up ssh and do sftp connections
...
```


```
name=wolfSSH_SFTP_LS(ssh, path);
if (name != NULL) {
wolfSSH_SFTPNAME_list_free(name);
}
```




## reget/reput機能




### wolfSSH_SFTP_SaveOfst()







**Synopsis:**


**Description:**


中断されたGETまたはPUTコマンドのオフセットを保存する機能。オフセットは、wolfssh_sftp_getofstを呼び出すことでberecoverすることができます


**Return Value**


成功にWS_SUCCESSを返します。


**Parameters:**


**ssh**  - 接続のためのwolfssh構造へのポインター
**from**  -  fromのパス
**to**  - to


Example:



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_SaveOfst(WOLFSSH* ssh , char* from , char*
to ,
word64 ofst );
```



**参照：**


wolfSSH_SFTP_GetOfst(), wolfSSH_SFTP_Interrupt()



```
WOLFSSH* ssh;
char from[NAME_SZ];
char to[NAME_SZ];
word64 ofst;
```


```
//set up ssh and do sftp connections
...
```


```
if (wolfSSH_SFTP_SaveOfst(ssh, from, to, ofst) !=
WS_SUCCESS) {
//handle error case
}
```




### wolfSSH_SFTP_GetOfst()







**Synopsis:**


**Description:**


中断されたGETまたはPUTコマンドのオフセットを取得する機能。


**Return Value**


成功のオフセット値を返します。保存されていないオフセットが見つかった場合、0が返されます。


**Parameters:**


**ssh**  - 接続のためのwolfssh構造へのポインター
**from**  -  null emonimatedソースパスの文字


**Example:**



```
#include <wolfssh/wolfsftp.h>
word64 wolfSSH_SFTP_GetOfst(WOLFSSH* ssh, char* from,
char* to);
```


```
WOLFSSH* ssh;
char from[NAME_SZ];
char to[NAME_SZ];
word64 ofst;
```


```
//set up ssh and do sftp connections
...
```


```
ofst=wolfSSH_SFTP_GetOfst(ssh, from, to);
//start reading/writing from ofst
```



**参照：**


wolfSSH_SFTP_SaveOfst(), wolfSSH_SFTP_Interrup()



### wolfSSH_SFTP_ClearOfst()







**Synopsis:**


**Description:**


すべての保存されたオフセット値をクリアする機能。


**Return Value**


**WS_SUCCESS**成功


**Parameters:**


**ssh**  -  Wolfssh構造へのポインター


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_ClearOfst(WOLFSSH* ssh);
```



**参照：**


wolfSSH_SFTP_SaveOfst(), wolfSSH_SFTP_GetOfst()



### wolfSSH_SFTP_Interrupt()







**Synopsis:**


**Description:**


割り込みフラグを設定し、get/putコマンドを停止する機能。


**Return Value**


None


**Parameters:**


**ssh**  -  Wolfssh構造へのポインター


**Example:**



```
WOLFSSH* ssh;
```


```
//set up ssh and do sftp connections
...
```


```
if (wolfSSH_SFTP_ClearOfst(ssh) != WS_SUCCESS) {
//handle error
}
```


```
#include <wolfssh/wolfsftp.h>
void wolfSSH_SFTP_Interrupt(WOLFSSH* ssh);
```



**参照：**


wolfSSH_SFTP_SaveOfst(), wolfSSH_SFTP_GetOfst()



```
WOLFSSH* ssh;
char from[NAME_SZ];
char to[NAME_SZ];
word64 ofst;
```


```
//set up ssh and do sftp connections
...
```


```
wolfSSH_SFTP_Interrupt(ssh);
wolfSSH_SFTP_SaveOfst(ssh, from, to, ofst);
```




## コマンド関数








### wolfSSH_SFTP_Remove()







**Synopsis:**


**Description:**


チャネル全体に「削除」パケットを送信する機能。「f」として渡されたファイル名は、削除のためにピアに送信されます。


**Return Value**


**WS_SUCCESS**：成功にWS_SUCCESSを返します。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**f**  - 削除するファイル名


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_Remove(WOLFSSH* ssh , char* f );
```


```
WOLFSSH* ssh;
int ret;
char* name[NAME_SZ];
```


```
//set up ssh and do sftp connections
...
```


```
ret=wolfSSH_SFTP_Remove(ssh, name);
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



### wolfSSH_SFTP_MKDIR()







**Synopsis:**


**Description:**


チャネル全体に「MKDIR」パケットを送信する機能。「dir」が渡されたディレクトリ名は、作成のためにピアに送られます。現在、渡された属性は使用されておらず、代わりにデフォルトの属性が設定されています。


**Return Value**


**WS_SUCCESS**：成功にWS_SUCCESSを返します。


**Parameters:**


ssh -connectiondirに使用されるwolfssh構造へのポインター -  null extrimatedディレクトリが作成されるために - ディレクトリの作成で使用される属性


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_MKDIR(WOLFSSH* ssh , char* dir ,
WS_SFTP_FILEATRB*
atr );
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



### wolfSSH_SFTP_RMDIR()







**Synopsis:**


**Description:**


チャネル全体に「RMDIR」パケットを送信する機能。「dir」が渡されたディレクトリ名は、削除のためにピアに送信されます。


**Return Value**


**WS_SUCCESS**：成功にWS_SUCCESSを返します。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター**dir**   - 削除するnull終了ディレクトリ



```
WOLFSSH* ssh;
int ret;
char* dir[DIR_SZ];
```


```
//set up ssh and do sftp connections
...
```


```
ret=wolfSSH_SFTP_MKDIR(ssh, dir, DIR_SZ);
```


```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_RMDIR(WOLFSSH* ssh , char* dir );
```



**Example:**


**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



```
WOLFSSH* ssh;
int ret;
char* dir[DIR_SZ];
```


```
//set up ssh and do sftp connections
...
```


```
ret=wolfSSH_SFTP_RMDIR(ssh, dir);
```




### wolfSSH_SFTP_Rename()







**Synopsis:**


**Description:**


チャネル全体に「名前変更」パケットを送信する機能。これは、「古い」から「nw」に至るまでピアファイヤーネームを付けようとします。


**Return Value**


**WS_SUCCESS**：成功にWS_SUCCESSを返します。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**old**  - 古いファイル名
**nw**  - 新しいファイル名


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_Rename(WOLFSSH* ssh , const char* old ,
const char*
nw );
```


```
WOLFSSH* ssh;
int ret;
char* old[NAME_SZ];
char* nw[NAME_SZ]; //new file name
```


```
//set up ssh and do sftp connections
...
```


```
ret=wolfSSH_SFTP_Rename(ssh, old, nw);
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



### wolfSSH_SFTP_LS()







**Synopsis:**


**Description:**


ls操作を実行するための関数。これは、すべてのファイルとディレクトリのリストを取得します。これは、RealPath、Opendir、Readdir、および緊密な操作を実行する高レベルの関数です。


**Return Value**


成功すると、故障時にws_sftpname structures.nullのリストにポインターを返します。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**dir**   - リストするディレクトリ


**Example:**



```
#include <wolfssh/wolfsftp.h>
WS_SFTPNAME* wolfSSH_SFTP_LS(WOLFSSH* ssh , char* dir );
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect(), wolfSSH_SFTPNAME_list_free()



```
WOLFSSH* ssh;
int ret;
char* dir[DIR_SZ];
WS_SFTPNAME* name;
WS_SFTPNAME* tmp;
```


```
//set up ssh and do sftp connections
...
```


```
name=wolfSSH_SFTP_LS(ssh, dir);
tmp=name;
while (tmp != NULL) {
printf("%s\n", tmp->fName);
tmp=tmp->next;
}
wolfSSH_SFTPNAME_list_free(name);
```




### wolfSSH_SFTP_Get()







**Synopsis:**


**Description:**


パフォーマンスを実行するための関数ピアからファイルを取得し、Alocal Directoryに配置します。これは、LSTAT、オープン、読み取り、および操作を実行する高レベルの関数です。操作を中断するには、functionwolfssh_sftp_interruptを呼び出します。(この機能のAPIドキュメントを参照してください。


**Return Value**


**WS_SUCCESS**：success。他のすべての返品値はエラーケースと見なされる必要があります。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**from**  -  取得するファイル名
**to** - 
**resume** - 操作の履歴書を試すためにフラグ。1の場合は1つの場合はありません  - ステータスを取得するコールバック関数


**Example:**



```
#include <wolfssh/wolfsftp.h>
```


```
int wolfSSH_SFTP_Get(WOLFSSH* ssh , char* from , char* to ,
byte resume ,
WS_STATUS_CB* statusCb );
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



```
static void myStatusCb(WOLFSSH* sshIn, long bytes, char*
name)
{
char buf[80];
WSNPRINTF(buf, sizeof(buf), "Processed %8ld\t bytes
\r", bytes);
WFPUTS(buf, fout);
(void)name;
(void)sshIn;
}
...
WOLFSSH* ssh;
char* from[NAME_SZ];
char* to[NAME_SZ];
```


```
//set up ssh and do sftp connections
...
```


```
if (wolfSSH_SFTP_Get( ssh , from , to , 0 , & myStatusCb ) !=
WS_SUCCESS) {
//handle error case
}
```




### wolfSSH_SFTP_Put()







**Synopsis:**


**Description:**


ファイルのローカルファイルをPeersディレクトリにプッシュするPut操作を実行するための関数。これは、オープン、書き込み、および操作を実行する高レベルの関数です。操作を中断するには、機能を呼び出しますwolfssh_sftp_interrupt。それが何をしているのかについての詳細)


**Return Value**


**WS_SUCCESS**成功について。他のすべての返品値はエラーケースと見なされる必要があります。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター
**from**  -  送るファイル名
**to**  -
**resume** - 
**statuscb**  - ステータスを取得するコールバック関数


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_Put(WOLFSSH* ssh , char* from , char* to ,
byte resume , WS_STATUS_CB* statusCb );
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



```
static void myStatusCb(WOLFSSH* sshIn, long bytes, char*
name)
{
char buf[80];
WSNPRINTF(buf, sizeof(buf), "Processed %8ld\t bytes
\r", bytes);
WFPUTS(buf, fout);
(void)name;
(void)sshIn;
}
...
```


```
WOLFSSH* ssh;
char* from[NAME_SZ];
char* to[NAME_SZ];
```


```
//set up ssh and do sftp connections
...
```


```
if (wolfSSH_SFTP_Put(ssh, from, to, 0, &myStatusCb) !=
WS_SUCCESS) {
//handle error case
}
```




## SFTPサーバー機能








### wolfSSH_SFTP_read()







**Synopsis:**


**Description:**


着信パケットを処理するメインSFTPサーバー関数。この関数は、I/Oバッファーから読み取ろうとし、SFTPパケットのタイペットに応じて内部関数を呼び出します。


**Return Value**


** WS_SUCCESS：**成功。


**Parameters:**


**ssh**  - 接続に使用されるwolfssh構造へのポインター


**Example:**



```
#include <wolfssh/wolfsftp.h>
int wolfSSH_SFTP_read(WOLFSSH* ssh );
```



**参照：**


wolfSSH_SFTP_accept(), wolfSSH_SFTP_connect()



```
WOLFSSH* ssh;
```


```
//set up ssh and do sftp connections
...
if (wolfSSH_SFTP_read(ssh) != WS_SUCCESS) {
//handle error case
}
```


