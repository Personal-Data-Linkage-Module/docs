# パーソナルデータ連携モジュール pxr-ctoken-ledger-service ビルド手順書

# 目次
<!-- vscode-markdown-toc -->
* 1. [はじめに](#)
	* 1.1. [前提条件](#-1)
* 2. [ビルド手順](#-1)
	* 2.1. [サービスをビルドする](#-1)
* 3. [Unit Test 手順](#UnitTest)
	* 3.1. [DB を作成する（1環境につき初回のみ）](#DB1)
	* 3.2. [Schema, Tableを作成する（1環境につき初回のみ）](#SchemaTable1)
	* 3.3. [Unit Testを実行する](#UnitTest-1)
* 4. [pxr-ctoken-ledger-service起動手順](#pxr-ctoken-ledger-service)
	* 4.1. [pxr-ctoken-ledger-serviceを起動する](#pxr-ctoken-ledger-service-1)
	* 4.2. [Webブラウザでアクセスする](#Web)
* 5. [Dockerコンテナイメージ作成手順](#Docker)
	* 5.1. [Dockerコンテナイメージを作成する](#Docker-1)
	* 5.2. [Dockerコンテナイメージをレジストリに登録する](#Docker-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=false
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->


##  1. <a name=''></a>はじめに
本書は、パーソナルデータ連携基盤の一部である、pxr-ctoken-ledger-serviceのビルド手順および
Unit Test 手順について記載・説明する。

###  1.1. <a name='-1'></a>前提条件
- Node（18.16.1）がインストールされていること
- PostgreSQL（12.x）がインストールされていること
- Docker（20.x）がインストールされていること

※Dockerコンテナを使用したパーソナルデータ連携基盤を構築する場合

##  2. <a name='-1'></a>ビルド手順
pxr-ctoken-ledger-serviceのビルド手順について記載する。
※本書では作業ディレクトリをホームディレクトリ配下としているが、任意のディレクトリを作業ディレクトリとすることも可能である(その場合は作業ディレクトリを読み替えて実行すること)。

###  2.1. <a name='-1'></a>サービスをビルドする
事前準備として、作業ディレクトリ配下に「pxr-ctoken-ledger-service」のプロジェクトを配置しておくこと。
以下のコマンドを実行し、エラーが出ないことを確認する。

<table>
<colgroup>
<col style="width: 53%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr class="header">
<th>Linux</th>
<th>Windows（PowerShell）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ npm i</p>
<p>$ npm run build</p></td>
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ npm i</p>
<p>$ npm run build</p></td>
</tr>
</tbody>
</table>

##  3. <a name='UnitTest'></a>Unit Test 手順
pxr-ctoken-ledger-serviceの Unit Test 手順について記載する。

###  3.1. <a name='DB1'></a>DB を作成する（1環境につき初回のみ）
以下を実行する。
（Linux環境はコマンドラインで実行した例を、Windows環境ではpgAdmin4を利用した例を示す）

<table>
<colgroup>
<col style="width: 53%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr class="header">
<th>Linux</th>
<th>Windows</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>$ psql -U postgres</p>
<p>----</p>
<p>postgres=# CREATE DATABASE pxr_pod</p>
<p>WITH</p>
<p>OWNER = postgres</p>
<p>ENCODING = 'UTF8'</p>
<p>LC_COLLATE = 'C'</p>
<p>LC_CTYPE = 'C'</p>
<p>TABLESPACE = pg_default</p>
<p>CONNECTION LIMIT = -1</p>
<p>;</p>
<p>----</p></td>
<td><p>・pgAdmin4を起動する</p>
<p>・左のメニューからServers＞PostgreSQL
12＞データベースの順に開き、データベースを右クリックして作成＞データベースを選択する</p>
<p>・データベースに「pxr_pod」と入力して保存する</p></td>
</tr>
</tbody>
</table>

###  3.2. <a name='SchemaTable1'></a>Schema, Tableを作成する（1環境につき初回のみ）
事前準備として、作業ディレクトリ配下にddlディレクトリを配置しておくこと。
以下を実行する。
（Linux環境はコマンドラインで実行した例を、Windows環境ではpgAdmin4を利用した例を示す）

<table>
<colgroup>
<col style="width: 55%" />
<col style="width: 44%" />
</colgroup>
<thead>
<tr class="header">
<th>Linux</th>
<th>Windows</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>$ cd ~/ddl/db/pxr-ctoken-ledger-service</p>
<p>$ psql -U postgres -d pxr_pod -f createDB.sql</p>
<p>$ psql -U postgres -d pxr_pod -f createTable.sql</p></td>
<td><p>・2.2で作成したpxr_podを右クリックして、クエリツールを選択する</p>
<p>・右側に表示された画面で、ファイルを開くを選択し、ddlリポジトリのdb\pxr-ctoken-ledger-service配下にあるcreateDB.sqlを開く</p>
<p>・実行を選択し、「ログイン/グループロール」にpxr_ctoken_ledger_userが作成されていること、pxr_podのスキーマ配下にpxr_ctoken_ledgerが作成されていることを確認する</p>
<p>・クエリツール画面で、ddlリポジトリのdb\pxr-ctoken-ledger-service配下にあるcreateTable.sqlを開いて、実行する</p></td>
</tr>
</tbody>
</table>

###  3.3. <a name='UnitTest-1'></a>Unit Testを実行する
以下のコマンドを実行し、エラーが出ないことを確認する。

<table>
<colgroup>
<col style="width: 53%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr class="header">
<th>Linux</th>
<th>Windows（PowerShell）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ npm run jest-clear</p>
<p>$ npm run test:unit</p></td>
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$npm run jest-clear</p>
<p>$ npm run test:unit</p></td>
</tr>
</tbody>
</table>

##  4. <a name='pxr-ctoken-ledger-service'></a>pxr-ctoken-ledger-service起動手順
pxr-ctoken-ledger-serviceの起動手順について記載する。

###  4.1. <a name='pxr-ctoken-ledger-service-1'></a>pxr-ctoken-ledger-serviceを起動する
以下のコマンドを実行する。

<table>
<colgroup>
<col style="width: 53%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr class="header">
<th>Linux</th>
<th>Windows（PowerShell）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ npm run start</p></td>
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ npm run start</p></td>
</tr>
</tbody>
</table>

###  4.2. <a name='Web'></a>Webブラウザでアクセスする
以下を実行する。

<table>
<colgroup>
<col style="width: 53%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr class="header">
<th>Linux</th>
<th>Windows</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Webブラウザで以下にアクセスし、Swaggerが表示されること</p>
<p><a
href="http://localhost">http://localhost</a>:3008/api-docs/</p></td>
<td><p>Webブラウザで以下にアクセスし、Swaggerが表示されること</p>
<p><a
href="http://localhost">http://localhost</a>:3008/api-docs/</p></td>
</tr>
</tbody>
</table>

##  5. <a name='Docker'></a>Dockerコンテナイメージ作成手順
Dockerコンテナイメージを作成する手順について記載する。
コンテナを使用したパーソナルデータ連携基盤の構築手順については以下を参照すること。
パーソナルデータ連携基盤_構築ガイド.docx

###  5.1. <a name='Docker-1'></a>Dockerコンテナイメージを作成する
以下のコマンドを実行する。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 49%" />
</colgroup>
<thead>
<tr class="header">
<th>Linux</th>
<th>Windows（PowerShell）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ docker build -t {イメージ名}:{タグ} .</p></td>
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ docker build -t {イメージ名}:{タグ} .</p></td>
</tr>
</tbody>
</table>

###  5.2. <a name='Docker-1'></a>Dockerコンテナイメージをレジストリに登録する
以下のコマンドを実行する。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 49%" />
</colgroup>
<thead>
<tr class="header">
<th>Linux</th>
<th>Windows（PowerShell）</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ docker tag {イメージ名}:{タグ}
{Dockerリポジトリ名}/{イメージ名}:{タグ}</p>
<p>$ docker push {Dockerリポジトリ名}/{イメージ名}:{タグ}</p></td>
<td><p>$ cd ~/pxr-ctoken-ledger-service</p>
<p>$ docker tag {イメージ名}:{タグ}
{Dockerレジストリ名}/{イメージ名}:{タグ}</p>
<p>$ docker push {Dockerレジストリ名}/{イメージ名}:{タグ}</p></td>
</tr>
</tbody>
</table>
