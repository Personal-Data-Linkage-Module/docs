# パーソナルデータ連携モジュール pxr-catalog-update-service ビルド手順書

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
* 4. [pxr-catalog-update-service起動手順](#pxr-catalog-update-service)
	* 4.1. [pxr-catalog-update-serviceを起動する](#pxr-catalog-update-service-1)
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
本書は、パーソナルデータ連携基盤の一部である、pxr-catalog-update-serviceのビルド手順および
Unit Test 手順について記載・説明する。

###  1.1. <a name='-1'></a>前提条件
- Node（18.16.1）がインストールされていること
- PostgreSQL（12.x）がインストールされていること
- Docker（20.x）がインストールされていること
※Dockerコンテナを使用したパーソナルデータ連携基盤を構築する場合

##  2. <a name='-1'></a>ビルド手順
pxr-catalog-update-serviceのビルド手順について記載する。
※本書では作業ディレクトリをホームディレクトリ配下としているが、任意のディレクトリを作業ディレクトリとすることも可能である(その場合は作業ディレクトリを読み替えて実行すること)。

###  2.1. <a name='-1'></a>サービスをビルドする
事前準備として、作業ディレクトリ配下に「pxr-catalog-update-service」のプロジェクトを配置しておくこと。
以下のコマンドを実行し、エラーが出ないことを確認する。

| Linux | Windows (PowerShell) |
|-------|---------------------|
| `$ cd ~/pxr-catalog-update-service`<br/>`$ npm i`<br/>`$ npm run build` | `$ cd ~/pxr-catalog-update-service`<br/>`$ npm i`<br/>`$ npm run build` |

##  3. <a name='UnitTest'></a>Unit Test 手順
pxr-catalog-update-serviceの Unit Test 手順について記載する。

###  3.1. <a name='DB1'></a>DB を作成する（1環境につき初回のみ）
以下を実行する。
（Linux環境はコマンドラインで実行した例を、Windows環境ではpgAdmin4を利用した例を示す）

| Linux | Windows |
|-------|---------|
| `$ psql -U postgres`<br/>----<br/>`postgres=# CREATE DATABASE pxr_pod`<br/>`WITH`<br/>`OWNER = postgres`<br/>`ENCODING = 'UTF8'`<br/>`LC_COLLATE = 'C'`<br/>`LC_CTYPE = 'C'`<br/>`TABLESPACE = pg_default`<br/>`CONNECTION LIMIT = -1`<br/>`;`<br/>---- | pgAdmin4を起動する<br/>左のメニューからServers＞PostgreSQL 12＞データベースの順に開き、データベースを右クリックして作成＞データベースを選択する<br/>データベースに「pxr_pod」と入力して保存する |

###  3.2. <a name='SchemaTable1'></a>Schema, Tableを作成する（1環境につき初回のみ）
事前準備として、作業ディレクトリ配下にddlディレクトリを配置しておくこと。
以下を実行する。
（Linux環境はコマンドラインで実行した例を、Windows環境ではpgAdmin4を利用した例を示す）

| Linux | Windows |
|-------|---------|
| `$ cd ~/ddl/db/pxr-catalog-update-service`<br/>`$ psql -U postgres -d pxr_pod -f createDB.sql`<br/>`$ psql -U postgres -d pxr_pod -f createTable.sql` | 2.2で作成したpxr_podを右クリックして、クエリツールを選択する<br/>右側に表示された画面で、ファイルを開くを選択し、ddlリポジトリのdb\pxr-catalog-update-service配下にあるcreateDB.sqlを開く<br/>実行を選択し、「ログイン/グループロール」にpxr_catalog_update_userが作成されていること、pxr_podのスキーマ配下にpxr_catalog_updateが作成されていることを確認する<br/>クエリツール画面で、ddlリポジトリのdb\pxr-catalog-update-service配下にあるcreateTable.sqlを開いて、実行する |

###  3.3. <a name='UnitTest-1'></a>Unit Testを実行する
以下のコマンドを実行し、エラーが出ないことを確認する。

| Linux | Windows (PowerShell) |
|-------|---------------------|
| `$ cd ~/pxr-catalog-update-service`<br/>`$ npm run jest-clear`<br/>`$ npm run test:unit` | `$ cd ~/pxr-catalog-update-service`<br/>`$ npm run jest-clear`<br/>`$ npm run test:unit` |

##  4. <a name='pxr-catalog-update-service'></a>pxr-catalog-update-service起動手順
pxr-catalog-update-serviceの起動手順について記載する。

###  4.1. <a name='pxr-catalog-update-service-1'></a>pxr-catalog-update-serviceを起動する
以下のコマンドを実行する。

| Linux | Windows (PowerShell) |
|-------|---------------------|
| `$ cd ~/pxr-catalog-update-service`<br/>`$ npm run start` | `$ cd ~/pxr-catalog-update-service`<br/>`$ npm run start` |

###  4.2. <a name='Web'></a>Webブラウザでアクセスする
以下を実行する。

| Linux | Windows |
|-------|---------|
| Webブラウザで以下にアクセスし、Swaggerが表示されること<br/>`http://localhost:3002/api-docs/` | Webブラウザで以下にアクセスし、Swaggerが表示されること<br/>`http://localhost:3002/api-docs/` |

##  5. <a name='Docker'></a>Dockerコンテナイメージ作成手順
Dockerコンテナイメージを作成する手順について記載する。
コンテナを使用したパーソナルデータ連携基盤の構築手順については以下を参照すること。
パーソナルデータ連携基盤_構築ガイド.docx

###  5.1. <a name='Docker-1'></a>Dockerコンテナイメージを作成する
以下のコマンドを実行する。

| Linux | Windows (PowerShell) |
|-------|---------------------|
| `$ cd ~/pxr-catalog-update-service`<br/>`$ docker build -t {イメージ名}:{タグ} .` | `$ cd ~/pxr-catalog-update-service`<br/>`$ docker build -t {イメージ名}:{タグ} .` |

###  5.2. <a name='Docker-1'></a>Dockerコンテナイメージをレジストリに登録する
以下のコマンドを実行する。

| Linux | Windows (PowerShell) |
|-------|---------------------|
| `$ cd ~/pxr-catalog-update-service`<br/>`$ docker tag {イメージ名}:{タグ} {Dockerリポジトリ名}/{イメージ名}:{タグ}`<br/>`$ docker push {Dockerリポジトリ名}/{イメージ名}:{タグ}` | `$ cd ~/pxr-catalog-update-service`<br/>`$ docker tag {イメージ名}:{タグ} {Dockerレジストリ名}/{イメージ名}:{タグ}`<br/>`$ docker push {Dockerレジストリ名}/{イメージ名}:{タグ}` |
