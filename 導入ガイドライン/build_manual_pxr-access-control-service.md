# パーソナルデータ連携モジュール pxr-access-control-service ビルド手順書

# 目次
<!-- vscode-markdown-toc -->
* 1. [はじめに](#)
	* 1.1. [前提条件](#-1)
	* 1.2. [証明書](#-1)
* 2. [ビルド手順](#-1)
	* 2.1. [サービスをビルドする](#-1)
* 3. [Unit Test 手順](#UnitTest)
	* 3.1. [DB を作成する（1環境につき初回のみ）](#DB1)
	* 3.2. [Schema, Tableを作成する（1環境につき初回のみ）](#SchemaTable1)
	* 3.3. [Unit Testを実行する](#UnitTest-1)
* 4. [pxr-access-control-service起動手順](#pxr-access-control-service)
	* 4.1. [pxr-access-control-serviceを起動する](#pxr-access-control-service-1)
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
本書は、パーソナルデータ連携基盤の一部である、pxr-access-control-serviceのビルド手順および
Unit Test 手順について記載・説明する。

###  1.1. <a name='-1'></a>前提条件

- Node（18.16.1）がインストールされていること
- PostgreSQL（12.x）がインストールされていること
- Docker（20.x）がインストールされていること

※Dockerコンテナを使用したパーソナルデータ連携基盤を構築する場合

###  1.2. <a name='-1'></a>証明書
access-control-serviceとaccess-control-manage-serviceとで共通のクライアント証明書を使用する。

1.  RSA秘密鍵とクライアント証明書をclient.key,
    client-ca.crtというファイル名でソースリポジトリのcertフォルダに格納する。

2.  設定ファイルconfig.jsonのcert.client_crt,
    cert.client_keyにパスを記載する。

##  2. <a name='-1'></a>ビルド手順
pxr-access-control-serviceのビルド手順について記載する。
※本書では作業ディレクトリをホームディレクトリ配下としているが、任意のディレクトリを作業ディレクトリとすることも可能である(その場合は作業ディレクトリを読み替えて実行すること)。

###  2.1. <a name='-1'></a>サービスをビルドする
事前準備として、作業ディレクトリ配下に「pxr-access-control-service」のプロジェクトを配置しておくこと。
以下のコマンドを実行し、エラーが出ないことを確認する。

| Linux | Windows（PowerShell） |
| --- | --- |
| `$ cd ~/pxr-access-control-service` `$ npm i` `$ npm run build` | `$ cd ~/pxr-access-control-service` `$ npm i` `$ npm run build` |

##  3. <a name='UnitTest'></a>Unit Test 手順
pxr-access-control-serviceの Unit Test 手順について記載する。

###  3.1. <a name='DB1'></a>DB を作成する（1環境につき初回のみ）
以下を実行する。
（Linux環境はコマンドラインで実行した例を、Windows環境ではpgAdmin4を利用した例を示す）

| Linux | Windows |
| --- | --- |
| `$ psql -U postgres`<br/>`postgres=# CREATE DATABASE pxr_pod`<br/>`WITH`<br/>`OWNER = postgres`<br/>`ENCODING = 'UTF8'`<br/>`LC_COLLATE = 'C'`<br/>`LC_CTYPE = 'C'`<br/>`TABLESPACE = pg_default`<br/>`CONNECTION LIMIT = -1` | ・pgAdmin4を起動する<br/>・左のメニューからServers＞PostgreSQL 12＞データベースの順に開き、データベースを右クリックして作成＞データベースを選択する<br/>・データベースに「pxr_pod」と入力して保存する |

###  3.2. <a name='SchemaTable1'></a>Schema, Tableを作成する（1環境につき初回のみ）
事前準備として、作業ディレクトリ配下にddlディレクトリを配置しておくこと。
以下を実行する。
（Linux環境はコマンドラインで実行した例を、Windows環境ではpgAdmin4を利用した例を示す）

| Linux | Windows |
| --- | --- |
| `$ cd ~/ddl/db/pxr-access-control-service` `$ psql -U postgres -d pxr_pod -f createDB.sql` `$ psql -U postgres -d pxr_pod -f createTable.sql` | ・2.2で作成したpxr_podを右クリックして、クエリツールを選択する<br/>・右側に表示された画面で、ファイルを開くを選択し、ddlリポジトリの`db\pxr-access-control-service`配下にある`createDB.sql`を開く<br/>・実行を選択し、「ログイン/グループロール」に`pxr_access_control_user`が作成されていること、pxr_podのスキーマ配下に`pxr_access_control`が作成されていることを確認する<br/>・クエリツール画面で、ddlリポジトリの`db\pxr-access-control-service`配下にある`createTable.sql`を開いて、実行する |

###  3.3. <a name='UnitTest-1'></a>Unit Testを実行する
以下のコマンドを実行し、エラーが出ないことを確認する。

| Linux | Windows（PowerShell） |
| --- | --- |
| `$ cd ~/pxr-access-control-service` `$ npm run jest-clear` `$ npm run test:unit` | `$ cd ~/pxr-access-control-service` `$ npm run jest-clear` `$ npm run test:unit` |

##  4. <a name='pxr-access-control-service'></a>pxr-access-control-service起動手順
pxr-access-control-serviceの起動手順について記載する。

###  4.1. <a name='pxr-access-control-service-1'></a>pxr-access-control-serviceを起動する
以下のコマンドを実行する。

| Linux | Windows（PowerShell） |
| --- | --- |
| `$ cd ~/pxr-access-control-service` `$ npm run start` | `$ cd ~/pxr-access-control-service` `$ npm run start` |

###  4.2. <a name='Web'></a>Webブラウザでアクセスする
以下を実行する。

| Linux | Windows |
| --- | --- |
| Webブラウザで以下にアクセスし、Swaggerが表示されること<br/>`http://localhost:3015/api-docs/` | Webブラウザで以下にアクセスし、Swaggerが表示されること<br/>`http://localhost:3015/api-docs/` |

##  5. <a name='Docker'></a>Dockerコンテナイメージ作成手順
Dockerコンテナイメージを作成する手順について記載する。
コンテナを使用したパーソナルデータ連携基盤の構築手順については以下を参照すること。
パーソナルデータ連携基盤_構築ガイド.docx

###  5.1. <a name='Docker-1'></a>Dockerコンテナイメージを作成する
以下のコマンドを実行する。

| Linux | Windows（PowerShell） |
| --- | --- |
| `$ cd ~/pxr-access-control-service` `$ docker build -t {イメージ名}:{タグ} .` | `$ cd ~/pxr-access-control-service` `$ docker build -t {イメージ名}:{タグ} .` |

###  5.2. <a name='Docker-1'></a>Dockerコンテナイメージをレジストリに登録する
以下のコマンドを実行する。

| Linux | Windows（PowerShell） |
| --- | --- |
| `$ cd ~/pxr-access-control-service` `$ docker tag {イメージ名}:{タグ} {Dockerリポジトリ名}/{イメージ名}:{タグ}` `$ docker push {Dockerリポジトリ名}/{イメージ名}:{タグ}` | `$ cd ~/pxr-access-control-service` `$ docker tag {イメージ名}:{タグ} {Dockerレジストリ名}/{イメージ名}:{タグ}` `$ docker push {Dockerレジストリ名}/{イメージ名}:{タグ}` |
