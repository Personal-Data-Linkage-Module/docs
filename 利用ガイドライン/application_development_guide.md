# パーソナルデータ連携モジュール アプリケーション開発ガイド

# 改版履歴

| 版数 | 日付 | 改版内容 |
| - | - | - |
| 1.0 | 2022年10月1日  | 初版 |
| 1.1 | 2022年11月30日 | 「1.3 関連ドキュメント」の「表1-1 関連ドキュメント」の記載を変更 |
| 1.2 | 2023年3月30日  | 公開APIリストに記載のないAPIを削除 |

# 目次
<!-- vscode-markdown-toc -->
* 1. [**本文書について**](#)
	* 1.1. [**本文書の目的**](#-1)
	* 1.2. [**前提条件**](#-1)
	* 1.3. [**関連ドキュメント**](#-1)
	* 1.4. [**API入力方法**](#API)
* 2. [**アーキテクチャー説明**](#-1)
	* 2.1. [**本モジュールの構成要素**](#-1)
		* 2.1.1. [**PxR-PF**](#PxR-PF)
		* 2.1.2. [**PxR-Block**](#PxR-Block)
		* 2.1.3. [**PxR-AP**](#PxR-AP)
		* 2.1.4. [**PxR-Service**](#PxR-Service)
	* 2.2. [**PxR-Blockの定義と構成**](#PxR-Block-1)
		* 2.2.1. [**PxR-Blockの定義**](#PxR-Block-1)
		* 2.2.2. [**PxR-Blockの構成**](#PxR-Block-1)
	* 2.3. [**PxR-Block外部からのAPI呼び出し**](#PxR-BlockAPI)
	* 2.4. [**PxR-Block間の協調動作**](#PxR-Block-1)
* 3. [**ソース改造時の留意事項**](#-1)
	* 3.1. [**APIログ出力**](#API-1)
* 4. [**PxR-Service基本仕様**](#PxR-Service-1)
	* 4.1. [**ログインおよびPxR-Service使用方法**](#PxR-Service-1)
	* 4.2. [**オペレーター種別**](#-1)
		* 4.2.1. [**流通制御サービスプロバイダー**](#-1)
		* 4.2.2. [**領域運営サービスプロバイダー**](#-1)
		* 4.2.3. [**アプリケーションプロバイダー**](#-1)
* 5. [**PxR-Service詳細仕様**](#PxR-Service-1)
	* 5.1. [**PxR-Service一覧**](#PxR-Service-1)
	* 5.2. [**PxR-Blockに搭載されるPxR-Service一覧**](#PxR-BlockPxR-Service)
	* 5.3. [**バッチ一覧**](#-1)
	* 5.4. [**PxR-Blockが利用するバッチ一覧**](#PxR-Block-1)
	* 5.5. [**オペレーターサービス**](#-1)
	* 5.6. [**カタログサービス**](#-1)
	* 5.7. [**カタログ更新サービス**](#-1)
	* 5.8. [**PxR-Block-Proxyサービス**](#PxR-Block-Proxy)
	* 5.9. [**通知サービス**](#-1)
	* 5.10. [**Book管理サービス**](#Book)
	* 5.11. [**Book運用サービス**](#Book-1)
	* 5.12. [**本人性確認サービス**](#-1)
	* 5.13. [**CToken台帳サービス**](#CToken)
	* 5.14. [**Local-CTokenサービス**](#Local-CToken)
	* 5.15. [**認証局サービス**](#-1)
	* 5.16. [**証明書管理サービス**](#-1)
	* 5.17. [**アクセス制御管理サービス**](#-1)
	* 5.18. [**アクセス制御サービス**](#-1)
	* 5.19. [**バイナリ管理サービス**](#-1)
	* 5.20. [**利用者作成バッチ**](#-1)
	* 5.21. [**CToken連携バッチ**](#CToken-1)
	* 5.22. [**Region改定再同意依頼バッチ**](#Region)
	* 5.23. [**Book自動閉鎖バッチ**](#Book-1)
	* 5.24. [**Region利用自動終了対象追加バッチ**](#Region-1)
	* 5.25. [**Region利用自動終了バッチ**](#Region-1)
	* 5.26. [**Book削除バッチ**](#Book-1)
	* 5.27. [**出力データファイル管理作成バッチ**](#-1)
	* 5.28. [**利用者データ削除バッチ**](#-1)
	* 5.29. [**Region終了対象追加バッチ**](#Region-1)
	* 5.30. [**Region利用終了バッチ**](#Region-1)
	* 5.31. [**Region利用者連携バッチ**](#Region-1)
* 6. [**PxR-Serviceを利用した実行順例**](#PxR-Service-1)
	* 6.1. [**概要**](#-1)
	* 6.2. [**アクター認定**](#-1)
	* 6.3. [**Region開始**](#Region-1)
	* 6.4. [**Region参加**](#Region-1)
	* 6.5. [**データカタログ作成**](#-1)
	* 6.6. [**My-Condition-Bookの開設**](#My-Condition-Book)
	* 6.7. [**Regionとの利用者ID連携**](#RegionID)
	* 6.8. [**アプリケーションとの利用者ID連携**](#ID)
	* 6.9. [**蓄積の定義**](#-1)
	* 6.10. [**共有の定義**](#-1)
	* 6.11. [**My-Condition-Dataの蓄積**](#My-Condition-Data)
	* 6.12. [**My-Condition-Dataの共有**](#My-Condition-Data-1)
	* 6.13. [**個人の離脱**](#-1)
	* 6.14. [**Region終了**](#Region-1)
	* 6.15. [**アクター終了**](#-1)
	* 6.16. [**【参考】データカタログの取得**](#-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=false
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

##  1. <a name=''></a>**本文書について**
###  1.1. <a name='-1'></a>**本文書の目的**
本文書は、「パーソナルデータ連携モジュール」（以下「本モジュール」といいます。）のアーキテクチャー概要を説明して、アプリケーション開発の拡張開発に必要な考慮事項および仕様・利用例を記載した文書です。アプリケーション開発の拡張開発とは、本モジュールのAPIを組み合わせて拡張APIの開発を行うこと、本モジュールのAPIを使用して独自のUIを開発することを指します。

###  1.2. <a name='-1'></a>**前提条件**
「パーソナルデータ連携モジュール構築ガイド」を前提に、本モジュールを構築していること。

###  1.3. <a name='-1'></a>**関連ドキュメント**
関連ドキュメントを以下に示します。

**表1-1 関連ドキュメント**

| ドキュメント名                                                   |
|------------------------------------------------------------------|
| パーソナルデータ連携モジュール アプリケーション開発ガイド別紙    |
| パーソナルデータ連携モジュール アプリケーション開発ガイドSwagger |
| パーソナルデータ連携モジュール 構築ガイド                        |
| パーソナルデータ連携モジュール 利用設定手順書                    |

###  1.4. <a name='API'></a>**API入力方法**

（例）：APIパス

| サービス名称     | API名称 | メソッドおよびパス      |
|------------------|---------|-------------------------|
| カタログサービス | 更新    | PUT /catalog/ext/{code} |

APIパス使用例：https://\<ドメイン\>/catalog/catalog/ext/{code}

※ドメインの後ろにAPIパスをつなげて使用する。

（例）：APIレスポンス

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p>{</p>
<p>"catalogItem": {</p>
<p>"ns":
"catalog/ext/{ext_name}/setting/actor-own/{actor_type}/actor_{actor_code}",</p>
<p>"name": "設定の名称",</p>
<p>"_code": {</p>
<p>"_value": 対象のコード,</p>
<p>"_ver": 対象のバージョン</p>
<p>},</p>
<p>"inherit": {</p>
<p>"_value": 継承元カタログコード,</p>
<p>"_ver": 継承元カタログバージョン</p>
<p>},</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

※PxR-Root-Block以外のPxR-BlockでPxR-Root-BlockのAPIを使用する場合は、proxyを経由するため以下のパスになります。

PUT /pxr-block-proxy/&block=$pxr-root-block&path=/catalog/ext/{code}

　　

　　※リクエスト・レスポンス例内に //
の後にコメントを付記している場合があります。実行の際は削除してください。

##  2. <a name='-1'></a>**アーキテクチャー説明**
本モジュールのアーキテクチャーについて記載します。この章では拡張開発に必要な部分を抜粋して記載します。

###  2.1. <a name='-1'></a>**本モジュールの構成要素**
AWSサービス利用を前提とした場合、本モジュールの構成要素は以下の通りです。

<img src="media/image3.png" style="width:5.94722in;height:4.90694in" />

**図2-1. 本モジュールの構成図**

####  2.1.1. <a name='PxR-PF'></a>**PxR-PF**
PxR-PFは、本モジュールを動作させるプラットフォームを指します。
本モジュールのプラットフォームは、AWSサービスを使用する前提としています。

####  2.1.2. <a name='PxR-Block'></a>**PxR-Block**
PxR-Blockは、本モジュールを利用する組織（以下「アクター」といいます。）が活動する際に使用するサブシステムを指します。

####  2.1.3. <a name='PxR-AP'></a>**PxR-AP**
PxR-APは、アクターが活動する際にアプリケーションを指します。後述のPxR-Serviceがアクターの種類に応じて搭載されます。

####  2.1.4. <a name='PxR-Service'></a>**PxR-Service**
PxR-Serviceは、PxR-APで動作するマイクロサービスを指します。
PxR-Serviceの一覧は、6.1 PxR-Service一覧を参照してください。

###  2.2. <a name='PxR-Block-1'></a>**PxR-Blockの定義と構成**

####  2.2.1. <a name='PxR-Block-1'></a>**PxR-Blockの定義**
アクターにつき1つのPxR-Blockが割り当てられます。アクターを追加する際は、PxR-Blockの追加が必要です。PxR-Blockの追加方法は、別紙『パーソナルデータ連携モジュール利用設定手順書』の「3 PxR-Blockの追加方法」を参照してください。
PxR-Block内のPxR-Serviceを利用してアクセスすることで、各種業務・活動が可能です。
アクター毎の利用者（以下「オペレーター」といいます。）が活動するPxR-Blockは以下の通りです。

-  流通制御サービスプロバイダー（流通制御SP）: PxR-Root-Block
-  個人: PxR-Root-Block（※）
-  領域運営サービスプロバイダー（領域運営SP）: Region-Root-Block
-  アプリケーションプロバイダー（アプリケーションP）: APP-Block

本モジュールではKubernetesを前提とし、Kubernetesの1Serviceが1PxR-Blockに当たります。
（※）個人はPxR-Root-Blockを利用しますが、個人に対してPxR-Blockが払い出されることはありません。

####  2.2.2. <a name='PxR-Block-1'></a>**PxR-Blockの構成**
PxR-BlockはKubernetesの以下の要素を使用して、構成されています。
PxR-BlockはKubernetes Serviceが該当し、
PxR-APはPxR-Blockの配下としてKubernetes Podが該当します。  
PxR-ServiceはKubernetes Containerとして、
オペレーターサービス、通知サービス、NGINX等が搭載され、PxR-AP内に複数存在します。
PxR-BlockとPxR-APは、アクターが行う業務に合わせたPxR-Serviceを搭載しています。共通で搭載されるサービスもあり、オペレーターの登録・認証を担うオペレーターサービスや、Block間通信のラッパーとなるPxR-Block-Proxyサービスなどが該当します。

<img src="media/image4.png" style="width:4.70899in;height:3.43798in"
alt="ダイアグラム 自動的に生成された説明" />

**図2-2 PxR-Blockで使用するKubernetesの要素**

KubernetesはAWS EKSを、DBはEKS外部のマネージドDB（Amazon Aurora
PostgreSQL）を利用する前提としています。DBへの接続は、DBアクセス用の共通Kubernetes
Serviceを使用します。

以下でアクター毎のPxR-Blockの構成について、流通制御サービスプロバイダー（PxR-Root-Block）の例を挙げて説明します。

流通制御サービスプロバイダーの場合

<img src="media/image5.png" style="width:6.69306in;height:2.90347in"
alt="ダイアグラム 自動的に生成された説明" />

**図2-3 PxR-Blockの構成（流通制御サービスプロバイダー）**

NGINXはPxR-Serviceとしてカスタマイズしておらず、公開されているWebサーバを使用しています。詳細は、4章コンテナ一覧をご覧ください。外部からのアクセスはすべてNGINXを経由する前提としています。

###  2.3. <a name='PxR-BlockAPI'></a>**PxR-Block外部からのAPI呼び出し**
外部からアクセスできるサービスは、オペレーターサービスおよびPxR-Block-Proxyサービスに限定しています。各ビジネスロジックサービスでの認証処理の実装漏れをなくすために、認証処理をオペレーターサービスに、認可処理をPxR-Block-Proxyに集中させています。
ここでは流通制御サービスプロバイダーと個人それぞれが、ポータルを介しログイン後、カタログ参照画面にてバイタル（体温等）に関するデータカタログを参照する際の通信順序を示します。

流通制御サービスプロバイダー 運営メンバーの場合

<img src="media/image8.png" style="width:6.69306in;height:2.84722in"
alt="ダイアグラム, タイムライン 自動的に生成された説明" />

**図2-5 PxR-Block外部からのAPI呼び出し（流通制御サービスプロバイダー）**

（※1）ログイン成功時、API側ではセッションIDを払い出します。ログイン後のセッション確認はセッションAPIを用いる必要があり、各API呼び出し時にも必ずセッション確認が入ります。

個人の場合

<img src="media/image9.png" style="width:6.69306in;height:2.77778in"
alt="ダイアグラム, タイムライン 自動的に生成された説明" />

**図2-6 PxR-Block外部からのAPI呼び出し（個人）**

（※1）ログイン成功時、API側ではセッションIDを払い出します。ログイン後のセッション確認はセッションAPIを用いる必要があり、各API呼び出し時にも必ずセッション確認が入ります。
（※2）ワンタイムログインコードは、個人ポータルに限り以下の条件を満たす場合に発行されるログイン時2段階認証用の6桁の数字を指します。
　・該当のPxRインスタンスでSMS認証が使用される設定となっていること
　・個人がSMS認証を使用する設定を行い、携帯電話番号が登録されていること
　ワンタイムログインコードは、通常のパスワードでのログイン後、個人に紐づく携帯電話にSMSで送信されます。ワンタイムログインコード入力用画面に遷移後、SMSの6桁の数字を入力することでログインが完了します。

###  2.4. <a name='PxR-Block-1'></a>**PxR-Block間の協調動作**
Block間でのAPI呼び出しは、有効期限を定めたAPIトークンを使用し認可を行います。
PxR-Block-ProxyサービスがAPIトークンの発行と照合を担っています。

<img src="media/image10.png" style="width:6.64583in;height:2.75847in" />

**図2-7 PxR-Block間での協調動作**

##  3. <a name='-1'></a>**ソース改造時の留意事項**
本モジュールのソースを改造する際の留意事項を記載します。

###  3.1. <a name='API-1'></a>**APIログ出力**
APIログ出力は、PxR-Serviceのcommon内にあるlogging.tsのapplicationLoggerを使用しています。ログレベルは以下で設定しており、ログファイルにはinfoレベル以上が出力されます。以下低いレベル順に列挙します。
- debug: 開発時に使用するログレベルです
- info: 処理完了等の通常のログ出力に使用するログレベルです
- warn: 対象データがない等警告する際に使用するログレベルです
- error: エラーが発生した際に使用するログレベルです
- fatal: 致命的なエラーが発生した場合に使用するログレベルです

##  4. <a name='PxR-Service-1'></a>**PxR-Service基本仕様**
PxR-ServiceおよびそのAPIを使用する際の基本仕様を記載します。

###  4.1. <a name='PxR-Service-1'></a>**ログインおよびPxR-Service使用方法**
オペレーターサービスとPxR-Block-Proxyサービスを利用して、本モジュールを使用します。
ログインの流れは、別紙『アプリケーション開発ガイド別紙』の「4.1
ログインおよび各PxR-Service使用方法」を参照してください。

###  4.2. <a name='-1'></a>**オペレーター種別**
オペレーターには4つの種別が存在し、アクター毎に作成されるオペレーターの種別が異なります。
種別は以下の通り分類されます。
説明列に記載のIDは、5. PxR-Service詳細仕様、6.
PxR-Serviceを利用した実例で詳細に解説します。

####  4.2.1. <a name='-1'></a>**流通制御サービスプロバイダー**

**表4-1 流通制御サービスプロバイダーのオペレーター一覧**

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 39%" />
<col style="width: 37%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>オペレーター種別</strong></th>
<th><strong>用途</strong></th>
<th><strong>ID説明</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>運営メンバー</td>
<td>本モジュール全体や個人の管理に関する業務を行います。次節で説明する権限は、運営メンバーにのみ存在します。</td>
<td><p>オペレーターID:
オペレーター作成時にシステムが自動で発行するオペレーターを一意に判別できる整数値です。</p>
<p>ログインID:
オペレーター作成時にオペレーターの作成者が指定するログインに使用する文字列です。オペレーターIDとは1:1で紐づきます。</p></td>
</tr>
<tr class="even">
<td>個人</td>
<td>蓄積したデータや共有履歴の閲覧、データ提供同意を行うことができます。個人はBook開設と同時にオペレーターが作成されます。Bookと個人オペレーターは1:1で紐づきます。</td>
<td><p>オペレーターID:
Book開設時にシステムが自動で発行するオペレーターを一意に判別できる整数値です。</p>
<p>ログインID:
Book開設時にBook開設者が指定するログインに使用する文字列です。オペレーターIDとは1:1で紐づきます。</p>
<p>PxR-ID:
Book開設時にBook開設者が指定するBookを一意に判別するための文字列です。オペレーターIDとは1:1で紐づきます。</p></td>
</tr>
</tbody>
</table>

####  4.2.2. <a name='-1'></a>**領域運営サービスプロバイダー**

**表4-2 領域運営サービスプロバイダーのオペレーター一覧**

| **オペレーター種別** | **用途**                                                                                                                                                       | **ID説明**                                                                      |
|-----------------|----------------------------|----------------------------|
| 運営メンバー         | アプリケーションプロバイダーの提供するアプリケーションを束ねる領域（Region）の管理に関する業務を行います。次節で説明する権限は、運営メンバーにのみ存在します。 | 表4-1 流通制御サービスプロバイダーのオペレーター一覧 運営メンバーのID説明欄参照 |

####  4.2.3. <a name='-1'></a>**アプリケーションプロバイダー**

**表4-3 アプリケーションプロバイダーのオペレーター一覧**

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 39%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>オペレーター種別</strong></th>
<th><strong>用途</strong></th>
<th><strong>ID説明</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>運営メンバー</td>
<td>アプリケーションの管理や個人利用者の管理に関する業務を行います。次節で説明する権限は、運営メンバーにのみ存在します。</td>
<td>表4-1 流通制御サービスプロバイダーのオペレーター一覧
運営メンバーのID説明欄参照</td>
</tr>
<tr class="even">
<td>アプリケーション</td>
<td>アプリケーションで発生した個人利用者のデータ蓄積や、他のアプリケーションとデータの相互共有を行うことができます。アプリケーションとアプリケーションオペレーターは、1:nで紐づきます。</td>
<td>表4-1 流通制御サービスプロバイダーのオペレーター一覧
運営メンバーのID説明欄参照</td>
</tr>
<tr class="odd">
<td>個人</td>
<td>データを蓄積するための利用者として作成されます。アプリケーションプロバイダーのPxR-Blockにログインすることはできません。個人は利用者作成と同時にオペレーターが作成されます。利用者と個人オペレーターは1:1で紐づきます。</td>
<td><p>オペレーターID:
利用者作成時にシステムが自動で発行するオペレーターを一意に判別できる整数値です。</p>
<p>利用者ID:
利用者の作成手順内で利用者を作成するオペレーターが指定する利用者を一意に判別するための文字列です。オペレーターIDとは1:1で紐づきます。</p></td>
</tr>
</tbody>
</table>

##  5. <a name='PxR-Service-1'></a>**PxR-Service詳細仕様**
PxR-ServiceとAPIの一覧を掲載します。詳細仕様は、Open API
Specを参照してください。

###  5.1. <a name='PxR-Service-1'></a>**PxR-Service一覧**
PxR-Serviceの一覧は以下の表5-1の通りです。

**表5-1 PxR-Service一覧**

| **サービス名**           | **説明**                                                                                        |
|------------------------|------------------------------------------------|
| オペレーターサービス     | オペレーター（運営メンバー、アプリケーション、個人）の管理と認証を行います。                    |
| カタログサービス         | My-Condition-Dataカタログの管理を行います。                                                     |
| カタログ更新サービス     | My-Condition-Dataカタログのうち、承認処理等追加で特殊な手順が必要なカタログの管理を行います。   |
| PxR-Block-Proxyサービス  | PxR-Blockのサービスへ各種メソッドでのProxyを行います。                                          |
| 通知サービス             | オペレーターへ通知や承認要求を行います。                                                        |
| Book管理サービス         | My-Condition-Bookの管理を行います。                                                             |
| Book運用サービス         | My-Condition-Bookへデータの蓄積と共有を行います。                                               |
| 本人性確認サービス       | 登録された本人性確認情報を使用して本人性確認を行います。                                        |
| CToken台帳サービス       | CToken台帳の管理を行います。                                                                    |
| Local-CTokenサービス     | Local CTokenをCToken台帳へ連携します。                                                          |
| 認証局サービス           | 証明書の生成や取得、検証を行います。                                                            |
| 証明書管理サービス       | 証明書の保存を行います。                                                                        |
| アクセス制御管理サービス | PxR-Block間通信のAPIトークンを生成する際の動的人を行います。                                    |
| アクセス制御サービス     | リクエストの認可に必要なAPIトークンの発行、管理を行います。                                     |
| バイナリ管理サービス     | バイナリファイルのアップロード、ダウンロードを行います。                                        |
| NGINX                    | PxR-Serviceへのプロキシを行います。UIを独自開発した際は、設定ファイルを変更する必要があります。 |

###  5.2. <a name='PxR-BlockPxR-Service'></a>**PxR-Blockに搭載されるPxR-Service一覧**

PxR-Blockに搭載されるPxR-Serviceの一覧は以下の表5-2の通りです。

**表5-2 PxR-Service搭載一覧**

| **サービス名**           | PxR-Root-Block | Region-Root-Block | APP-Block |
|--------------------------|----------------|-------------------|-----------|
| オペレーターサービス     | ○              | ○                 | ○         |
| カタログサービス         | ○              |                   |           |
| カタログ更新サービス     | ○              |                   |           |
| PxR-Block-Proxyサービス  | ○              | ○                 | ○         |
| 通知サービス             | ○              | ○                 | ○         |
| Book管理サービス         | ○              |                   |           |
| Book運用サービス         |                | ○                 | ○         |
| 本人性確認サービス       | ○              |                   |           |
| CToken台帳サービス       | ○              |                   |           |
| Local-CTokenサービス     |                |                   | ○         |
| 認証局サービス           | ○              |                   |           |
| 証明書管理サービス       | ○              | ○                 | ○         |
| アクセス制御管理サービス | ○              |                   |           |
| アクセス制御サービス     | ○              | ○                 | ○         |
| バイナリ管理サービス     |                |                   | ○         |

###  5.3. <a name='-1'></a>**バッチ一覧**
バッチの一覧は以下の表5-3の通りです。
バッチ処理については、開発者側で開発が必要になります。バッチ概要は本紙に、またシーケンス図が別紙にあるので、参照し開発してください。

**表5-3バッチ一覧**

| **バッチ名**                     | **説明**                                                                     |
|------------------------|------------------------------------------------|
| 利用者作成バッチ                 | Rootで連携済の利用者を作成します。                                           |
| CToken連携バッチ                 | Local-CTokenをCToken台帳へ連携します。                                       |
| Region改定再同意依頼バッチ       | Region規約再同意データを取得し、各個人へ再同意依頼を行います。               |
| Book自動閉鎖バッチ               | PF規約の再同意が得られなかった個人のBookを自動停止します。                   |
| Region利用自動終了対象追加       | Region規約の再同意が得られなかった個人のRegion利用を自動終了の対象とします。 |
| Region利用自動終了バッチ         | 個人のRegion利用を自動終了します。                                           |
| Book削除バッチ                   | Book削除フラグに基づいてMy-Condition-Bookを削除します。                      |
| 出力データファイル管理作成バッチ | データ出力指定に基づいて、出力データ管理作成を作成します。                   |
| 利用者データ削除バッチ           | 利用者データを削除し、処理結果をRoot-Blockに通知します。                     |
| Region終了対象追加               | 終了承認されたRegionを使用している個人を自動終了の対象とします。             |
| Region利用終了                   | 終了承認されたRegion利用を終了します。                                       |
| Region利用者連携                 | 申請中のRegion利用者連携を連携中にします。                                   |

###  5.4. <a name='PxR-Block-1'></a>**PxR-Blockが利用するバッチ一覧**
PxR-Blockが利用するバッチの一覧は以下の表5-4の通りです。

**表5-4バッチ搭載一覧**

| **バッチ名**                     | PxR-Root-Block | Region-Root-Block | APP-Block |
|------------------------|---------------|-----------------|-----------------|
| 利用者作成バッチ                 |                |                   | ○         |
| CToken連携バッチ                 |                |                   | ○         |
| Region改定再同意依頼バッチ       | ○              |                   |           |
| Book自動閉鎖バッチ               | ○              |                   |           |
| Region利用自動終了対象追加       | ○              |                   |           |
| Region利用自動終了バッチ         | ○              |                   |           |
| Book削除バッチ                   | ○              |                   |           |
| 出力データファイル管理作成バッチ | ○              |                   |           |
| 利用者データ削除バッチ           |                |                   | ○         |
| Region終了対象追加               | ○              |                   |           |
| Region利用終了                   | ○              |                   |           |
| Region利用者連携                 |                | ○                 |           |

###  5.5. <a name='-1'></a>**オペレーターサービス**
オペレーター（運営メンバー、アプリケーション、個人）の管理と認証を行います。
APIの詳細は、別紙『オペレーターサービスOpenAPI.json』を参照してください。

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 41%" />
<col style="width: 39%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>API名称</strong></th>
<th><strong>概要</strong></th>
<th><strong>メソッドおよびパス</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>追加</td>
<td>オペレーターを追加します。</td>
<td>POST /operator</td>
</tr>
<tr class="even">
<td>更新</td>
<td>オペレーターを更新します。</td>
<td>PUT /operator/{operatorId}</td>
</tr>
<tr class="odd">
<td>削除</td>
<td>オペレーターを削除します。</td>
<td>DELETE /operator/{operatorId}</td>
</tr>
<tr class="even">
<td>種別による取得</td>
<td>種別（+ログインID）を指定してオペレーター情報を取得します。</td>
<td>GET /operator</td>
</tr>
<tr class="odd">
<td>IDによる取得</td>
<td>オペレーターIDを指定してオペレーター情報を取得します。</td>
<td>GET /operator/{operatorId}</td>
</tr>
<tr class="even">
<td>ログイン</td>
<td>個人以外のオペレーターでログインします。SMS認証を利用する場合、ワンタイムログインコードを生成しSMSでログイン者へ送付します。それ以外の場合は、オペレーターをログインさせます。</td>
<td>POST /operator/login</td>
</tr>
<tr class="odd">
<td>個人用ログイン</td>
<td>個人オペレーターでログインします。<br />
SMS認証を利用する場合、ワンタイムログインコードを生成しSMSで個人へ送付します。それ以外の場合は、個人オペレーターをログインさせます。</td>
<td>POST /operator/ind/login</td>
</tr>
<tr class="even">
<td>透過ログイン</td>
<td>個人以外のオペレーターでIDコネクト連携を使用してログインします。</td>
<td>POST /operator/login/sso</td>
</tr>
<tr class="odd">
<td>ワンタイムログインコード照合</td>
<td>ワンタイムログインコードの照合に成功したら、オペレーターをログインさせます。</td>
<td>POST /operator/login/onetime</td>
</tr>
<tr class="even">
<td>個人用透過ログイン</td>
<td>個人オペレーターでIDコネクト連携を使用してログインします。</td>
<td>POST /operator/ind/login/sso</td>
</tr>
<tr class="odd">
<td>ログアウト</td>
<td>セッションIDを受け、ログイン中のオペレーターをログアウトさせます。</td>
<td>POST /operator/logout</td>
</tr>
<tr class="even">
<td>個人用ログアウト</td>
<td>セッションIDを受け、ログイン中の個人オペレーターをログアウトさせます。</td>
<td>POST /operator/ind/logout</td>
</tr>
<tr class="odd">
<td>セッション確認</td>
<td>セッションIDまたはIDコネクトサービスのアクセストークンを受け、有効か確認します。有効な場合、オペレーター情報を返します。</td>
<td>POST /operator/session</td>
</tr>
<tr class="even">
<td>個人用セッション確認</td>
<td>セッションIDまたはIDコネクトサービスのアクセストークンを受け、有効か確認します。有効な場合、オペレーター情報を返します。</td>
<td>POST /operator/ind/session</td>
</tr>
<tr class="odd">
<td>パスワードリセット</td>
<td>オペレーターのパスワードをリセットします。</td>
<td>PUT /operator/password/{operatorId}</td>
</tr>
<tr class="even">
<td>個人用ワンタイムログインコード照合</td>
<td>ワンタイムログインコードの照合に成功したら、個人オペレーターをログインさせます。</td>
<td>POST /operator/ind/login/onetime</td>
</tr>
<tr class="odd">
<td>本人性確認コード登録</td>
<td>本人性確認コードを登録します。</td>
<td>POST /operator/ind/identifyCode</td>
</tr>
<tr class="even">
<td>利用者管理情報登録</td>
<td>個人オペレーターの利用者管理情報の登録・更新します。</td>
<td>POST /operator/user/info</td>
</tr>
<tr class="odd">
<td>利用者管理情報削除</td>
<td>個人オペレーターの利用者管理情報を削除します。</td>
<td>DELETE /operator/user/info</td>
</tr>
<tr class="even">
<td>個人による利用者管理情報修正</td>
<td>自身による個人オペレーターの利用者管理情報の登録・更新します。</td>
<td>PUT /operator/ind/user/info</td>
</tr>
<tr class="odd">
<td>利用者管理情報取得</td>
<td>指定したPXR-ID、利用者IDの利用者管理情報を取得します。</td>
<td>GET /operator/user/info</td>
</tr>
<tr class="even">
<td>個人用利用者管理情報取得</td>
<td>指定したPXR-ID、利用者IDの利用者管理情報を取得します。</td>
<td>GET /operator/ind/user/info</td>
</tr>
<tr class="odd">
<td>利用者管理情報リスト取得</td>
<td>指定したPXR-ID、利用者IDの利用者管理情報を複数取得します。</td>
<td>POST /operator/user/info/list</td>
</tr>
<tr class="even">
<td>利用者管理情報によるPXR-ID取得</td>
<td>検索条件に合致する利用者管理情報を持つ個人オペレーターのPXR-IDを取得します。</td>
<td>POST /operator/user/info/search</td>
</tr>
<tr class="odd">
<td>SMS検証コード発行</td>
<td>電話番号を受け、SMS検証コードを発行しSMSで通知します。</td>
<td>POST /operator/ind/sms-verificate</td>
</tr>
<tr class="even">
<td>SMS検証コード検証</td>
<td>検証コードIDコードおよびSMS検証コードを受け、検証結果を返却します。</td>
<td>POST /operator/ind/sms-verificate/verifiy</td>
</tr>
</tbody>
</table>

###  5.6. <a name='-1'></a>**カタログサービス**

My-Condition-Dataカタログの管理を行います。

APIの詳細は、別紙『カタログサービスOpenAPI.json』を参照してください。

| **API名称**                                | **概要**                                                                                                           | **メソッドおよびパス**                  |
|-------------|-------------------------------|-----------------------------|
| カタログ名称更新                           | カタログ名と説明、拡張ネームスペースを指定して、使用開始済のカタログの名称、説明、拡張ネームスペースを更新します。 | PUT /catalog/name                       |
| カタログ名称取得                           | 使用開始済のカタログの名称と説明、拡張ネームスペースを取得します。                                                 | GET /catalog/name                       |
| 公開カタログ取得                           | 公開が許可されているカタログ項目を取得します。                                                                     | GET /catalog/public                     |
| カタログ取得(code指定)                     | カタログ項目コードを指定して、最新バージョンのカタログ項目を取得します。                                           | GET /catalog/{code}                     |
| カタログ取得(code,version指定)             | カタログ項目コードとバージョンを指定して、カタログ項目を取得します。                                               | GET /catalog/{code}/{ver}               |
| カタログ取得(複数code指定)                 | 複数のコードを指定して、カタログ項目を取得します。                                                                 | POST /catalog/                          |
| ネームスペースによる取得                   | ネームスペースを指定して、カタログ項目を取得します。                                                               | GET /catalog/                           |
| 内部クラス取得(code指定)                   | カタログ項目コードと内部クラス名を指定して、最新バージョンのカタログ項目内部クラスを取得します。                   | GET /catalog/inner/{code}/{name}        |
| 内部クラス取得(code,version指定)           | カタログ項目コードとバージョン、内部クラス名を指定して、カタログ項目内部クラスを取得します。                       | GET /catalog/inner/{code}/{ver}/{name}  |
| ネームスペース取得                         | ネームスペースIdを指定して、ネームスペースを取得します。                                                           | GET /catalog/ns/{nsId}                  |
| ネームスペースを指定してネームスペース取得 | ネームスペースを指定して、前方一致のネームスペースを取得します。                                                   | GET /catalog/ns                         |
| 拡張用ネームスペース追加                   | PXRインスタンス毎に作成される拡張用のネームスペースを追加します。                                                  | POST /catalog/ns/ext                    |
| 拡張用ネームスペース更新                   | PXRインスタンス毎に作成された拡張用のネームスペースを更新します。                                                  | PUT /catalog/ns/ext/{nsId}              |
| 拡張用ネームスペース削除                   | PXRインスタンス毎に作成された拡張用のネームスペースを削除します。                                                  | DELETE /catalog/ns/ext/{nsId}           |
| 拡張追加                                   | PXRインスタンス毎に作成される拡張カタログ項目を追加します。                                                        | POST /catalog/ext                       |
| 拡張更新                                   | PXRインスタンス毎に作成された拡張カタログ項目を更新します。                                                        | PUT /catalog/ext/{code}                 |
| 拡張削除                                   | PXRインスタンス毎に作成された拡張カタログ項目を削除します。                                                        | DELETE /catalog/ext/{code}              |
| 変更セット情報取得                         | 自身の変更セットリストを取得します。                                                                               | GET /catalog/updateSet/search/info/{id} |
| 未承認変更セットリスト取得                 | 自身の未承認変更セットリストを取得します。                                                                         | GET /catalog/updateSet/search/request   |
| 承認済変更セットリスト取得                 | 全ての承認済み変更セットリストを取得します。                                                                       | GET /catalog/updateSet/search/approval  |
| 変更セット登録                             | 変更セット(追加、更新、削除)を登録します。                                                                         | POST /catalog/updateSet/register        |
| 変更セット登録変更                         | 変更セット(追加、更新、削除)を登録変更します。                                                                     | PUT /catalog/updateSet/register/{id}    |
| 変更セット登録削除                         | 変更セット(追加、更新、削除)を削除します。                                                                         | DELETE /catalog/updateSet/register/{id} |
| 変更セット申請                             | 変更セット(追加、更新、削除)を申請します。                                                                         | POST /catalog/updateSet/request         |
| 変更セット承認                             | 変更セット(追加、更新、削除)を承認・否認します。                                                                   | POST /catalog/updateSet/approval/{id}   |

###  5.7. <a name='-1'></a>**カタログ更新サービス**
My-Condition-Dataカタログのうち、承認処理等追加で特殊な手順が必要なカタログの管理を行います。
APIの詳細は、別紙『カタログ更新サービスOpenAPI.json』を参照してください。

| **API名称**                   | **概要**                                                                                                                                                   | **メソッドおよびパス**                             |
|-----------|--------------------------------|------------------------------|
| 申請先取得                    | 申請するアクター種別を受け、その種別の認定を行っているアクター一覧を返却します。申請前はクライアント証明書がないため、申請するアクターの検証は行いません。 | GET /catalog-update/actor/accreditor               |
| アクター認定/認定解除申請取得 | 認定申請を取得します。申請前はクライアント証明書がないため、申請するアクターの検証は行いません。                                                           | GET /catalog-update/actor                          |
| アクター認定申請              | 拡張カタログ項目(アクター)を受け、認定を行うアクターへ承認を要求します。申請前はクライアント証明書がないため、申請するアクターの検証は行いません。         | POST /catalog-update/actor                         |
| アクター認定解除申請          | 認定解除の申請を行います。                                                                                                                                 | POST /catalog-update/actor/remove                  |
| Region参加/離脱申請取得       | Region参加申請を取得します。                                                                                                                               | GET /catalog-update/join                           |
| Region参加申請                | Region参加要求を登録します。                                                                                                                               | POST /catalog-update/join                          |
| Region離脱申請                | Region離脱要求を登録します。                                                                                                                               | POST /catalog-update/join/remove                   |
| Region作成/削除管理取得       | Region作成/削除管理を取得します。                                                                                                                          | GET / catalog-update/region                        |
| Region作成                    | Regionを作成します。                                                                                                                                       | POST /catalog-update/region                        |
| Region削除                    | Regionを削除します。                                                                                                                                       | POST /catalog-update/region/delete                 |
| Region開始終了申請取得        | Region開始終了申請を取得します。                                                                                                                           | GET /catalog-update/region/status                  |
| Region開始要求                | Regionを開始するための要求を登録します。                                                                                                                   | POST /catalog-update/region/status/start           |
| Region終了要求                | Regionを終了するための要求を登録します。                                                                                                                   | POST /catalog-update/region/status/end             |
| Region開始終了承認結果登録    | 承認結果を受け、双方の運営メンバーへ結果を通知します。                                                                                                     | POST /catalog-update/region/status/approval/{code} |
| PF利用規約作成                | 利用規約情報を受け、利用規約を作成します。                                                                                                                 | POST /catalog-update/platform-terms-of-use         |
| PF利用規約更新                | 利用規約情報を受け、利用規約を更新します。再同意が必要な場合、個人へ同意を要求します。                                                                     | PUT /catalog-update/platform-terms-of-use          |
| Region利用規約作成            | 利用規約情報を受け、利用規約を作成します。                                                                                                                 | POST /catalog-update/region-terms-of-use           |
| Region利用規約更新            | 利用規約情報を受け、利用規約を更新します。再同意が必要な場合、個人へ同意を要求します。                                                                     | PUT /catalog-update/region-terms-of-use            |
| 蓄積イベント通知定義作成      | 蓄積イベント通知定義テーブルを更新します。                                                                                                                 | POST /catalog-update/store-event                   |
| 蓄積イベント通知定義更新      | 蓄積イベント通知定義テーブルを更新します。                                                                                                                 | PUT /catalog-update/store-event                    |
| 変更セット申請                | 変更セットを申請します。                                                                                                                                   | POST /catalog-update/updateSet/request             |
| 変更セット承認                | 変更セットを承認・否認します。                                                                                                                             | POST /catalog-update/updateSet/approval/{id}       |

###  5.8. <a name='PxR-Block-Proxy'></a>**PxR-Block-Proxyサービス**
PxR-Blockのサービスへ各種メソッドでのProxyを行います。
APIの詳細は、別紙『PxR-Block-ProxyサービスOpenAPI.json』を参照してください。

| **API名称**      | **概要**                                         | **メソッドおよびパス**                   |
|-------------|------------------------------|-----------------------------|
| プロキシー       | 対象のBlockにある本サービスへ透過アクセスするAPI | GET/POST/DELETE/PUT /pxr-block-proxy.    |
| 個人用プロキシー | プロキシーAPIの個人用                            | GET/POST/DELETE/PUT /pxr-block-proxy/ind |

###  5.9. <a name='-1'></a>**通知サービス**
オペレーターへ通知や承認要求を行います。
APIの詳細は、別紙『通知サービスOpenAPI.json』を参照してください。

| **API名称** | **概要**                                                     | **メソッドおよびパス**     |
|------------|--------------------------------|-----------------------------|
| 登録        | 通知を登録する。必要があれば他Blockへの連携などを行います。  | POST /notification         |
| 取得        | 登録されている通知を、そのリクエストの条件を元に取得します。 | GET /notification          |
| 既読        | 登録された通知に対し、既読フラグをつけます。                 | PUT /notification          |
| 承認        | 登録された承認要求タイプの通知に対し、否認や承認を行います。 | PUT /notification/approval |

###  5.10. <a name='Book'></a>**Book管理サービス**
My-Condition-Bookの管理を行います。
APIの詳細は、別紙『Book管理サービスOpenAPI.json』を参照してください。

| **API名称**                     | **概要**                                                                                                                                                                                                          | **メソッドおよびパス**                             |
|-------------|-----------------------------|-------------------------------|
| 共有アクセス取得依頼            | ログインしている個人の共有アクセスログを取得します。                                                                                                                                                              | POST /book-manage/accesslog                        |
| Book開設                        | 本人性確認事項を受け、individualとPXRログインコードを作成し、PXRログインコードを返却します。本人性確認事項でMy-Condition-Bookの存在が確認できないこと。IDコネクトサービスを使用する場合、属性更新を呼び出します。 | POST /book-manage                                  |
| PXR-ID重複チェック              | すでに存在するPXR-IDがないかチェックして結果を返却します。                                                                                                                                                        | POST /book-manage/check/pxr-id                     |
| 本人性確認書類重複チェック      | 本人性確認書類を受け、既に存在する本人性確認書類かチェックして結果を返却します。                                                                                                                                  | POST /book-manage/check/identification             |
| 強制削除                        | 流通制御により、My-Condition-Bookを強制的に削除します。                                                                                                                                                           | DELETE /book-manage/force/{PXR-ID}                 |
| Book閉鎖                        | My-Condition-Bookを閉鎖し、オペレータを削除します。                                                                                                                                                               | DELETE /book-manage.                               |
| データ出力準備                  | My-Condition-Data出力コードを登録します。                                                                                                                                                                         | POST /book-manage/output/condition/prepare         |
| My-Condition-Book一覧取得       | My-Condition-Bookの一覧を取得します。                                                                                                                                                                             | POST /book-manage/search                           |
| 本人性確認事項取得              | 個人が本人性確認事項を取得します。                                                                                                                                                                                | GET /book-manage/identification                    |
| 本人性確認事項検索              | 本人性確認事項を取得します。                                                                                                                                                                                      | POST /book-manage/identity                         |
| パスワード再作成                | PXR-IDを受け、PXRログインコードの再度作成します。再作成条件は、Book開設権限のあるオペレーターからの申請で対象のオペレーターの電話番号が設定されていること。                                                       | POST /book-manage/login-code                       |
| CToken件数取得                  | 個人が蓄積したデータ種と量を取得する。                                                                                                                                                                            | POST /book-manage/ctoken                           |
| BOOK参照依頼                    | PXR-IDを受け、個人に紐づくBOOKを取得します。                                                                                                                                                                      | POST /book-manage/book/search                      |
| 利用者ID連携                    | 本人性確認コード照合を呼出し、PXR-IDと利用者IDを連携します。利用者ID連携条件は、本人性確認が成功していること。                                                                                                    | POST /book-manage/cooperate                        |
| 利用者ID連携取得                | ログイン情報のPXR-IDをもとに、その個人が連携している情報を返却します。                                                                                                                                            | GET /book-manage/cooperate                         |
| データ蓄積定義追加              | データ蓄積定義をindividualに追加します。                                                                                                                                                                          | POST /book-manage/settings/store                   |
| データ蓄積定義削除              | individualからデータ蓄積定義を削除します。                                                                                                                                                                        | DELETE /book-manage/settings/store/{storeId}       |
| データ蓄積定義取得              | individualからデータ蓄積定義を取得します。                                                                                                                                                                        | GET /book-manage/settings/store/{id}               |
| データ共有定義追加              | データ共有定義をindividualに追加します。                                                                                                                                                                          | POST /book-manage/setting/share                    |
| データ共有定義削除              | individualからデータ共有定義を削除します。                                                                                                                                                                        | DELETE /book-manage/setting/share/{shareId}        |
| データ共有定義取得              | データ共有定義を取得します。                                                                                                                                                                                      | GET /book-manage//setting/share                    |
| 一時的データ共有コード生成      | 一時的データ共有定義をindividualに追加し、一時的データ共有コードを生成します。                                                                                                                                    | POST /book-manage/share/temp                       |
| 一時的データ共有コード照合      | 一時的データ共有コードを受け照合します。                                                                                                                                                                          | POST /book-manage/share/temp/collation             |
| 一時的データ共有コード取得      | 一時的データ共有コードを取得します。                                                                                                                                                                              | GET /book-manage/share/temp                        |
| 利用者情報登録                  | PXR-IDが一致する利用者情報を更新します。                                                                                                                                                                          | POST /book-manage/user/info                        |
| ガイダンス送信                  | 初回ログイン用のSMSを送信します。                                                                                                                                                                                 | POST /book-manage/sms/open                         |
| Book取得                        | 個人のBookを取得します。                                                                                                                                                                                          | GET /book-manage                                   |
| appendix更新（個人）            | Bookの付録を更新します。                                                                                                                                                                                          | PUT /book-manage/appendix                          |
| 未同意規約取得（個人）          | 個人が未同意の規約を取得します。                                                                                                                                                                                  | GET /book-manage/term_of_use/request/list          |
| PF利用規約同意                  | 個人が未同意のPF利用規約に同意します。                                                                                                                                                                            | POST /book-manage/term_of_use/platform             |
| Region利用規約同意              | 個人が未同意のRegion利用規約に同意します。                                                                                                                                                                        | POST /book-manage/term_of_use/region               |
| My-Condition-Data出力コード取得 | My-Condition-Data出力コードのレコードを取得します。                                                                                                                                                               | POST /book-manage/output                           |
| 出力データ収集アクター取得      | 出力データ収集アクターのレコードを取得します。                                                                                                                                                                    | GET /book-manage/output/condition                  |
| 出力データ管理取得              | 出力データ管理のレコードを取得します。                                                                                                                                                                            | POST /book-manage/output/condition/data_mng/search |
| 出力データ管理更新              | 出力データ管理の更新を行います。                                                                                                                                                                                  | PUT /book-manage/output/condition/data_mng/{id}    |
| Book無効化                      | PXR-IDをリクエストに、関連付けられたBookの無効化およびその個人のログインを禁止させます。                                                                                                                          | PUT /book-manage/force-deletion                    |
| 強制解除                        | PXR-IDをリクエストに、関連付けられたBookの無効化およびログインの禁止を解除します。                                                                                                                                | PUT /book-manage/force-enable                      |
| ユーザー取得                    | ユーザーIDからユーザー情報を取得します。                                                                                                                                                                          | POST /book-manage /search/user                     |

###  5.11. <a name='Book-1'></a>**Book運用サービス**
My-Condition-Bookへデータの蓄積と共有を行います。
APIの詳細は、別紙『Book運用サービスOpenAPI.json』を参照してください。

| **API名称**                      | **概要**                                                                           | **メソッドおよびパス**                                  |
|------------|-----------------------------|--------------------------------|
| 利用者作成                       | 利用者情報を受け、利用者を作成します。                                             | POST /book-operate/user                                 |
| 利用者削除                       | 本人性確認コードを受け、利用者を削除します。                                       | POST /book-operate/user/delete                          |
| 利用者一覧検索                   | 検索条件を受け、利用者一覧を取得します。                                           | POST /book-operate/user/list                            |
| イベント蓄積                     | イベント配列を受け、Bookへ登録します。                                             | POST /book-operate/event/{userId}                       |
| ソースIDによるイベント更新       | ソースID、利用者ID、イベントを受け、Bookのイベントを更新します。                   | PUT /book-operate/event/{userId}                        |
| イベント更新                     | 利用者ID、イベント識別子、イベントを受け、Bookのイベントを更新します。             | PUT /book-operate/event/{userId}/{eventId}              |
| ソースIDによるイベント削除       | 利用者ID、ソースIDを受け、Bookのイベントを削除します。                             | DELETE /book-operate/event/{userId}                     |
| イベント削除                     | 利用者ID、イベント識別子を受け、Bookのイベントを削除します。                       | DELETE /book-operate/event/{userId}/{eventId}           |
| モノ追加                         | 利用者ID、イベント識別子、モノを受け、Bookのイベントにモノを追加します。           | POST /book-operate/thing/{userId}/{eventId}             |
| モノ一括追加                     | 利用者ID、イベント識別子、モノ配列を受け、Bookのイベントに複数モノを追加します。   | POST /book-operate/thing/bulk/{userId}/{eventId}        |
| ソースIDによるモノ更新           | 利用者ID、イベント識別子、モノのソースID、モノを受け、Bookのモノを更新します。     | PUT /book-operate/thing/{userId}/{eventId}              |
| モノ更新                         | 利用者ID、イベント識別子、モノのソースID、モノを受け、Bookのモノを更新します。     | PUT /book-operate/thing/{userId}/{eventId}              |
| ソースIDによるモノ削除           | 利用者ID、イベント識別子、モノのソースIDを受け、Bookのモノを削除します。           | DELETE /book-operate/thing/{userId}/{eventId}           |
| モノ削除                         | 利用者ID、イベント識別子、モノ識別子を受け、Bookのモノを削除します。               | DELETE /book-operate/thing/{userId}/{eventId}/{thingId} |
| ドキュメント蓄積                 | 利用者ID、ドキュメントを受け、Bookへ登録します。                                   | POST /book-operate/document/{userId}                    |
| ソースIDによるドキュメント更新   | ソースID、利用者ID、ドキュメントを受け、Bookのドキュメントを更新します。           | PUT /book-operate/document/{userId}                     |
| ドキュメント更新                 | 利用者ID、ドキュメント識別子、ドキュメントを受け、Bookのドキュメントを更新します。 | PUT /book-operate/document/{userId}/{documentId}        |
| ソースIDによるドキュメント削除   | 利用者ID、ソースIDを受け、Bookのドキュメントを削除します。                         | DELETE /book-operate/document/{userId}                  |
| ドキュメント削除                 | 利用者ID、ドキュメント識別子を受け、Bookのドキュメントを削除します。               | DELETE /book-operate/document/{userId}/{documentId}     |
| データ共有によるデータ取得       | データ共有を呼び出し、データを取得します。                                         | POST /book-operate/share                                |
| 一時的データ共有によるデータ取得 | 一時的データ共有コードを受け、データを取得後、統合処理を行って返却します。         | POST /book-operate/share/temp                           |

###  5.12. <a name='-1'></a>**本人性確認サービス**
登録された本人性確認情報を使用して本人性確認を行います。
APIの詳細は、別紙『本人性確認サービスOpenAPI.json』を参照してください。

| **API名称**                    | **概要**                                             | **メソッドおよびパス**                                             |
|------------|---------------------------|----------------------------------|
| 本人性確認事項取得             | 本人性確認事項を取得します。                         | GET /identity-verificate/acquisition/identification/{identifyCode} |
| コード発行                     | 本人性確認を行うための本人性確認コードを発行します。 | POST /identity-verificate/code                                     |
| 確認済本人性確認コード生成     | 確認済本人性確認コードを生成します。                 | POST /identity-verificate/code/verified                            |
| 発行URL確認                    | 発行したURLの確認を行います。                        | POST /identity-verificate/url/confirm                              |
| 本人性確認コード照合           | 本人性確認コードを照合します。                       | POST /identity-verificate/collate                                  |
| 個人による本人性確認コード照合 | 個人による本人性確認コード照合を実施します。         | POST /identity-verificate/ind/collate                              |
| URL発行（新）                  | 本人性確認を行うUIのURLを発行します。                | POST /identity-verificate/url/issue                                |
| 利用者ID設定                   | 対象の利用者連携について利用者IDの設定を行います。   | POST /identity-verificate/user/settings                            |
| 第三者による本人性確認         | 第三者による本人性確認を行います。                   | PUT /identity-verificate/verify/others/{identifyCode}              |

###  5.13. <a name='CToken'></a>**CToken台帳サービス**
CToken台帳の管理を行います。
APIの詳細は、別紙『CToken台帳サービスOpenAPI.json』を参照してください。

| **API名称**    | **概要**                   | **メソッドおよびパス**    |
|----------------|----------------------------|---------------------------|
| CToken件数検索 | CTokenの件数を取得します。 | POST /ctoken-ledger/count |
| CToken取得     | CTokenを取得します。       | POST /ctoken-ledger/      |

###  5.14. <a name='Local-CToken'></a>**Local-CTokenサービス**
Local CTokenをCToken台帳へ連携します。
全て内部実行されるAPIのため、一覧は記載しません。

###  5.15. <a name='-1'></a>**認証局サービス**
証明書の生成や取得、検証を行います。
全て内部実行されるAPIのため、一覧は記載しません。

###  5.16. <a name='-1'></a>**証明書管理サービス**
証明書の保存を行います。
APIの詳細は、別紙『証明書管理サービスOpenAPI.json』を参照してください。

| **API名称**                              | **概要**                                         | **メソッドおよびパス**               |
|------------|-----------------------------|-------------------------------|
| クライアント証明書アクティベートチェック | クライアント証明書が登録されているか確認します。 | GET /certificate-manage/check/client |

###  5.17. <a name='-1'></a>**アクセス制御管理サービス**
PxR-Block間通信のAPIトークンを生成する際の動的認可を行います。
全て内部実行されるAPIのため、一覧は記載しません。

###  5.18. <a name='-1'></a>**アクセス制御サービス**
リクエストの認可に必要なAPIトークンの発行、管理を行います。
全て内部実行されるAPIのため、一覧は記載しません。

###  5.19. <a name='-1'></a>**バイナリ管理サービス**
バイナリファイルのアップロード、ダウンロードを行います。
APIの詳細は、別紙『バイナリ管理サービスOpenAPI.json』を参照してください。

| **API名称**                    | **概要**                                       | **メソッドおよびパス**                            |
|------------|-------------------------|-----------------------------------|
| ファイル管理データ削除         | ファイル管理データを削除します。               | DELETE /binary-manage/{manageId}                  |
| ファイルダウンロード開始       | ファイルダウンロードを開始します。             | POST /binary-manage/download/start/{manageId}     |
| ファイルダウンロード終了       | ファイルダウンロードを終了します。             | POST /binary-manage/download/end/{manageId}       |
| ファイルダウンロードキャンセル | ファイルダウンロードを途中でキャンセルします。 | POST /binary-manage/download/cancel/{manageId}    |
| ファイルダウンロード           | ファイルをダウンロードします。                 | POST /binary-manage/download/{manageId}/{chunkNo} |

###  5.20. <a name='-1'></a>**利用者作成バッチ**
流通制御サービスプロバイダーで連携済の利用者を作成します。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.17
利用者作成バッチ」を参照してください。

###  5.21. <a name='CToken-1'></a>**CToken連携バッチ**
Local-CTokenをCToken台帳へ連携します。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.18
CToken連携バッチ」を参照してください。

###  5.22. <a name='Region'></a>**Region改定再同意依頼バッチ**
Region規約再同意データを取得し、各個人へ再同意依頼を行います。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.19
Region改定再同意依頼バッチ」を参照してください。

###  5.23. <a name='Book-1'></a>**Book自動閉鎖バッチ**
PF規約の再同意が得られなかった個人のBookを自動停止します。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.20
Book自動閉鎖バッチ」を参照してください。

###  5.24. <a name='Region-1'></a>**Region利用自動終了対象追加バッチ**
Region規約の再同意が得られなかった個人のRegion利用を自動終了の対象とします。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.21
Region利用自動終了対象追加バッチ」を参照してください。

###  5.25. <a name='Region-1'></a>**Region利用自動終了バッチ**
個人のRegion利用を自動終了します。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.22
Region利用自動終了バッチ」を参照してください。

###  5.26. <a name='Book-1'></a>**Book削除バッチ**
Book削除フラグに基づいてMy-Condition-Bookを削除します。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.23
Book削除バッチ」を参照してください。

###  5.27. <a name='-1'></a>**出力データファイル管理作成バッチ**
データ出力指定に基づいて、出力データ管理作成を作成します。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.24
出力データファイル管理作成バッチ」を参照してください。

###  5.28. <a name='-1'></a>**利用者データ削除バッチ**
利用者データを削除し、処理結果をRoot-Blockに通知します。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.25
利用者データ削除バッチ」を参照してください。

###  5.29. <a name='Region-1'></a>**Region終了対象追加バッチ**
終了承認されたRegionを使用している個人を自動終了の対象とします。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.26
Region終了対象追加バッチ」を参照してください。

###  5.30. <a name='Region-1'></a>**Region利用終了バッチ**
終了承認されたRegion利用を終了します。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.27
Region利用終了バッチ」を参照してください。

###  5.31. <a name='Region-1'></a>**Region利用者連携バッチ**
申請中のRegion利用者連携を連携中にします。
処理のシーケンス図は、別紙『アプリケーション開発ガイド別紙』の「5.28
Region利用者連携バッチ」を参照してください。

##  6. <a name='PxR-Service-1'></a>**PxR-Serviceを利用した実行順例**
本章では主な業務フローとして、My-Condition-Bookの開設～利用者ID連携、蓄積・共有の同意～My-Condition-Dataの蓄積・共有までのPxR-Serviceの実行順例を説明します。

###  6.1. <a name='-1'></a>**概要**

以下の業務について、6.2以降で説明します。（※は必須手順ではありません）

<img src="media/image11.emf" style="width:6.69306in;height:7.63611in" />

<img src="media/image12.emf" style="width:6.69306in;height:3.82153in" />

###  6.2. <a name='-1'></a>**アクター認定**
別紙『パーソナルデータ連携モジュール
利用設定手順書』の「3.PxR-Blockの追加方法」を参照してください。

###  6.3. <a name='Region-1'></a>**Region開始**
１．領域運営サービスプロバイダー：ログイン。

| サービス名称         | API名称  | メソッドおよびパス            |
|----------------------|----------|-------------------------------|
| オペレーターサービス | ログイン | POST /operator/operator/login |

リクエスト

> {
>
> "type": 3,
>
> "loginId": "region_root_member01",
>
> "hpassword":
> "pua2xcl6x37nibfahqnmlh5rkhrqv8da25pomd2pwqxksd08odt1pybijzhlq5si"
>
> }

レスポンス

> {
>
> "sessionId":
> "2q2shdetl8h8idc7nxrymqtcforbjzugctec7ltnz5af6ct8zy8tb7z9b9ci28et",
>
> "operatorId": 237,
>
> "type": 3,
>
> "loginId": "root0",
>
> "name": "領域運営　１",
>
> "auth": {
>
> "member": {
>
> "add": true,
>
> "update": true,
>
> "delete": true
>
> },
>
> "book": {
>
> "create": true
>
> },
>
> "actor": {
>
> "application": true,
>
> "approval": true
>
> },
>
> "catalog": {
>
> "create": true
>
> },
>
> "setting": {
>
> "update": true
>
> }
>
> },
>
> "lastLoginAt": "2022-06-21T20:31:56.284+0900",
>
> "passwordChangedFlg": true,
>
> "loginProhibitedFlg": false,
>
> "attributes": {
>
> "nameEn": "xxx xxxx"
>
> },
>
> "block": {
>
> "\_value": 1000401,
>
> "\_ver": 1
>
> },
>
> "actor": {
>
> "\_value": 1000431,
>
> "\_ver": 1
>
> }
>
> }

２．領域運営サービスプロバイダー：Region開始を申請する。

| サービス名称         | API名称    | メソッドおよびパス                                                 |
|-------------------|----------|--------------------------------------------|
| カタログ更新サービス | Region作成 | POST /pxr-block-proxy/pxr-block-proxy/?path=/catalog-update/region |

リクエスト

> {
>
> "name": "Region登録",
>
> "description": "Region登録の理由等記載する説明。",
>
> "catalog": \[
>
> {
>
> "type": 1,
>
> "catalogCode": null,
>
> "comment": null,
>
> "template": {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxx
> -consortium/actor/region-root/actor_1000432/region",
>
> "name": "○○○○サポート",
>
> "description": "○○○○サポートのリージョンの定義です。",
>
> "\_code": null,
>
> "inherit": {
>
> "\_value": 48,
>
> "\_ver": null
>
> }
>
> },
>
> "template": {
>
> "prop": null,
>
> "value": \[
>
> {
>
> "key": "statement",
>
> "value": \[
>
> {
>
> "key": "title",
>
> "value": "リージョンステートメント"
>
> },
>
> {
>
> "key": "section",
>
> "value": \[
>
> {
>
> "key": "title",
>
> "value": "ステートメント"
>
> },
>
> {
>
> "key": "content",
>
> "value": \[
>
> {
>
> "key": "sentence",
>
> "value":
> "○○○が○○○サービスを簡単に受けられるようにサポートするアプリの提供を行っています。"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> "inner": null,
>
> "attribute": null
>
> }
>
> }
>
> \],
>
> "appendix": null,
>
> "isDraft": false
>
> }

レスポンス

> {
>
> {
>
> "id": 1,
>
> "name": "Region登録",
>
> "description": "Region登録の理由等記載する説明。",
>
> "catalog": \[
>
> {
>
> "type": 1,
>
> "catalogCode": null,
>
> "comment": null,
>
> "template": {
>
> "catalogItem": {
>
> "ns":
> "catalog/ext/xxxx-consortium/actor/region-root/actor_1000432/region",
>
> "name": "○○○○サポート",
>
> "description": "○○○○サポートのリージョンの定義です。",
>
> "\_code": null,
>
> "inherit": {
>
> "\_value": 48,
>
> "\_ver": null
>
> }
>
> },
>
> "template": {
>
> "prop": null,
>
> "value": \[
>
> {
>
> "key": "statement",
>
> "value": \[
>
> {
>
> "key": "title",
>
> "value": "リージョンステートメント"
>
> },
>
> {
>
> "key": "section",
>
> "value": \[
>
> {
>
> "key": "title",
>
> "value": "ステートメント"
>
> },
>
> {
>
> "key": "content",
>
> "value": \[
>
> {
>
> "key": "sentence",
>
> "value":
> "○○○が○○○サービスを簡単に受けられるようにサポートするアプリの提供を行っています。"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> "inner": null,
>
> "attribute": null
>
> }
>
> }
>
> \],
>
> "appendix": null,
>
> "isDraft": false
>
> }

３．流通制御サービスプロバイダー：ログイン。

| サービス名称         | API名称  | メソッドおよびパス   |
|----------------------|----------|----------------------|
| オペレーターサービス | ログイン | POST /operator/login |

リクエスト

> {
>
> "type": 3, // 運営メンバー固定
>
> "loginId": "root0", // 流通制御サービスプロバイダーの運営メンバー
>
> "hpassword":
> "pua2xcl6x37nibfahqnmlh5rkhrqv8da25pomd2pwqxksd08odt1pybijzhlq5si" //
> 流通制御サービスプロバイダーの運営メンバーPWのhash化
>
> }

レスポンス

> {
>
> "sessionId":
> "2q2shdetl8h8idc7nxrymqtcforbjzugctec7ltnz5af6ct8zy8tb7z9b9ci28et",
>
> "operatorId": 237,
>
> "type": 3,
>
> "loginId": "root0",
>
> "name": "管理者　１",
>
> "auth": {
>
> "member": {
>
> "add": true,
>
> "update": true,
>
> "delete": true
>
> },
>
> "book": {
>
> "create": true
>
> },
>
> "actor": {
>
> "application": true,
>
> "approval": true
>
> },
>
> "catalog": {
>
> "create": true
>
> },
>
> "setting": {
>
> "update": true
>
> }
>
> },
>
> "lastLoginAt": "2022-06-21T20:31:56.284+0900",
>
> "passwordChangedFlg": true,
>
> "loginProhibitedFlg": false,
>
> "attributes": {
>
> "nameEn": "xxx　xxxx"
>
> },
>
> "block": {
>
> "\_value": 1000401,
>
> "\_ver": 1
>
> },
>
> "actor": {
>
> "\_value": 1000431,
>
> "\_ver": 1
>
> }
>
> }

４．流通制御サービスプロバイダー：Region開始申請を承認する。

| サービス名称 | API名称 | メソッドおよびパス                                                |
|-------------------|----------|--------------------------------------------|
| 通知サービス | 承認    | PUT /pxr-block-proxy/pxr-block-proxy/?path=/notification/approval |

リクエスト

> {
>
> "id": 2,
>
> "status": 1,
>
> "comment": null
>
> }

レスポンス

> {
>
> "id": 2,
>
> "status": 1,
>
> "comment": null
>
> }

###  6.4. <a name='Region-1'></a>**Region参加**

１．アプリケーションプロバイダー：ログイン。

| サービス名称         | API名称  | メソッドおよびパス            |
|----------------------|----------|-------------------------------|
| オペレーターサービス | ログイン | POST /operator/operator/login |

リクエスト

> {
>
> "type": 2,
>
> "loginId": "ap01user02",
>
> "hpassword":
> "pua2xcl6x37nibfahqnmlh5rkhrqv8da25pomd2pwqxksd08odt1pybijzhlq5si"
>
> }

レスポンス

> {
>
> "sessionId":
> "2q2shdetl8h8idc7nxrymqtcforbjzugctec7ltnz5af6ct8zy8tb7z9b9ci28et",
>
> "operatorId": 101,
>
> "type": 2,
>
> "loginId": "ap01user02",
>
> "name": "myApp-A",
>
> "lastLoginAt": "2022-04-28T15:16:45.489+0900",
>
> "passwordChangedFlg": false,
>
> "loginProhibitedFlg": false,
>
> "attributes": {},
>
> "roles": \[
>
> {
>
> "\_value": 1000879,
>
> "\_ver": 1
>
> }
>
> \],
>
> "block": {
>
> "\_value": 1000407,
>
> "\_ver": 1
>
> },
>
> "actor": {
>
> "\_value": 1000787,
>
> "\_ver": 1
>
> }
>
> }

２．アプリケーションプロバイダー：Region参加を申請する。

| サービス名称         | API名称  | メソッドおよびパス                                               |
|-------------------|----------|--------------------------------------------|
| カタログ更新サービス | 参加要求 | POST /pxr-block-proxy/pxr-block-proxy/?path=/catalog-update/join |

リクエスト

> {
>
> "region": {
>
> "code": 1000011,
>
> "version": 1
>
> },
>
> "actor": {
>
> "code": 1000021,
>
> "version": 1,
>
> "application": \[
>
> {
>
> "code": 1000021,
>
> "version": 1
>
> }
>
> \],
>
> "wf": null
>
> },
>
> "isDraft": false
>
> }

レスポンス

> {
>
> "id": 1,
>
> "region": {
>
> "code": 1000011,
>
> "version": 1
>
> },
>
> "actor": {
>
> "code": 1000021,
>
> "version": 1,
>
> "application": \[
>
> {
>
> "code": 1000021,
>
> "version": 1
>
> }
>
> \],
>
> "wf": null
>
> },
>
> "expireAt": "2020-01-10 10:00:00.000",
>
> "isDraft": false
>
> }

３．領域運営サービスプロバイダー：Region参加を承認する。

※事前に6.3.1領域運営サービスプロバイダー：ログインを実施すること。

| サービス名称 | API名称 | メソッドおよびパス                                                |
|-------------------|----------|--------------------------------------------|
| 通知サービス | 承認    | PUT /pxr-block-proxy/pxr-block-proxy/?path=/notification/approval |

リクエスト

> {
>
> "id": 2,
>
> "status": 1,
>
> "comment": null
>
> }

レスポンス

> {
>
> "id": 2,
>
> "status": 1,
>
> "comment": null
>
> }

###  6.5. <a name='-1'></a>**データカタログ作成**

１．アプリケーションプロバイダー：モノのカタログ登録。（カタログ変更セット登録）

| サービス名称     | API名称        | メソッドおよびパス                                              |
|-------------------|----------|--------------------------------------------|
| カタログサービス | 変更セット登録 | POST /pxr-block-proxy/pxr-block-proxy/?path=/updateSet/register |

※事前に6.4.1アプリケーションプロバイダー：ログインを実施すること。

リクエスト

> {
>
> "name": "モノ名(新規作成)",
>
> "description": null,
>
> "type": 0,
>
> "ns": \[\],
>
> "catalog": \[
>
> {
>
> "type": 1,
>
> "catalogCode": null,
>
> "template": {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxxxxxxxx/thing/actor_1000436",
>
> "name": "モノ名",
>
> "\_code": {
>
> "\_value": null,
>
> "\_ver": null
>
> },
>
> "inherit": {
>
> "\_value": 55,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "モノ概要",
>
> "content": \[
>
> {
>
> "sentence": "モノ概要"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "prop": null,
>
> "value": null
>
> },
>
> "attribute": {
>
> "objects": \[\],
>
> "tags": \[\]
>
> },
>
> "inner": \[
>
> {
>
> "template": {
>
> "prop": \[\]
>
> },
>
> "inner": \[\]
>
> }
>
> \]
>
> }
>
> }
>
> \],
>
> "attribute": \[\],
>
> "appendix": \[\]
>
> }

レスポンス

> {
>
> "id": 1, //変更セットID
>
> "name": "モノ名(新規作成)",
>
> "description": null,
>
> "callerActorCode": 1000436, //申請アクターコード
>
> "approvalActorCode": null, //承認アクターコード
>
> "approver": null, //承認オペレーター
>
> "approvalAt": null, //承認日時
>
> "comment": null,
>
> "status": 1, //承認ステータス
>
> "registerActorCode": 1000436, //登録アクターコード
>
> "register": " ap01user02",
>
> "registAt": "2022-06-21T20:31:56.284+0900",
>
> "ns": null,
>
> "catalog": //申請したカタログの内容（省略）
>
> }

２．アプリケーションプロバイダー：イベントのカタログ登録。（カタログ変更セット登録）

| サービス名称     | API名称        | メソッドおよびパス                                              |
|-------------------|----------|--------------------------------------------|
| カタログサービス | 変更セット登録 | POST /pxr-block-proxy/pxr-block-proxy/?path=/updateSet/register |

リクエスト

> {
>
> "name": "イベント名(新規作成)",
>
> "description": null,
>
> "type": 0,
>
> "ns": \[\],
>
> "catalog": \[
>
> {
>
> "type": 1,
>
> "catalogCode": null,
>
> "template": {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxxxxxxxx/event/actor_1000436",
>
> "name": "イベント名",
>
> "\_code": {
>
> "\_value": null,
>
> "\_ver": null
>
> },
>
> "inherit": {
>
> "\_value": 53,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "イベント概要",
>
> "content": \[
>
> {
>
> "sentence": "イベント概要"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "prop": \[
>
> {
>
> "key": "thing",
>
> "type": {
>
> "candidate": {
>
> "ns": \[
>
> "catalog/model/thing/\*",
>
> "catalog/built_in/thing/\*",
>
> "catalog/ext/xxxxxxxxxxx/thing/\*"
>
> \],
>
> "\_code": \[
>
> {
>
> "\_value": 1000100, //紐づけるモノのコード。複数ある場合は記述繰り返し
>
> "\_ver": 1
>
> }
>
> \],
>
> "base": null
>
> },
>
> "cmatrix": null,
>
> "\_code": null,
>
> "of": "item\[\]"
>
> },
>
> "description": "モノの配列",
>
> "isInherit": false
>
> }
>
> \],
>
> "value": null
>
> },
>
> "attribute": {
>
> "objects": \[\],
>
> "tags": \[\]
>
> },
>
> "inner": \[
>
> {
>
> "template": {
>
> "prop": \[\]
>
> },
>
> "inner": \[\]
>
> }
>
> \]
>
> }
>
> }
>
> \],
>
> "attribute": \[\],
>
> "appendix": \[\]
>
> }

レスポンス

> {
>
> "id": 1, //変更セットID
>
> "name": "イベント名(新規作成)",
>
> "description": null,
>
> "callerActorCode": 1000436, //申請アクターコード
>
> "approvalActorCode": null, //承認アクターコード
>
> "approver": null, //承認オペレーター
>
> "approvalAt": null, //承認日時
>
> "comment": null,
>
> "status": 1, //承認ステータス
>
> "registerActorCode": 1000436, //登録アクターコード
>
> "register": " ap01user02",
>
> "registAt": "2022-06-21T20:31:56.284+0900",
>
> "ns": null,
>
> "catalog": //申請したカタログの内容（省略）
>
> }

３．アプリケーションプロバイダー：ドキュメントのカタログ登録。（カタログ変更セット登録）

| サービス名称     | API名称        | メソッドおよびパス                                              |
|-------------------|----------|--------------------------------------------|
| カタログサービス | 変更セット登録 | POST /pxr-block-proxy/pxr-block-proxy/?path=/updateSet/register |

リクエスト

> {
>
> "name": "ドキュメント名(新規作成)",
>
> "description": null,
>
> "type": 0,
>
> "ns": \[\],
>
> "catalog": \[
>
> {
>
> "type": 1,
>
> "catalogCode": null,
>
> "template": {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxxxxxxxx/document/actor_1001197",
>
> "name": "ドキュメント名",
>
> "\_code": {
>
> "\_value": null,
>
> "\_ver": null
>
> },
>
> "inherit": {
>
> "\_value": 52,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "ドキュメント名",
>
> "content": \[
>
> {
>
> "sentence": "ドキュメント名"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "prop": \[
>
> {
>
> "key": "chapter",
>
> "type": {
>
> "candidate": {
>
> "ns": null,
>
> "\_code": \[
>
> {
>
> "\_value": 1000101, //紐づけるイベントのコード,
>
> "\_ver": 1 //紐づけるイベントのバージョン
>
> }
>
> \],
>
> "base": null
>
> },
>
> "cmatrix": null,
>
> "inner": "Chapter",
>
> "of": "inner\[\]"
>
> },
>
> "description": "チャプターの配列",
>
> "isInherit": false
>
> }
>
> \],
>
> "value": null
>
> },
>
> "attribute": {
>
> "objects": \[\],
>
> "tags": \[\]
>
> },
>
> "inner": \[
>
> {
>
> "template": {
>
> "prop": \[\]
>
> },
>
> "inner": \[\]
>
> }
>
> \]
>
> }
>
> }
>
> \],
>
> "attribute": \[\],
>
> "appendix": \[\]
>
> }

レスポンス

> {
>
> "id": 1, //変更セットID
>
> "name": "ドキュメント名(新規作成)",
>
> "description": null,
>
> "callerActorCode": 1000436, //申請アクターコード
>
> "approvalActorCode": null, //承認アクターコード
>
> "approver": null, //承認オペレーター
>
> "approvalAt": null, //承認日時
>
> "comment": null,
>
> "status": 1, //承認ステータス
>
> "registerActorCode": 1000436, //登録アクターコード
>
> "register": " ap01user02",
>
> "registAt": "2022-06-21T20:31:56.284+0900",
>
> "ns": null,
>
> "catalog": //申請したカタログの内容（省略）
>
> }

リクエスト

> {
>
> "name": "ドキュメント名(新規作成)",
>
> "description": null,
>
> "type": 0,
>
> "ns": \[\],
>
> "catalog": \[
>
> {
>
> "type": 1,
>
> "catalogCode": null,
>
> "template": {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxxxxxxxx/document/actor_1001197",
>
> "name": "ドキュメント名",
>
> "\_code": {
>
> "\_value": null,
>
> "\_ver": null
>
> },
>
> "inherit": {
>
> "\_value": 52,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "ドキュメント名",
>
> "content": \[
>
> {
>
> "sentence": "ドキュメント名"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "prop": \[
>
> {
>
> "key": "chapter",
>
> "type": {
>
> "candidate": {
>
> "ns": null,
>
> "\_code": \[
>
> {
>
> "\_value": 1000101, //紐づけるイベントのコード,
>
> "\_ver": 1 //紐づけるイベントのバージョン
>
> }
>
> \],
>
> "base": null
>
> },
>
> "cmatrix": null,
>
> "inner": "Chapter",
>
> "of": "inner\[\]"
>
> },
>
> "description": "チャプターの配列",
>
> "isInherit": false
>
> }
>
> \],
>
> "value": null
>
> },
>
> "attribute": {
>
> "objects": \[\],
>
> "tags": \[\]
>
> },
>
> "inner": \[
>
> {
>
> "template": {
>
> "prop": \[\]
>
> },
>
> "inner": \[\]
>
> }
>
> \]
>
> }
>
> }
>
> \],
>
> "attribute": \[\],
>
> "appendix": \[\]
>
> }

レスポンス

> {
>
> "id": 1, //変更セットID
>
> "name": "ドキュメント名(新規作成)",
>
> "description": null,
>
> "callerActorCode": 1000436, //申請アクターコード
>
> "approvalActorCode": null, //承認アクターコード
>
> "approver": null, //承認オペレーター
>
> "approvalAt": null, //承認日時
>
> "comment": null,
>
> "status": 1, //承認ステータス
>
> "registerActorCode": 1000436, //登録アクターコード
>
> "register": " ap01user02",
>
> "registAt": "2022-06-21T20:31:56.284+0900",
>
> "ns": null,
>
> "catalog": //申請したカタログの内容（省略）
>
> }

４．アプリケーションプロバイダー：カタログ変更セット申請。

| サービス名称     | API名称        | メソッドおよびパス                                             |
|-------------------|----------|--------------------------------------------|
| カタログサービス | 変更セット登録 | POST /pxr-block-proxy/pxr-block-proxy/?path=/updateSet/request |

リクエスト

> {
>
> "id": 1, //変更セット登録で返ってくるID
>
> "approvalActor": 1000400 //申請先のアクターコード
>
> }

レスポンス

> {
>
> "id": 1, //変更セットID
>
> "name": "ドキュメント名(新規作成)",
>
> "description": null,
>
> "callerActorCode": 1000436, //申請アクターコード
>
> "approvalActorCode": null, //承認アクターコード
>
> "approver": null, //承認オペレーター
>
> "approvalAt": null, //承認日時
>
> "comment": null,
>
> "status": 1, //承認ステータス
>
> "registerActorCode": 1000436, //登録アクターコード
>
> "register": " ap01user02",
>
> "registAt": "2022-06-21T20:31:56.284+0900",
>
> "ns": null,
>
> "catalog": //申請したカタログの内容（省略）
>
> }

５．流通制御サービスプロバイダー：カタログ変更セット承認。

※事前に6.3.3流通制御サービスプロバイダー：ログインを実施すること。

| サービス名称     | API名称        | メソッドおよびパス                                                                             |
|-------------------|----------|--------------------------------------------|
| カタログサービス | 変更セット登録 | POST /pxr-block-proxy/pxr-block-proxy/?path=/updateSet/approval/{変更セット登録で返ってくるID} |

リクエスト

> {
>
> "status": 1,
>
> "comment": "変更セットを承認します。"
>
> }

レスポンス

> {
>
> "id": 1, //変更セットID
>
> "name": "ドキュメント名(新規作成)",
>
> "description": null,
>
> "callerActorCode": 1000436, //申請アクターコード
>
> "approvalActorCode": null, //承認アクターコード
>
> "approver": null, //承認オペレーター
>
> "approvalAt": null, //承認日時
>
> "comment": null,
>
> "status": 1, //承認ステータス
>
> "registerActorCode": 1000436, //登録アクターコード
>
> "register": " ap01user02",
>
> "registAt": "2022-06-21T20:31:56.284+0900",
>
> "ns": null,
>
> "catalog": //申請したカタログの内容（省略）
>
> }

###  6.6. <a name='My-Condition-Book'></a>**My-Condition-Bookの開設**

１. 流通制御サービスプロバイダー：My-Condition-Bookの開設。

| サービス名称     | API名称  | メソッドおよびパス |
|------------------|----------|--------------------|
| Book管理サービス | Book開設 | POST /book-manage  |

※事前に6.3.3流通制御サービスプロバイダー：ログインを実施すること。

リクエスト

> {
>
> "pxrId": "User00005",
>
> "idConnectFlg": false,
>
> "attributes": {},
>
> "appendix": {},
>
> "identification": \[
>
> {
>
> "\_code": {
>
> "\_value": 30001,
>
> "\_ver": 1
>
> },
>
> "item-group": \[
>
> {
>
> "title": "氏名",
>
> "item": \[
>
> {
>
> "title": "姓",
>
> "type": {
>
> "\_value": 30019,
>
> "\_ver": 1
>
> },
>
> "content": "サンプル"
>
> },
>
> {
>
> "title": "名",
>
> "type": {
>
> "\_value": 30020,
>
> "\_ver": 1
>
> },
>
> "content": "太郎"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "性別",
>
> "item": \[
>
> {
>
> "title": "性別",
>
> "type": {
>
> "\_value": 30021,
>
> "\_ver": 1
>
> },
>
> "content": "男"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "生年月日",
>
> "item": \[
>
> {
>
> "title": "生年月日",
>
> "type": {
>
> "\_value": 30022,
>
> "\_ver": 1
>
> },
>
> "content": "2000-01-01"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \],
>
> "userInformation": {
>
> "\_code": {
>
> "\_value": 1000373,
>
> "\_ver": 1
>
> },
>
> "item-group": \[
>
> {
>
> "title": "氏名",
>
> "item": \[
>
> {
>
> "title": "姓",
>
> "type": {
>
> "\_value": 30019,
>
> "\_ver": 1
>
> },
>
> "content": "サンプル"
>
> },
>
> {
>
> "title": "名",
>
> "type": {
>
> "\_value": 30020,
>
> "\_ver": 1
>
> },
>
> "content": "太郎"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "性別",
>
> "item": \[
>
> {
>
> "title": "性別",
>
> "type": {
>
> "\_value": 30021,
>
> "\_ver": 1
>
> },
>
> "content": "男"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "生年（西暦）",
>
> "item": \[
>
> {
>
> "title": "生年（西暦）",
>
> "type": {
>
> "\_value": 1000372,
>
> "\_ver": 1
>
> },
>
> "content": 2000
>
> }
>
> \]
>
> },
>
> {
>
> "title": "住所（行政区）",
>
> "item": \[
>
> {
>
> "title": "住所（行政区）",
>
> "type": {
>
> "\_value": 1000371,
>
> "\_ver": 1
>
> },
>
> "content": "東京都港区"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "連絡先電話番号",
>
> "item": \[
>
> {
>
> "title": "連絡先電話番号",
>
> "type": {
>
> "\_value": 30036,
>
> "\_ver": 1
>
> },
>
> "content": "080-0000-0000",
>
> "changable-flag": true,
>
> "require-sms-verification": false //
> true：sms（ワンタイムログイン有効）要、false:sms（ワンタイムログイン無効）不要
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> "platform_terms_of_use": {
>
> "\_value": 1000782, //　利用規約コード取得（configに設定）
>
> "\_ver": 1
>
> }
>
> }

レスポンス

> {
>
> "pxrId": "User00005", // 以降で利用するID
>
> "initialPassword": "2fsnoz6d0vau", //
> Book開設時の初期パスワード、Hash化して以降利用
>
> "attributes": {},
>
> "appendix": {},
>
> "identifyCode": null
>
> }

###  6.7. <a name='RegionID'></a>**Regionとの利用者ID連携**

１．流通制御サービスプロバイダー：オペレーターIDを取得。

※事前に6.3.3流通制御サービスプロバイダー：ログインを実施すること。

| サービス名称         | API名称      | メソッドおよびパス                    |
|-------------------|------------|------------------------------------------|
| オペレーターサービス | IDによる取得 | GET /operator/?type=0&loginId={pxrId} |

リクエスト

> なし

レスポンス

> {
>
> "operatorId": 596, // パスワード変更を実施する際利用
>
> "type": 0,
>
> "loginId": "User00005",
>
> "pxrId": "User00005",
>
> "mobilePhone": "080-0000-0000",
>
> "passwordChangedFlg": false,
>
> "attributes": {
>
> "initialPasswordExpire": "2022-06-29T13:36:11.283+0900",
>
> "smsAuth": false
>
> },
>
> "loginProhibitedFlg": false
>
> }

2\. 流通制御サービスプロバイダー：パスワード変更（※必要に応じ実施）

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>オペレーターサービス</td>
<td>更新</td>
<td><p>PUT /operator/{operatorId}</p>
<p>※{operatorId}：オペレーターID、3.オペレーターIDの取得のレスポンスのoperatorIdを利用</p></td>
</tr>
</tbody>
</table>

リクエスト

> {
>
> //
> 初期パスワードの期限：1週間、引き続きBookを利用する場合、パスワードの変更が必要
>
> // パスワードの変更を行わない場合、Request
> Bodyから"hpassword"、"newHpassword"を除外
>
> "hpassword": "", // 現在のパスワード
> 2.Book開設のレスポンスのinitialPasswordnのhash化した値を利用
>
> "newHpassword": "", // 変更するパスワードのhash化した値を記載
>
> "attributes": {
>
> "initialPasswordExpire": "2022-06-09T09:46:32.032+0900",
>
> "smsAuth": false
>
> }
>
> }

レスポンス

> {
>
> "id": 529,
>
> "type": 0,
>
> "loginId": "User00002",
>
> "pxrId": "User00002",
>
> "mobilePhone": "080-0000-0000",
>
> "passwordChangedFlg": false,
>
> "attributes": {
>
> "initialPasswordExpire": "2022-06-09T09:46:32.032+0900",
>
> "smsAuth": false //
> ワンタイムログイン無効化されたことを確認、凡例：true（有効）、false（無効）
>
> }
>
> }

３．個人：ログイン。

| サービス名称         | API名称        | メソッドおよびパス       |
|----------------------|----------------|--------------------------|
| オペレーターサービス | 個人用ログイン | POST /operator/ind/login |

リクエスト

> {
>
> "type": 0, // 個人固定
>
> "loginId": "User00005", // 4.パスワード変更のresponseのloginId
>
> "hpassword":
> "pua2xcl6x37nibfahqnmlh5rkhrqv8da25pomd2pwqxksd08odt1pybijzhlq5si" //
> 3.パスワード変更で変更したパスワードのhash化
>
> }

レスポンス

> {
>
> "sessionId":
> "2q2shdetl8h8idc7nxrymqtcforbjzugctec7ltnz5af6ct8zy8tb7z9b9ci28et",
>
> "operatorId": 596,
>
> "type": 0,
>
> "loginId": "User00005",
>
> "pxrId": "User00005",
>
> "userInformation": {
>
> "\_code": {
>
> "\_value": 1000373,
>
> "\_ver": 1
>
> },
>
> "item-group": \[
>
> {
>
> "title": "氏名",
>
> "item": \[
>
> {
>
> "title": "姓",
>
> "type": {
>
> "\_value": 30019,
>
> "\_ver": 1
>
> },
>
> "content": "サンプル"
>
> },
>
> {
>
> "title": "名",
>
> "type": {
>
> "\_value": 30020,
>
> "\_ver": 1
>
> },
>
> "content": "太郎"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "性別",
>
> "item": \[
>
> {
>
> "title": "性別",
>
> "type": {
>
> "\_value": 30021,
>
> "\_ver": 1
>
> },
>
> "content": "男"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "生年（西暦）",
>
> "item": \[
>
> {
>
> "title": "生年（西暦）",
>
> "type": {
>
> "\_value": 1000372,
>
> "\_ver": 1
>
> },
>
> "content": 2000
>
> }
>
> \]
>
> },
>
> {
>
> "title": "住所（行政区）",
>
> "item": \[
>
> {
>
> "title": "住所（行政区）",
>
> "type": {
>
> "\_value": 1000371,
>
> "\_ver": 1
>
> },
>
> "content": "東京都港区"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "連絡先電話番号",
>
> "item": \[
>
> {
>
> "title": "連絡先電話番号",
>
> "type": {
>
> "\_value": 30036,
>
> "\_ver": 1
>
> },
>
> "content": "080-0000-0000",
>
> "changable-flag": true,
>
> "require-sms-verification": false
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> "mobilePhone": "080-0000-0000",
>
> "lastLoginAt": null,
>
> "passwordChangedFlg": false,
>
> "loginProhibitedFlg": false,
>
> "attributes": {
>
> "initialPasswordExpire": "2022-06-29T13:36:11.283+0900",
>
> "smsAuth": false
>
> },
>
> "block": {
>
> "\_value": 1000401,
>
> "\_ver": 1
>
> },
>
> "actor": {
>
> "\_value": 1000431,
>
> "\_ver": 1
>
> }
>
> }

4\. 個人：Region利用者ID連携の実施。

| サービス名称       | API名称    | メソッドおよびパス                                                              |
|-------------------|----------|--------------------------------------------|
| 本人性確認サービス | コード発行 | POST /pxr-block-proxy/ind/?block=$pxr-root-block&path=/identity-verificate/code |

リクエスト

> {
>
> "actor": {
>
> "\_value": 1000783, // Regionのアクターコード
>
> "\_ver": 3
>
> },
>
> "region": {
>
> "\_value": 1000816, //
> 事前に決まったRegionコード、該当Regionの利用規約の取得に利用
>
> "\_ver": 11
>
> },
>
> "app": null,
>
> "wf": null
>
> }

レスポンス

> {
>
> "identifyCode": "Y2JlOGIzYjItY2U4OC00MzY4LWJjOWMtMGMwMGE5YTgyMzRm",
>
> "actor": {
>
> "\_value": 1000783,
>
> "\_ver": 3
>
> },
>
> "region": {
>
> "\_value": 1000816,
>
> "\_ver": 11
>
> },
>
> "expireAt": "2022-06-29T13:39:56.274+0900"
>
> }

5.流通制御サービスプロバイダー：Region利用規約のコードを取得（※必要に応じ実施）

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 13%" />
<col style="width: 61%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>カタログサービス</td>
<td>カタログ取得(code指定)</td>
<td><p>GET /catalog/{code}</p>
<p>{code}：カタログ項目コード、6.
Region利用者ID連携のrequestのregionコードを利用</p></td>
</tr>
</tbody>
</table>

リクエスト

> なし

レスポンス

> {
>
> "catalogItem": {
>
> "ns":
> "catalog/ext/xxxxx-service/actor/region-root/actor_1000783/region",
>
> "name": "〇〇〇サービス",
>
> "\_code": {
>
> "\_value": 1000816,
>
> "\_ver": 11
>
> },
>
> "inherit": {
>
> "\_value": 48,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": "Regionステートメント",
>
> "section": \[
>
> {
>
> "title": "〇〇〇サービス",
>
> "content": \[
>
> {
>
> "sentence": "〇〇〇サービスのRegionです"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "\_code": {
>
> "\_value": 1000816,
>
> "\_ver": 11
>
> },
>
> "app-alliance": \[
>
> {
>
> "\_value": 1000809,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000855,
>
> "\_ver": 2
>
> },
>
> {
>
> "\_value": 1000879,
>
> "\_ver": 2
>
> }
>
> \],
>
> "information-site": null,
>
> "required_app": \[
>
> {
>
> "\_value": 1000809,
>
> "\_ver": 1
>
> }
>
> \],
>
> "required_wf": \[
>
> {
>
> "\_value": 1000807,
>
> "\_ver": 1
>
> }
>
> \],
>
> "statement": \[
>
> {
>
> "title": "Regionステートメント",
>
> "section": \[
>
> {
>
> "title": "〇〇〇サービス",
>
> "content": \[
>
> {
>
> "sentence": "〇〇〇サービスのRegionです"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \],
>
> "status": "open",
>
> "terms-of-use": {
>
> "\_value": 1000815, // region利用規約同意に利用
>
> "\_ver": 1
>
> },
>
> "wf-alliance": \[
>
> {
>
> "\_value": 1000807,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000862,
>
> "\_ver": 2
>
> },
>
> {
>
> "\_value": 1000911,
>
> "\_ver": 2
>
> },
>
> {
>
> "\_value": 1000929,
>
> "\_ver": 1
>
> }
>
> \]
>
> },
>
> "prop": \[
>
> {
>
> "key": "app-alliance",
>
> "type": {
>
> "of": "code\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": {
>
> "\_value": 41,
>
> "\_ver": 1
>
> }
>
> }
>
> },
>
> "description": "Regionメンバー(アプリケーション)のコード配列",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "information-site",
>
> "type": {
>
> "of": "string",
>
> "cmatrix": null,
>
> "format": null,
>
> "unit": null,
>
> "candidate": null
>
> },
>
> "description": "Regionの情報サイト",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "required_app",
>
> "type": {
>
> "of": "code\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": {
>
> "\_value": 41,
>
> "\_ver": 1
>
> }
>
> }
>
> },
>
> "description": "必須アプリケーションのコード配列",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "statement",
>
> "type": {
>
> "of": "item\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": \[
>
> {
>
> "\_value": 61,
>
> "\_ver": 1
>
> }
>
> \],
>
> "base": null
>
> }
>
> },
>
> "description": "Regionステートメント",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "status",
>
> "type": {
>
> "of": "string",
>
> "cmatrix": null,
>
> "format": null,
>
> "unit": null,
>
> "candidate": {
>
> "value": \[
>
> "open",
>
> "close"
>
> \]
>
> }
>
> },
>
> "description": "サービス運用ステータス",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "terms-of-use",
>
> "type": {
>
> "of": "code",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": null
>
> }
>
> },
>
> "description": "Region利用規約のカタログコード",
>
> "isInherit": true
>
> }
>
> \],
>
> "value": \[
>
> {
>
> "key": "app-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000809
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "app-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000855
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "app-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000879
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "information-site",
>
> "value": null
>
> },
>
> {
>
> "key": "required_app",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000809
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "required_wf",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000807
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "statement",
>
> "value": \[
>
> {
>
> "key": "title",
>
> "value": "Regionステートメント"
>
> },
>
> {
>
> "key": "section",
>
> "value": \[
>
> {
>
> "key": "title",
>
> "value": "〇〇〇サービス"
>
> },
>
> {
>
> "key": "content",
>
> "value": \[
>
> {
>
> "key": "sentence",
>
> "value": "〇〇〇サービスのRegionです"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> {
>
> "key": "status",
>
> "value": "open"
>
> },
>
> {
>
> "key": "terms-of-use",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000815
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "wf-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000807
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "wf-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000862
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "wf-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000911
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "wf-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000929
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> }
>
> \],
>
> "attribute": null
>
> }

6\. 個人：Region利用規約に同意。

| サービス名称                  | API名称            | メソッドおよびパス                                                                        |
|-------------------|-----------|-------------------------------------------|
| My-Condition-Book管理サービス | Region利用規約同意 | POST /pxr-block-proxy/ind/?block=$pxr-root-block&path=/book-manage/ind/term_of_use/region |

リクエスト

> {
>
> "actor": {
>
> "\_value": 1000783, // 6.Region利用者ID連携のRequest Bodyのactor
>
> "\_ver": 3
>
> },
>
> "region": {
>
> "\_value": 1000816, // 6.Region利用者ID連携のRequest Bodyのregion
>
> "\_ver": 11
>
> },
>
> "\_code": {
>
> "\_value": 1000815, // 7.
> 利用規約のコードを取得のterms-of-useのコードを利用
>
> "\_ver": 1
>
> }
>
> }

レスポンス

> {
>
> "result": "success"
>
> }

###  6.8. <a name='ID'></a>**アプリケーションとの利用者ID連携**

１．個人：アプリケーションとの利用者ID連携申請。

※事前に6.7.3 個人：ログインを実施すること。

| サービス名称       | API名称    | メソッドおよびパス                                                             |
|-------------------|----------|--------------------------------------------|
| 本人性確認サービス | コード発行 | POST pxr-block-proxy/ind/?block=$pxr-root-block&path=/identity-verificate/code |

リクエスト

> {
>
> "actor": {
>
> "\_value": 1000787, // アクターカタログコード
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879, // アプリケーションコード
>
> "\_ver": 7
>
> },
>
> "wf": null
>
> }

レスポンス

> {
>
> "identifyCode":
> "NDc5NGZhNmYtMjI3MC00YzBjLWFmMGQtNTUwYTJiMjJiMjAx",　//
> 認証コード以降の手順で利用する
>
> "actor": {
>
> "\_value": 1000787,
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "expireAt": "2022-06-29T13:43:28.281+0900"
>
> }

２．アプリケーションプロバイダー：URLを発行。

| サービス名称       | API名称 | メソッドおよびパス                                                              |
|-------------------|----------|--------------------------------------------|
| 本人性確認サービス | URL発行 | POST pxr-block-proxy/?block=$pxr-root-block&path=/identity-verificate/url/issue |

※事前に6.4.1アプリケーションプロバイダー：ログインを実施すること。

リクエスト

> {
>
> "urlType": 1,
>
> "userId": "User00005" // 任意で設定可能、当箇所は3.
> オペレータIDの取得のresponseのuserIdで設定
>
> }

レスポンス

> {
>
> "url":
> "https://root.xxxxxx.jp/personal-portal/enter-code/?identifyCode=N1YjhjYWYtOWQzZC00MWNlLTlhMTQtZTgzYWU3OWFlMzEy"
>
> //
> NjZmZjQ5ZjEtNDk0OS00NTI0LTliOTktMmVkMWZiOTI5ZGZmを以降利用、※pathCodeとして利用.
>
> }

３．個人：リダイレクトの不正チェック。

※事前に6.7.3 個人：ログインを実施すること。

| サービス名称       | API名称     | メソッドおよびパス                                                                     |
|-------------------|----------|--------------------------------------------|
| 本人性確認サービス | 発行URL確認 | POST /pxr-block-proxy/ind/?block=$pxr-root-block&path=/identity-verificate/url/confirm |

リクエスト

> {
>
> "path": "ZWE1YjhjYWYtOWQzZC00MWNlLTlhMTQtZTgzYWU3OWFlMzEy" // 3.
> URL発行のパスコードを利用
>
> }

レスポンス

> {
>
> "result":"success"
>
> }

４．個人：本人性確認コードの認証。

| サービス名称       | API名称                        | メソッドおよびパス                                                                    |
|-------------------|----------|--------------------------------------------|
| 本人性確認サービス | 個人による本人性確認コード照合 | POST pxr-block-proxy/ind/?block=$pxr-root-block&path=/identity-verificate/ind/collate |

リクエスト

> {
>
> "identifyCode": "NDc5NGZhNmYtMjI3MC00YzBjLWFmMGQtNTUwYTJiMjJiMjAx",
> // 1. APP利用者ID連携申請のidentifyCodeを利用
>
> "path": "ZWE1YjhjYWYtOWQzZC00MWNlLTlhMTQtZTgzYWU3OWFlMzEy" // 3.
> URL発行のパスコードを利用
>
> }

レスポンス

> {
>
> "pxrId": "User00005",
>
> "userId": "User00005",
>
> "actor": {
>
> "\_value": "1000787",
>
> "\_ver": "3"
>
> },
>
> "app": {
>
> "\_value": "1000879",
>
> "\_ver": "7"
>
> }
>
> }

５．個人：利用者ID連携の実施。

| サービス名称     | API名称      | メソッドおよびパス                                                              |
|-------------------|----------|--------------------------------------------|
| Book管理サービス | 利用者ID連携 | POST pxr-block-proxy/ind/?block=$pxr-root-block&path=/book-manage/ind/cooperate |

リクエスト

> {
>
> "identifyCode": "NDc5NGZhNmYtMjI3MC00YzBjLWFmMGQtNTUwYTJiMjJiMjAx",
> // 1. APP利用者ID連携申請のidentifyCodeを利用
>
> "path": "ZWE1YjhjYWYtOWQzZC00MWNlLTlhMTQtZTgzYWU3OWFlMzEy" // 3.
> URL発行のパスコードを利用
>
> }

レスポンス

> {
>
> "pxrId": "User00005",
>
> "userId": "User00005",
>
> "actor": {
>
> "\_value": 1000787,
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "status": 1, //
> 利用者連携の確認、凡例：0（申請中）、1（連携中）、2（解除）
>
> "startAt": "2022-06-22T13:47:09.652+0900"
>
> }

６. 個人：連携状況を確認。

| サービス名称     | API名称 | メソッドおよびパス                                                   |
|-------------------|----------|--------------------------------------------|
| Book管理サービス |         | GET pxr-block-proxy/ind/?block=$pxr-root-block&path=/book-manage/ind |

リクエスト

> なし

レスポンス

> {
>
> "status": 0,
>
> "identification": \[
>
> {
>
> "\_code": {
>
> "\_value": 30001,
>
> "\_ver": 1
>
> },
>
> "item-group": \[
>
> {
>
> "title": "氏名",
>
> "item": \[
>
> {
>
> "title": "姓",
>
> "type": {
>
> "\_value": 30019,
>
> "\_ver": 1
>
> },
>
> "content": "サンプル"
>
> },
>
> {
>
> "title": "名",
>
> "type": {
>
> "\_value": 30020,
>
> "\_ver": 1
>
> },
>
> "content": "太郎"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "性別",
>
> "item": \[
>
> {
>
> "title": "性別",
>
> "type": {
>
> "\_value": 30021,
>
> "\_ver": 1
>
> },
>
> "content": "男"
>
> }
>
> \]
>
> },
>
> {
>
> "title": "生年月日",
>
> "item": \[
>
> {
>
> "title": "生年月日",
>
> "type": {
>
> "\_value": 30022,
>
> "\_ver": 1
>
> },
>
> "content": "2000-01-01"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \],
>
> "cooperation": \[
>
> {
>
> "actor": {
>
> "\_value": 1000783,
>
> "\_ver": 3
>
> },
>
> "region": {
>
> "\_value": 1000816,
>
> "\_ver": 11
>
> },
>
> "userId": null,
>
> "status": 0 //
> Regionの連携状況、凡例：0（申請中）、1（連携中）、2（解除）★statusが0である場合、連携バッチが未実行の可能性あり
>
> },
>
> {
>
> "actor": {
>
> "\_value": 1000787,
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "userId": "User00005",
>
> "status": 1 //
> APPの連携状況、凡例：0（申請中）、1（連携中）、2（解除）★statusが0である場合、連携バッチが未実行の可能性あり
>
> }
>
> }
>
> \],
>
> "termsOfUse": {
>
> "platform": {
>
> "\_value": 1000782,
>
> "\_ver": 1
>
> },
>
> "region": \[
>
> {
>
> "actor": {
>
> "\_value": 1000783,
>
> "\_ver": 3
>
> },
>
> "region": {
>
> "\_value": 1000816,
>
> "\_ver": 11
>
> },
>
> "\_code": {
>
> "\_value": 1000815,
>
> "\_ver": 1
>
> }
>
> }
>
> \]
>
> },
>
> "appendix": {}
>
> }

###  6.9. <a name='-1'></a>**蓄積の定義**

１．流通制御サービスプロバイダー：蓄積コードを取得。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>カタログサービス</td>
<td>カタログ取得(code指定)</td>
<td><p>GET catalog/{code}</p>
<p>{code}：カタログ項目コード、6.4.8.
連携状況確認のresponseのappコードを利用</p></td>
</tr>
</tbody>
</table>

※事前に6.3.3流通制御サービスプロバイダー：ログインを実施すること。

リクエスト

> なし

レスポンス

> {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxx-service/actor/app/actor_1000787/application",
>
> "name": "myApp-A",
>
> "\_code": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "inherit": {
>
> "\_value": 41,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "myApp-A",
>
> "content": \[
>
> {
>
> "sentence": "myApp-A"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "\_code": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "information-site": null,
>
> "redirect_url": null,
>
> "region-alliance": \[
>
> {
>
> "\_value": 1000816,
>
> "\_ver": 8
>
> }
>
> \],
>
> "share": \[
>
> {
>
> "\_value": 1000881,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000907,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000933,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000935, // 共有同意時に利用
>
> "\_ver": 1
>
> }
>
> \],
>
> "store": \[
>
> {
>
> "\_value": 1000878, // 蓄積同意時に利用
>
> "\_ver": 2
>
> }
>
> \]
>
> },
>
> "prop": \[
>
> {
>
> "key": "information-site",
>
> "type": {
>
> "of": "string",
>
> "cmatrix": null,
>
> "format": null,
>
> "unit": null,
>
> "candidate": null
>
> },
>
> "description": "アプリケーションの情報サイト",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "redirect_url",
>
> "type": {
>
> "of": "string",
>
> "cmatrix": null,
>
> "format": null,
>
> "unit": null,
>
> "candidate": null
>
> },
>
> "description": "IDConnectログイン後のリダイレクト先URL",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "region-alliance",
>
> "type": {
>
> "of": "code\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": {
>
> "\_value": 48,
>
> "\_ver": 1
>
> }
>
> }
>
> },
>
> "description":
> "参加している領域運営サービスプロバイダーのリージョンコード配列",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "share",
>
> "type": {
>
> "of": "code\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": {
>
> "\_value": 40,
>
> "\_ver": 1
>
> }
>
> }
>
> },
>
> "description": "アプリケーションが提供する状態共有機能の定義",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "store",
>
> "type": {
>
> "of": "code\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": {
>
> "\_value": 39,
>
> "\_ver": 1
>
> }
>
> }
>
> },
>
> "description": "アプリケーションが蓄積可能なデータの定義",
>
> "isInherit": true
>
> }
>
> \],
>
> "value": \[
>
> {
>
> "key": "store",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000878
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000881
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000907
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000933
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000935
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "region-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000816
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 8
>
> }
>
> \]
>
> },
>
> {
>
> "key": "information-site",
>
> "value": null
>
> }
>
> \],
>
> "attribute": null
>
> }

２. 流通制御サービスプロバイダー：蓄積コードのstoreCatalogIdを取得。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>カタログサービス</td>
<td>カタログ取得(code指定)</td>
<td><p>GET catalog/{code}</p>
<p>{code}：カタログ項目コード、1.
蓄積、共有コード取得のresponseのstoreコードを利用</p></td>
</tr>
</tbody>
</table>

リクエスト

> なし

レスポンス

> {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxx-service/actor/app/actor_1000787/store",
>
> "name": "myApp-A",
>
> "\_code": {
>
> "\_value": 1000878,
>
> "\_ver": 2
>
> },
>
> "inherit": {
>
> "\_value": 39,
>
> "\_ver": 1
>
> },
>
> "description": "アプリケーションが蓄積可能なデータ定義です。"
>
> },
>
> "template": {
>
> "\_code": {
>
> "\_value": 1000878,
>
> "\_ver": 2
>
> },
>
> "store": \[
>
> {
>
> "id": "1806ea1251d3b1", // 蓄積同意時に利用
>
> "event": \[
>
> {
>
> "code": {
>
> "\_value": 1000800,
>
> "\_ver": 2
>
> },
>
> "requireConsent": true,
>
> "thing": \[
>
> {
>
> "code": {
>
> "\_value": 1000799,
>
> "\_ver": 1
>
> },
>
> "requireConsent": true
>
> }
>
> \]
>
> },
>
> {
>
> "code": {
>
> "\_value": 1000835,
>
> "\_ver": 2
>
> },
>
> "requireConsent": true,
>
> "thing": \[
>
> {
>
> "code": {
>
> "\_value": 1000834,
>
> "\_ver": 1
>
> },
>
> "requireConsent": true
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> {
>
> "id": "180db800f5c2a5",
>
> "event": \[
>
> {
>
> "code": {
>
> "\_value": 1000900,
>
> "\_ver": 1
>
> },
>
> "requireConsent": true,
>
> "thing": \[
>
> {
>
> "code": {
>
> "\_value": 1000897,
>
> "\_ver": 1
>
> },
>
> "requireConsent": true
>
> }
>
> \]
>
> },
>
> {
>
> "code": {
>
> "\_value": 1000899,
>
> "\_ver": 1
>
> },
>
> "requireConsent": true,
>
> "thing": \[
>
> {
>
> "code": {
>
> "\_value": 1000896,
>
> "\_ver": 1
>
> },
>
> "requireConsent": true
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> "prop": \[
>
> {
>
> "key": "store",
>
> "type": {
>
> "of": "inner\[\]",
>
> "inner": "Store",
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "蓄積定義",
>
> "isInherit": true
>
> }
>
> \],
>
> "value": \[
>
> {
>
> "key": "\_code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000878
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "store",
>
> "value": \[
>
> {
>
> "key": "id",
>
> "value": "1806ea1251d3b1"
>
> },
>
> {
>
> "key": "event",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000800
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> },
>
> {
>
> "key": "thing",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000799
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> {
>
> "key": "event",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000835
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> },
>
> {
>
> "key": "thing",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000834
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> {
>
> "key": "store",
>
> "value": \[
>
> {
>
> "key": "id",
>
> "value": "180db800f5c2a5"
>
> },
>
> {
>
> "key": "event",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000900
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> },
>
> {
>
> "key": "thing",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000897
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> {
>
> "key": "event",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000899
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> },
>
> {
>
> "key": "thing",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000896
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \],
>
> "attribute": null
>
> }

３．個人：蓄積の同意。

| サービス名称     | API名称            | メソッドおよびパス                                                               |
|-------------------|----------|--------------------------------------------|
| Book管理サービス | データ蓄積定義追加 | POST pxr-block-proxy/ind/?block=$pxr-root-block&path=/book-manage/settings/store |

※事前に6.7.3 個人：ログインを実施すること。

リクエスト　※storeCatalogIdの数分呼び出す必要がある。

> {
>
> "actor": {
>
> "\_value": 1000787, // 6.4.8 連携状況確認のresponseから取得
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879, // 6.4.8 連携状況確認のresponseから取得
>
> "\_ver": 7
>
> },
>
> "wf": null,
>
> "store": {
>
> "\_value": 1000878, // 6.5.1
> 蓄積、共有コードを取得のresponseのstoreから取得
>
> "\_ver": 2
>
> },
>
> "storeCatalogId": "1806ea1251d3b1", // 6.5.2
> 蓄積コードのstoreCatalogId取得のresponseのstoreのidから取得
>
> "excludeDocument": null,
>
> "excludeEvent": \[
>
> \]
>
> }

レスポンス

> {
>
> "result":"success"
>
> }

４．流通制御サービスプロバイダー：蓄積同意状況の確認（※必要に応じ実施）

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Book管理サービス</td>
<td>データ蓄積定義取得</td>
<td><p>GET book-manage/settings/store/{userId}</p>
<p>{userId}：利用者ID、3.
オペレータIDの取得のパラメータで指定したloginIdを利用</p></td>
</tr>
</tbody>
</table>

　　　※事前に6.2.1
流通制御サービスプロバイダー：ログインを実施すること。

リクエスト　※storeCatalogIdの数分呼び出す必要がある。

> なし

レスポンス

> \[
>
> {
>
> "id": 409,
>
> "actor": {
>
> "\_value": 1000787,
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "wf": null,
>
> "store": {
>
> "\_value": 1000878,
>
> "\_ver": 2
>
> },
>
> "storeCatalogId": "1806ea1251d3b1",
>
> "document": \[\],
>
> "event": \[
>
> {
>
> "\_code": {
>
> "\_value": 1000800,
>
> "\_ver": 2
>
> },
>
> "thing": \[
>
> {
>
> "\_code": {
>
> "\_value": 1000799,
>
> "\_ver": 1
>
> }
>
> }
>
> \]
>
> },
>
> {
>
> "\_code": {
>
> "\_value": 1000835,
>
> "\_ver": 2
>
> },
>
> "thing": \[
>
> {
>
> "\_code": {
>
> "\_value": 1000834,
>
> "\_ver": 1
>
> }
>
> }
>
> \]
>
> }
>
> \],
>
> "thing": null
>
> },
>
> {
>
> "id": 409,
>
> "actor": {
>
> "\_value": 1000787,
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "wf": null,
>
> "store": {
>
> "\_value": 1000878,
>
> "\_ver": 2
>
> },
>
> "storeCatalogId": "180db800f5c2a5",
>
> "document": \[\],
>
> "event": \[
>
> {
>
> "\_code": {
>
> "\_value": 1000900,
>
> "\_ver": 1
>
> },
>
> "thing": \[
>
> {
>
> "\_code": {
>
> "\_value": 1000897,
>
> "\_ver": 1
>
> }
>
> }
>
> \]
>
> },
>
> {
>
> "\_code": {
>
> "\_value": 1000899,
>
> "\_ver": 1
>
> },
>
> "thing": \[
>
> {
>
> "\_code": {
>
> "\_value": 1000896,
>
> "\_ver": 1
>
> }
>
> }
>
> \]
>
> }
>
> \],
>
> "thing": null
>
> }
>
> \]

###  6.10. <a name='-1'></a>**共有の定義**

１．流通制御サービスプロバイダー：共有コードを取得。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>カタログサービス</td>
<td>カタログ取得(code指定)</td>
<td><p>GET catalog/{code}</p>
<p>{code}：カタログ項目コード、6.4.8.
連携状況確認のresponseのappコードを利用</p></td>
</tr>
</tbody>
</table>

※事前に6.3.3流通制御サービスプロバイダー：ログインを実施すること。

リクエスト

> なし

レスポンス

> {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxx-service/actor/app/actor_1000787/application",
>
> "name": "myApp-A",
>
> "\_code": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "inherit": {
>
> "\_value": 41,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "myApp-A",
>
> "content": \[
>
> {
>
> "sentence": "myApp-A"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "\_code": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "information-site": null,
>
> "redirect_url": null,
>
> "region-alliance": \[
>
> {
>
> "\_value": 1000816,
>
> "\_ver": 8
>
> }
>
> \],
>
> "share": \[
>
> {
>
> "\_value": 1000881,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000907,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000933,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000935, // 共有同意時に利用
>
> "\_ver": 1
>
> }
>
> \],
>
> "store": \[
>
> {
>
> "\_value": 1000878, // 蓄積同意時に利用
>
> "\_ver": 2
>
> }
>
> \]
>
> },
>
> "prop": \[
>
> {
>
> "key": "information-site",
>
> "type": {
>
> "of": "string",
>
> "cmatrix": null,
>
> "format": null,
>
> "unit": null,
>
> "candidate": null
>
> },
>
> "description": "アプリケーションの情報サイト",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "redirect_url",
>
> "type": {
>
> "of": "string",
>
> "cmatrix": null,
>
> "format": null,
>
> "unit": null,
>
> "candidate": null
>
> },
>
> "description": "IDConnectログイン後のリダイレクト先URL",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "region-alliance",
>
> "type": {
>
> "of": "code\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": {
>
> "\_value": 48,
>
> "\_ver": 1
>
> }
>
> }
>
> },
>
> "description":
> "参加している領域運営サービスプロバイダーのリージョンコード配列",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "share",
>
> "type": {
>
> "of": "code\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": {
>
> "\_value": 40,
>
> "\_ver": 1
>
> }
>
> }
>
> },
>
> "description": "アプリケーションが提供する状態共有機能の定義",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "store",
>
> "type": {
>
> "of": "code\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": null,
>
> "base": {
>
> "\_value": 39,
>
> "\_ver": 1
>
> }
>
> }
>
> },
>
> "description": "アプリケーションが蓄積可能なデータの定義",
>
> "isInherit": true
>
> }
>
> \],
>
> "value": \[
>
> {
>
> "key": "store",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000878
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000881
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000907
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000933
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000935
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 1
>
> }
>
> \]
>
> },
>
> {
>
> "key": "region-alliance",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000816
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 8
>
> }
>
> \]
>
> },
>
> {
>
> "key": "information-site",
>
> "value": null
>
> }
>
> \],
>
> "attribute": null
>
> }

２．流通制御サービスプロバイダー：共有コードのshareCatalogIdを取得。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>カタログサービス</td>
<td>カタログ取得(code指定)</td>
<td><p>GET catalog/{code}</p>
<p>{code}：カタログ項目コード、1.
蓄積、共有コード取得のresponseのshareコードを利用</p></td>
</tr>
</tbody>
</table>

リクエスト

> なし

レスポンス

> {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxx-service/actor/app/actor_1000787/share",
>
> "name": "状態共有-1 ",
>
> "\_code": {
>
> "\_value": 1000935,
>
> "\_ver": 1
>
> },
>
> "inherit": {
>
> "\_value": 45,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "状態共有-1",
>
> "content": \[
>
> {
>
> "sentence": "状態共有-1"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "\_code": {
>
> "\_value": 1000935,
>
> "\_ver": 1
>
> },
>
> "share": \[
>
> {
>
> "id": "54549512-35c3-4f10-9eaf-632c3af94db9", // 共有同意時に利用
>
> "role": null,
>
> "event": \[
>
> {
>
> "code": {
>
> "\_value": 1000927,
>
> "\_ver": 3
>
> },
>
> "thing": \[
>
> {
>
> "code": {
>
> "\_value": 1000926,
>
> "\_ver": 3
>
> },
>
> "requireConsent": true
>
> }
>
> \],
>
> "requireConsent": true
>
> },
>
> {
>
> "code": {
>
> "\_value": 1000927,
>
> "\_ver": 3
>
> },
>
> "thing": \[
>
> {
>
> "code": {
>
> "\_value": 1000926,
>
> "\_ver": 3
>
> },
>
> "requireConsent": true
>
> }
>
> \],
>
> "requireConsent": true,
>
> "sourceActor": \[
>
> {
>
> "\_value": 1000785,
>
> "\_ver": 6
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> },
>
> "prop": \[
>
> {
>
> "key": "share",
>
> "type": {
>
> "of": "inner\[\]",
>
> "inner": "Share",
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "状態共有機能定義",
>
> "isInherit": true
>
> }
>
> \],
>
> "value": \[
>
> {
>
> "key": "share",
>
> "value": \[
>
> {
>
> "key": "id",
>
> "value": "54549512-35c3-4f10-9eaf-632c3af94db9"
>
> },
>
> {
>
> "key": "role",
>
> "value": null
>
> },
>
> {
>
> "key": "event",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000927
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 3
>
> }
>
> \]
>
> },
>
> {
>
> "key": "thing",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000926
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 3
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> }
>
> \]
>
> },
>
> {
>
> "key": "event",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000927
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 3
>
> }
>
> \]
>
> },
>
> {
>
> "key": "thing",
>
> "value": \[
>
> {
>
> "key": "code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000926
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 3
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> }
>
> \]
>
> },
>
> {
>
> "key": "requireConsent",
>
> "value": true
>
> },
>
> {
>
> "key": "sourceActor",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": "1000785"
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": "6"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> \],
>
> "attribute": null
>
> }

３．個人：共有の同意。

| サービス名称     | API名称            | メソッドおよびパス                                                                  |
|-------------------|----------|--------------------------------------------|
| Book管理サービス | データ共有定義追加 | POST pxr-block-proxy/ind/?block=$pxr-root-block&path=/book-manage/ind/setting/share |

※事前に6.3.3個人：ログインを実施すること。

リクエスト　※状態共有機能及び、shareCatalogIdの数分呼び出す必要がある。

> {
>
> "actor": {
>
> "\_value": 1000787, // 6.4.8 連携状況確認のresponseから取得
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879, // 6.4.8 連携状況確認のresponseから取得
>
> "\_ver": 7
>
> },
>
> "wf": null,
>
> "share": {
>
> "\_value": 1000935, // 6.5.1
> 蓄積、共有コードを取得のresponseのshareから取得
>
> "\_ver": 1
>
> },
>
> "shareCatalogId": "54549512-35c3-4f10-9eaf-632c3af94db9", // 6.5.2
> 共有コードのshareCatalogId取得のresponseのshareのidから取得
>
> "excludeDocument": null,
>
> "excludeEvent": \[
>
> \]
>
> }

レスポンス

> {
>
> "result":"success"
>
> }

４．共有同意状況確認（※必要に応じ実施）

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Book管理サービス</td>
<td>データ共有定義取得</td>
<td><p>GET
book-manage/ind/setting/share?id={userId}&amp;app={appCode}</p>
<p>{userId}：利用者ID、3.
オペレータIDの取得のパラメータで指定したloginIdを利用</p>
<p>{appCode}：アプリケーションカタログ項目コード、16.
連携状況確認のresponseのappを利用</p></td>
</tr>
</tbody>
</table>

　　　事前に6.2.1 流通制御サービスプロバイダー：ログインを実施すること。

リクエスト　※状態共有機能及び、shareCatalogIdの数分呼び出す必要がある。

> なし

レスポンス

> \[
>
> {
>
> "id": 410,
>
> "actor": {
>
> "\_value": 1000787,
>
> "\_ver": 3
>
> },
>
> "app": {
>
> "\_value": 1000879,
>
> "\_ver": 7
>
> },
>
> "wf": null,
>
> "share": {
>
> "\_value": 1000935,
>
> "\_ver": 1
>
> },
>
> "shareCatalogId": "54549512-35c3-4f10-9eaf-632c3af94db9",
>
> "document": null,
>
> "event": \[
>
> {
>
> "\_code": {
>
> "\_value": 1000927,
>
> "\_ver": 3
>
> },
>
> "thing": \[
>
> {
>
> "\_code": {
>
> "\_value": 1000926,
>
> "\_ver": 3
>
> }
>
> },
>
> {
>
> "\_code": {
>
> "\_value": 1000926,
>
> "\_ver": 3
>
> }
>
> }
>
> \]
>
> }
>
> \],
>
> "thing": null
>
> }
>
> \]

###  6.11. <a name='My-Condition-Data'></a>**My-Condition-Dataの蓄積**

１．アプリケーションプロバイダー：利用者一覧を取得。

※事前に6.4.1アプリケーションプロバイダー：ログインを実施すること。

| サービス名称     | API名称        | メソッドおよびパス                                                  |
|-------------------|----------|--------------------------------------------|
| Book運用サービス | 利用者一覧検索 | POST /pxr-block-proxy/pxr-block-proxy/?path=/book-operate/user/list |

リクエスト

> {
>
> "userId": \[
>
> "useTestUser000002"
>
> \],
>
> "establishAt": {
>
> "start": null,
>
> "end": null
>
> }
>
> }

レスポンス

> {
>
> {
>
> "status": 1,
>
> "app": {
>
> "\_value": 1000879,
>
> "\_ver": 3
>
> },
>
> "wf": null,
>
> "userId": "useTestUser000002",
>
> "establishAt": "2022-04-28T14:42:26.000+0900",
>
> "attribute": {},
>
> "store": {
>
> "document": \[\],
>
> "event": \[
>
> {
>
> "\_value": 1000800,
>
> "\_ver": 2
>
> },
>
> {
>
> "\_value": 1000835,
>
> "\_ver": 2
>
> }
>
> \],
>
> "thing": \[
>
> {
>
> "\_value": 1000799,
>
> "\_ver": 1
>
> },
>
> {
>
> "\_value": 1000834,
>
> "\_ver": 1
>
> }
>
> \]
>
> },
>
> "userInformation": null
>
> }
>
> }

２．アプリケーションプロバイダー：ドキュメントの蓄積。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Book運用サービス</td>
<td>ドキュメント蓄積</td>
<td><p>POST
/pxr-block-proxy/pxr-block-proxy/?path=/book-operate/document/{userId}</p>
<p>{userId}: 蓄積対象の利用者ID</p></td>
</tr>
</tbody>
</table>

リクエスト

> {
>
> "\_code": {
>
> "\_value": 1000785, // ドキュメントコード
>
> "\_ver": 4
>
> },
>
> "wf": {
>
> "code": {
>
> "index": "2_3_1",
>
> "value": {
>
> "\_value": 1000785,
>
> "\_ver": 1
>
> }
>
> },
>
> "wf": {
>
> "index": "2_3_2",
>
> "value": {
>
> "\_value": 1000862,
>
> "\_ver": 5
>
> }
>
> },
>
> "role": {
>
> "index": "2_3_3",
>
> "value": {
>
> "\_value": 1000864,
>
> "\_ver": 5
>
> }
>
> },
>
> "staffId": {
>
> "index": "2_3_4",
>
> "value": "wf01user04"
>
> }
>
> },
>
> "chapter": \[
>
> {
>
> "title": "イベント識別子",
>
> "event": \[
>
> "c7ae8e8c-1174-4973-8b98-8999f8811778"
>
> \],
>
> "sourceId": \[\]
>
> }
>
> \],
>
> "code": {
>
> "index": "2_1_2",
>
> "value": {
>
> "\_value": 1000833, // ドキュメントコード
>
> "\_ver": 4
>
> }
>
> },
>
> "createdAt": {
>
> "index": "2_2_1",
>
> "value": "2020-02-20T00:00:00.000+0900"
>
> },
>
> "id": {
>
> "index": "2_1_1",
>
> "value": null
>
> },
>
> "sourceId": "202108-1",
>
> "app": null,
>
> "userId": {
>
> "index": "3_6_1",
>
> "value": "useTestUser000001"
>
> }
>
> }

レスポンス

> {
>
> "\_code": {
>
> "\_value": 1000833,
>
> "\_ver": 4
>
> },
>
> "wf": {
>
> "code": {
>
> "index": "2_3_1",
>
> "value": {
>
> "\_value": 1000785,
>
> "\_ver": 1
>
> }
>
> },
>
> "wf": {
>
> "index": "2_3_2",
>
> "value": {
>
> "\_value": 1000862,
>
> "\_ver": 5
>
> }
>
> },
>
> "role": {
>
> "index": "2_3_3",
>
> "value": {
>
> "\_value": 1000864,
>
> "\_ver": 5
>
> }
>
> },
>
> "staffId": {
>
> "index": "2_3_4",
>
> "value": "wf01user04"
>
> }
>
> },
>
> "chapter": \[
>
> {
>
> "title": "test",
>
> "event": \[
>
> "c7ae8e8c-1174-4973-8b98-8999f8811778"
>
> \],
>
> "sourceId": \[\]
>
> }
>
> \],
>
> "code": {
>
> "index": "2_1_2",
>
> "value": {
>
> "\_value": 1000833,
>
> "\_ver": 4
>
> }
>
> },
>
> "createdAt": {
>
> "index": "2_2_1",
>
> "value": "2020-02-20T00:00:00.000+0900"
>
> },
>
> "id": {
>
> "index": "2_1_1",
>
> "value": "e07f4abb-84d7-476f-be10-1ee2e8d6fc3b"
>
> },
>
> "sourceId": "202108-1",
>
> "app": null,
>
> "userId": {
>
> "index": "3_6_1",
>
> "value": "useTestUser000001"
>
> }
>
> }

３．アプリケーションプロバイダー：イベントの蓄積。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Book運用サービス</td>
<td>イベント蓄積</td>
<td><p>POST
/pxr-block-proxy/pxr-block-proxy/?path=/book-operate/event/{userId}</p>
<p>{userId}: 蓄積対象の利用者ID</p></td>
</tr>
</tbody>
</table>

リクエスト

> {
>
> "\_code": {
>
> "\_value": 1000800, // イベントコード
>
> "\_ver": 2
>
> },
>
> "app": {
>
> "code": {
>
> "index": "2_3_1",
>
> "value": {
>
> "\_value": 1000787, // アクターコード
>
> "\_ver": 1
>
> }
>
> },
>
> "app": {
>
> "index": "2_3_2",
>
> "value": {
>
> "\_value": 1000809, // アプリケーションコード
>
> "\_ver": 3
>
> }
>
> }
>
> },
>
> "code": {
>
> "index": "3_1_2",
>
> "value": {
>
> "\_value": 1000800, // イベントコード
>
> "\_ver": 2
>
> }
>
> },
>
> "end": {
>
> "index": "3_2_2",
>
> "value": null
>
> },
>
> "env": null,
>
> "id": {
>
> "index": "3_1_1",
>
> "value": null
>
> },
>
> "location": {
>
> "index": "3_3_1",
>
> "value": null
>
> },
>
> "sourceId": null,
>
> "start": {
>
> "index": "3_2_1",
>
> "value": null
>
> },
>
> "thing": null,
>
> "userId": {
>
> "index": "3_6_1",
>
> "value": "useTestUser000001" // 利用者ID
>
> },
>
> "wf": null
>
> }

レスポンス

> {
>
> "\_code": {
>
> "\_value": 1000800,
>
> "\_ver": 2
>
> },
>
> "app": {
>
> "code": {
>
> "index": "2_3_1",
>
> "value": {
>
> "\_value": 1000787,
>
> "\_ver": 1
>
> }
>
> },
>
> "app": {
>
> "index": "2_3_2",
>
> "value": {
>
> "\_value": 1000809,
>
> "\_ver": 3
>
> }
>
> }
>
> },
>
> "code": {
>
> "index": "3_1_2",
>
> "value": {
>
> "\_value": 1000800,
>
> "\_ver": 2
>
> }
>
> },
>
> "end": {
>
> "index": "3_2_2",
>
> "value": null
>
> },
>
> "env": null,
>
> "id": {
>
> "index": "3_1_1",
>
> "value": "0b6398e8-4b65-422e-af50-4ae86fc33a95" //
> イベントID（モノの蓄積で必要になる）
>
> },
>
> "location": {
>
> "index": "3_3_1",
>
> "value": null
>
> },
>
> "sourceId": null,
>
> "start": {
>
> "index": "3_2_1",
>
> "value": null
>
> },
>
> "thing": null,
>
> "userId": {
>
> "index": "3_6_1",
>
> "value": "useTestUser000001"
>
> },
>
> "wf": null
>
> }

４．アプリケーションプロバイダー：モノの蓄積。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Book運用サービス</td>
<td>モノ追加</td>
<td><p>POST
/pxr-block-proxy/pxr-block-proxy/?path=/book-operate/thing/{userId}/{eventId}</p>
<p>{userId}: 利用者ID</p>
<p>{eventId}: イベント蓄積のレスポンス</p></td>
</tr>
</tbody>
</table>

リクエスト

> {
>
> "\_code": {
>
> "\_value": 1000799,
>
> "\_ver": 1
>
> },
>
> "app": {
>
> "code": {
>
> "index": "2_3_1",
>
> "value": {
>
> "\_value": 1000787,
>
> "\_ver": 1
>
> }
>
> },
>
> "app": {
>
> "index": "2_3_2",
>
> "value": {
>
> "\_value": 1000809,
>
> "\_ver": 3
>
> }
>
> }
>
> },
>
> "code": {
>
> "index": "4_1_2",
>
> "value": {
>
> "\_value": 1000799,
>
> "\_ver": 1
>
> }
>
> },
>
> "env": \[
>
> {
>
> "index": "4_3_1",
>
> "value": {}
>
> }
>
> \],
>
> "envelope": {
>
> "resourceType": "settlementResult",
>
> "identifier": \[
>
> {
>
> "system": "urn:uuid",
>
> "value": "550e8400-e29b-41d4-a716-446655440000"
>
> }
>
> \],
>
> "subject": {
>
> "reference": "ID/12345678"
>
> },
>
> "issuedate": "20211201",
>
> "class": "outID",
>
> "classhistory": {
>
> "class": "outID",
>
> "period": {
>
> "start": "20211201",
>
> "end": "20211201"
>
> }
>
> },
>
> "referenceNumber": "999999",
>
> "invoiceSerialNumber": "999999",
>
> "invoiceBranceNumber": "999999,",
>
> "departmentCode": "99",
>
> "paymentClass": "1",
>
> "totalBilled": "9999",
>
> "depositThisTime": "9999",
>
> "lastDeposit": "9999"
>
> },
>
> "id": {
>
> "index": "4_1_1",
>
> "value": null
>
> },
>
> "sourceId": null,
>
> "wf": null
>
> }

レスポンス

> {
>
> "\_code": {
>
> "\_value": 1000799,
>
> "\_ver": 1
>
> },
>
> "app": {
>
> "code": {
>
> "index": "2_3_1",
>
> "value": {
>
> "\_value": 1000787,
>
> "\_ver": 1
>
> }
>
> },
>
> "app": {
>
> "index": "2_3_2",
>
> "value": {
>
> "\_value": 1000809,
>
> "\_ver": 3
>
> }
>
> }
>
> },
>
> "code": {
>
> "index": "4_1_2",
>
> "value": {
>
> "\_value": 1000799,
>
> "\_ver": 1
>
> }
>
> },
>
> "env": \[
>
> {
>
> "index": "4_3_1",
>
> "value": {}
>
> }
>
> \],
>
> "envelope": {
>
> "resourceType": "settlementResult",
>
> "identifier": \[
>
> {
>
> "system": "urn:uuid",
>
> "value": "550e8400-e29b-41d4-a716-446655440000"
>
> }
>
> \],
>
> "subject": {
>
> "reference": "ID/12345678"
>
> },
>
> "issuedate": "20211201",
>
> "class": "outID",
>
> "classhistory": {
>
> "class": "outID",
>
> "period": {
>
> "start": "20211201",
>
> "end": "20211201"
>
> }
>
> },
>
> "referenceNumber": "999999",
>
> "invoiceSerialNumber": "999999",
>
> "invoiceBranceNumber": "999999,",
>
> "departmentCode": "99",
>
> "paymentClass": "1",
>
> "totalBilled": "9999",
>
> "depositThisTime": "9999",
>
> "lastDeposit": "9999"
>
> },
>
> "id": {
>
> "index": "4_1_1",
>
> "value": "15acadac-94c9-469a-be41-f390520f46c9"
>
> },
>
> "sourceId": null,
>
> "wf": null
>
> }

###  6.12. <a name='My-Condition-Data-1'></a>**My-Condition-Dataの共有**

事前に6.8.1アプリケーションプロバイダー：ログインを実施すること。

１．アプリケーションプロバイダー：共有のリクエスト。

※事前に6.4.1アプリケーションプロバイダー：ログインを実施すること。

| サービス名称     | API名称                    | メソッドおよびパス                                              |
|-------------------|----------|--------------------------------------------|
| Book運用サービス | データ共有によるデータ取得 | POST /pxr-block-proxy/pxr-block-proxy/?path=/book-operate/share |

リクエスト

> {
>
> "userId": "useTestUser000002",
>
> "event": \[
>
> {
>
> "\_value": 1000875,
>
> "\_ver": 1
>
> }
>
> \]
>
> }

レスポンス

> {
>
> {
>
> "document": null,
>
> "event": \[
>
> {
>
> "id": {
>
> "index": "3_1_1",
>
> "value": "267a2c83-f58d-45fa-9a34-b25a30d8c83e"
>
> },
>
> "code": {
>
> "index": "3_1_2",
>
> "value": {
>
> "\_value": 1000875,
>
> "\_ver": 1
>
> }
>
> },
>
> "start": {
>
> "index": "3_2_1",
>
> "value": null
>
> },
>
> "end": {
>
> "index": "3_2_2",
>
> "value": null
>
> },
>
> "location": {
>
> "index": "3_3_1",
>
> "value": null
>
> },
>
> "sourceId": null,
>
> "env": null,
>
> "app": null,
>
> "wf": {
>
> "code": {
>
> "index": "3_5_1",
>
> "value": {
>
> "\_value": 1000785,
>
> "\_ver": 1
>
> }
>
> },
>
> "wf": {
>
> "index": "3_5_2",
>
> "value": {
>
> "\_value": 1000862,
>
> "\_ver": 5
>
> }
>
> },
>
> "role": {
>
> "index": "3_5_3",
>
> "value": {
>
> "\_value": 1000864,
>
> "\_ver": 5
>
> }
>
> }
>
> },
>
> "thing": \[
>
> {
>
> "app": null,
>
> "code": {
>
> "index": "4_1_2",
>
> "value": {
>
> "\_value": 1000874,
>
> "\_ver": 1
>
> }
>
> },
>
> "env": \[
>
> {
>
> "index": "4_3_1",
>
> "value": null
>
> }
>
> \],
>
> "envelope": {
>
> "resourceType": "Bundle",
>
> "id": "01312345",
>
> "type": "document",
>
> "timestamp": "2018-12-13T17:36:48+09:00",
>
> "entry": \[
>
> {
>
> "resource": {
>
> "resourceType": "QuestionnaireResponse",
>
> "identifier": {
>
> "system": "https://xxxxxxxxx.com/ ",
>
> "value": "550e8400-e29b-41d4-a716-446655440000"
>
> },
>
> "questionnaire": "Questionnaire",
>
> "status": "completed",
>
> "subject": {
>
> "reference": "ID/12345678",
>
> "type": "ID"
>
> },
>
> "authored": "2019-10-09T15:58:01.0724504+09:00",
>
> "item": \[
>
> {
>
> "linkId": "issueDate",
>
> "answer": \[
>
> {
>
> "valueString": "20220322"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "startDate",
>
> "answer": \[
>
> {
>
> "valueString": "20220322"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "endDate",
>
> "answer": \[
>
> {
>
> "valueString": "20220622"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "IDClass",
>
> "answer": \[
>
> {
>
> "valueString": "1"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "referenceNumber",
>
> "answer": \[
>
> {
>
> "valueString": "999999"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "invoiceSerialNumber",
>
> "answer": \[
>
> {
>
> "valueString": "999999"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "invoiceBranchNumber",
>
> "answer": \[
>
> {
>
> "valueString": "99"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "departmentCode",
>
> "answer": \[
>
> {
>
> "valueString": "99"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "paymentClass",
>
> "answer": \[
>
> {
>
> "valueString": "102"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "totalBilled",
>
> "answer": \[
>
> {
>
> "valueString": "99999999"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "deposit",
>
> "answer": \[
>
> {
>
> "valueString": "99999999"
>
> }
>
> \]
>
> },
>
> {
>
> "linkId": "lastDeposit",
>
> "answer": \[
>
> {
>
> "valueString": "99999999"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> {
>
> "resource": {
>
> "resourceType": "Organization",
>
> "identifier": \[
>
> {
>
> "system": "https://xxxxxxxxx.com/ ",
>
> "value": "1311234567"
>
> }
>
> \],
>
> "type": \[
>
> {
>
> "coding": \[
>
> {
>
> "system": " https://xxxxxxxxx.com/,
>
> "code": "prov"
>
> }
>
> \]
>
> }
>
> \],
>
> "name": "AAA施設
>
> }
>
> }
>
> \]
>
> },
>
> "id": {
>
> "index": "4_1_1",
>
> "value": "5e657d2b-a23d-411b-8a45-d73e2a66eac2"
>
> },
>
> "sourceId": null,
>
> "wf": {
>
> "code": {
>
> "index": "2_3_1",
>
> "value": {
>
> "\_value": 1000785,
>
> "\_ver": 1
>
> }
>
> },
>
> "wf": {
>
> "index": "2_3_2",
>
> "value": {
>
> "\_value": 1000862,
>
> "\_ver": 5
>
> }
>
> },
>
> "role": {
>
> "index": "2_3_3",
>
> "value": {
>
> "\_value": 1000864,
>
> "\_ver": 5
>
> }
>
> },
>
> "staffId": {
>
> "index": "2_3_4",
>
> "value": "wf01user04"
>
> }
>
> }
>
> }
>
> \]
>
> }
>
> \],
>
> "thing": null
>
> }
>
> }

###  6.13. <a name='-1'></a>**個人の離脱**

１．流通制御サービスプロバイダー：Book閉鎖を実施。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 14%" />
<col style="width: 59%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Book管理サービス</td>
<td>Book閉鎖</td>
<td><p>DELETE
/pxr-block-proxy/pxr-block-proxy/?path=/book-manage/users/{userId}</p>
<p>{userId}: 閉鎖対象の利用者ID</p></td>
</tr>
</tbody>
</table>

※事前に6.3.3流通制御サービスプロバイダー：ログインを実施すること。

リクエスト

> なし

レスポンス

> {
>
> "myConditionDateOutCode": "xxx"
>
> }

###  6.14. <a name='Region-1'></a>**Region終了**

別紙『パーソナルデータ連携モジュール
利用設定手順書』の「5.PxR-Blockの削除方法」を参照してください。

###  6.15. <a name='-1'></a>**アクター終了**

別紙『パーソナルデータ連携モジュール
利用設定手順書』の「5.PxR-Blockの削除方法」を参照してください。

###  6.16. <a name='-1'></a>**【参考】データカタログの取得**

１．最新テンプレートを取得する例。

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 11%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>カタログサービス</td>
<td>カタログ取得(code指定)</td>
<td><p>GET
/pxr-block-proxy/pxr-block-proxy/?block=$pxr-root-block&amp;path=/catalog/{code}</p>
<p>{Code}: 取得対象のカタログコード</p></td>
</tr>
</tbody>
</table>

リクエスト

> なし

レスポンス

> {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxx-service/event/actor_1000785",
>
> "name": "サンプルイベント",
>
> "\_code": {
>
> "\_value": 1000875,
>
> "\_ver": 1
>
> },
>
> "inherit": {
>
> "\_value": 53,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "サンプルイベント",
>
> "content": \[
>
> {
>
> "sentence": "サンプルイベントの定義です。"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "\_code": {
>
> "\_value": 1000875,
>
> "\_ver": 1
>
> },
>
> "app": null,
>
> "code": {
>
> "index": "3_1_2",
>
> "value": null
>
> },
>
> "end": {
>
> "index": "3_2_2",
>
> "value": null
>
> },
>
> "env": null,
>
> "id": {
>
> "index": "3_1_1",
>
> "value": null
>
> },
>
> "location": {
>
> "index": "3_3_1",
>
> "value": null
>
> },
>
> "sourceId": null,
>
> "start": {
>
> "index": "3_2_1",
>
> "value": null
>
> },
>
> "thing": null,
>
> "userId": {
>
> "index": "3_6_1",
>
> "value": null
>
> },
>
> "wf": null
>
> },
>
> "prop": \[
>
> {
>
> "key": "app",
>
> "type": {
>
> "of": "inner",
>
> "inner": "App",
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベントを発生させたアプリケーションプロバイダー",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "code",
>
> "type": {
>
> "of": "item",
>
> "\_code": {
>
> "\_value": 18,
>
> "\_ver": 1
>
> },
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント種別",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "end",
>
> "type": {
>
> "of": "item",
>
> "\_code": {
>
> "\_value": 20,
>
> "\_ver": 1
>
> },
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント終了時刻",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "env",
>
> "type": {
>
> "of": "item\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": \[
>
> "catalog/model/env/event/\*",
>
> "catalog/built_in/env/event/\*",
>
> "catalog/ext/xxxxx-service/env/event/\*"
>
> \],
>
> "\_code": null,
>
> "base": null
>
> }
>
> },
>
> "description": "イベント環境の配列",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "id",
>
> "type": {
>
> "of": "item",
>
> "\_code": {
>
> "\_value": 17,
>
> "\_ver": 1
>
> },
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント識別子",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "location",
>
> "type": {
>
> "of": "item",
>
> "\_code": {
>
> "\_value": 21,
>
> "\_ver": 1
>
> },
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント発生位置",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "sourceId",
>
> "type": {
>
> "of": "string",
>
> "cmatrix": null,
>
> "format": null,
>
> "unit": null,
>
> "candidate": null
>
> },
>
> "description": "イベントのソースID",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "start",
>
> "type": {
>
> "of": "item",
>
> "\_code": {
>
> "\_value": 19,
>
> "\_ver": 1
>
> },
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント開始時刻",
>
> "isInherit": true
>
> },
>
> {
>
> "key": "thing",
>
> "type": {
>
> "of": "item\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": \[
>
> "catalog/model/thing/\*",
>
> "catalog/built_in/thing/\*",
>
> "catalog/ext/xxxxx-service/thing/\*"
>
> \],
>
> "\_code": \[
>
> {
>
> "\_value": 1000874,
>
> "\_ver": 1
>
> }
>
> \],
>
> "base": null
>
> }
>
> },
>
> "description": "モノの配列",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "userId",
>
> "type": {
>
> "of": "item",
>
> "\_code": {
>
> "\_value": 221,
>
> "\_ver": 1
>
> },
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベントを発生させた個人識別子",
>
> "isInherit": true
>
> }
>
> \],
>
> "value": null,
>
> "attribute": null
>
> }

２．バージョンを指定してテンプレートを取得する例。

<table>
<colgroup>
<col style="width: 24%" />
<col style="width: 14%" />
<col style="width: 61%" />
</colgroup>
<thead>
<tr class="header">
<th>サービス名称</th>
<th>API名称</th>
<th>メソッドおよびパス</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>カタログサービス</td>
<td>カタログ取得(code,version指定)</td>
<td><p>GET
/pxr-block-proxy/pxr-block-proxy/?block=$pxr-root-block&amp;path=/catalog/{code}/{ver}</p>
<p>{Code}: 取得対象のカタログコード</p>
<p>{ver}: 取得対象のカタログバージョン</p></td>
</tr>
</tbody>
</table>

リクエスト

> なし

レスポンス

> {
>
> "catalogItem": {
>
> "ns": "catalog/ext/xxxxx-service/event/actor_1000785",
>
> "name": "サンプルイベント",
>
> "\_code": {
>
> "\_value": 1000794,
>
> "\_ver": 3
>
> },
>
> "inherit": {
>
> "\_value": 53,
>
> "\_ver": 1
>
> },
>
> "description": {
>
> "title": null,
>
> "section": \[
>
> {
>
> "title": "サンプルイベント",
>
> "content": \[
>
> {
>
> "sentence": "サンプルイベントです。"
>
> }
>
> \]
>
> }
>
> \]
>
> }
>
> },
>
> "template": {
>
> "\_code": {
>
> "\_value": 1000794,
>
> "\_ver": 3
>
> },
>
> "app": null,
>
> "code": {},
>
> "end": {},
>
> "env": null,
>
> "id": {},
>
> "location": {},
>
> "sourceId": "",
>
> "start": {},
>
> "thing": null,
>
> "userId": {
>
> "index": "3_6_1",
>
> "value": null
>
> },
>
> "wf": null
>
> },
>
> "prop": \[
>
> {
>
> "key": "app",
>
> "type": {
>
> "of": "inner",
>
> "inner": null,
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベントを発生させたアプリケーションプロバイダー",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "code",
>
> "type": {
>
> "of": "item",
>
> "\_code": null,
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント種別",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "end",
>
> "type": {
>
> "of": "item",
>
> "\_code": null,
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント終了時刻",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "env",
>
> "type": {
>
> "of": "item\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": \[
>
> "catalog/model/env/event/\*",
>
> "catalog/built_in/env/event/\*",
>
> "catalog/ext/xxxxx-service/env/event/\*"
>
> \],
>
> "\_code": null,
>
> "base": null
>
> }
>
> },
>
> "description": "イベント環境の配列",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "id",
>
> "type": {
>
> "of": "item",
>
> "\_code": null,
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント識別子",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "location",
>
> "type": {
>
> "of": "item",
>
> "\_code": null,
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント発生位置",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "sourceId",
>
> "type": {
>
> "of": "string",
>
> "cmatrix": null,
>
> "format": null,
>
> "unit": null,
>
> "candidate": null
>
> },
>
> "description": "イベントのソースID",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "start",
>
> "type": {
>
> "of": "item",
>
> "\_code": null,
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベント開始時刻",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "thing",
>
> "type": {
>
> "of": "item\[\]",
>
> "cmatrix": null,
>
> "candidate": {
>
> "ns": null,
>
> "\_code": \[
>
> {
>
> "\_value": 1000795,
>
> "\_ver": 1
>
> }
>
> \],
>
> "base": null
>
> }
>
> },
>
> "description": "モノの配列",
>
> "isInherit": false
>
> },
>
> {
>
> "key": "userId",
>
> "type": {
>
> "of": "item",
>
> "\_code": {
>
> "\_value": 221,
>
> "\_ver": 1
>
> },
>
> "cmatrix": null,
>
> "candidate": null
>
> },
>
> "description": "イベントを発生させた個人識別子",
>
> "isInherit": true
>
> }
>
> \],
>
> "value": \[
>
> {
>
> "key": "\_code",
>
> "value": \[
>
> {
>
> "key": "\_value",
>
> "value": 1000794
>
> },
>
> {
>
> "key": "\_ver",
>
> "value": 2
>
> }
>
> \]
>
> },
>
> {
>
> "key": "app",
>
> "value": null
>
> },
>
> {
>
> "key": "code",
>
> "value": \[\]
>
> },
>
> {
>
> "key": "end",
>
> "value": \[\]
>
> },
>
> {
>
> "key": "env",
>
> "value": null
>
> },
>
> {
>
> "key": "id",
>
> "value": \[\]
>
> },
>
> {
>
> "key": "location",
>
> "value": \[\]
>
> },
>
> {
>
> "key": "sourceId",
>
> "value": ""
>
> },
>
> {
>
> "key": "start",
>
> "value": \[\]
>
> },
>
> {
>
> "key": "thing",
>
> "value": null
>
> },
>
> {
>
> "key": "wf",
>
> "value": null
>
> }
>
> \],
>
> "attribute": null
>
> }
