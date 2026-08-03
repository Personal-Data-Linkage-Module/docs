# パーソナルデータ連携モジュール pxr-operator-service ビルド手順書

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
* 4. [pxr-operator-service起動手順](#pxr-operator-service)
	* 4.1. [pxr-operator-serviceを起動する](#pxr-operator-service-1)
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
本書は、パーソナルデータ連携基盤の一部である、pxr-operator-serviceのビルド手順および
Unit Test 手順について記載・説明する。

###  1.1. <a name='-1'></a>前提条件
- Node（18.16.1）がインストールされていること
- PostgreSQL（12.x）がインストールされていること
- Docker（20.x）がインストールされていること

※Dockerコンテナを使用したパーソナルデータ連携基盤を構築する場合

##  2. <a name='-1'></a>ビルド手順
pxr-operator-serviceのビルド手順について記載する。
※本書では作業ディレクトリをホームディレクトリ配下としているが、任意のディレクトリを作業ディレクトリとすることも可能である(その場合は作業ディレクトリを読み替えて実行すること)。

###  2.1. <a name='-1'></a>サービスをビルドする
事前準備として、作業ディレクトリ配下に「pxr-operator-service」のプロジェクトを配置しておくこと。
以下のコマンドを実行し、エラーが出ないことを確認する。

| Linux | Windows（PowerShell） |
|---|---|
| `cd ~/pxr-operator-service`<br/>`npm i`<br/>`npm run build` | `cd ~/pxr-operator-service`<br/>`npm i`<br/>`npm run build` |

##  3. <a name='UnitTest'></a>Unit Test 手順
pxr-operator-serviceの Unit Test 手順について記載する。

###  3.1. <a name='DB1'></a>DB を作成する（1環境につき初回のみ）
以下を実行する。
（Linux環境はコマンドラインで実行した例を、Windows環境ではpgAdmin4を利用した例を示す）

| Linux | Windows |
|---|---|
| `$ psql -U postgres`<br/>`postgres=# CREATE DATABASE pxr_pod WITH OWNER = postgres ENCODING = 'UTF8' LC_COLLATE = 'C' LC_CTYPE = 'C' TABLESPACE = pg_default CONNECTION LIMIT = -1 ;` | ・`pgAdmin4`を起動する<br/>・左のメニューから`Servers＞PostgreSQL 12＞データベース`の順に開き、データベースを右クリックして作成＞データベースを選択する<br/>・データベースに「`pxr_pod`」と入力して保存する |

###  3.2. <a name='SchemaTable1'></a>Schema, Tableを作成する（1環境につき初回のみ）
事前準備として、作業ディレクトリ配下にddlディレクトリを配置しておくこと。
以下を実行する。
（Linux環境はコマンドラインで実行した例を、Windows環境ではpgAdmin4を利用した例を示す）

| Linux | Windows |
|---|---|
| `cd ~/ddl/db/pxr-operator-service`<br/>`psql -U postgres -d pxr_pod -f createDB.sql`<br/>`psql -U postgres -d pxr_pod -f createTable.sql` | ・2.2で作成した`pxr_pod`を右クリックして、クエリツールを選択する<br/>・右側に表示された画面で、ファイルを開くを選択し、ddlリポジトリの`db\pxr-operator-service`配下にある`createDB.sql`を開く<br/>・実行を選択し、「ログイン/グループロール」に`pxr_operator_user`が作成されていること、`pxr_pod`のスキーマ配下に`pxr_operator`が作成されていることを確認する<br/>・クエリツール画面で、ddlリポジトリの`db\pxr-operator-service`配下にある`createTable.sql`を開いて、実行する |

###  3.3. <a name='UnitTest-1'></a>Unit Testを実行する
以下のコマンドを実行し、エラーが出ないことを確認する。

| Linux | Windows（PowerShell） |
|---|---|
| `cd ~/pxr-operator-service`<br/>`npm run jest-clear`<br/>`npm run test:unit` | `cd ~/pxr-operator-service`<br/>`npm run jest-clear`<br/>`npm run test:unit` |

##  4. <a name='pxr-operator-service'></a>pxr-operator-service起動手順
pxr-operator-serviceの起動手順について記載する。

###  4.1. <a name='pxr-operator-service-1'></a>pxr-operator-serviceを起動する
以下のコマンドを実行する。

| Linux | Windows（PowerShell） |
|---|---|
| `cd ~/pxr-operator-service`<br/>`npm run start` | `cd ~/pxr-operator-service`<br/>`npm run start` |

###  4.2. <a name='Web'></a>Webブラウザでアクセスする
以下を実行する。

| Linux | Windows |
|---|---|
| Webブラウザで以下にアクセスし、Swaggerが表示されること<br/>`http://localhost:3000/api-docs/` | Webブラウザで以下にアクセスし、Swaggerが表示されること<br/>`http://localhost:3000/api-docs/` |

##  5. <a name='Docker'></a>Dockerコンテナイメージ作成手順
Dockerコンテナイメージを作成する手順について記載する。
コンテナを使用したパーソナルデータ連携基盤の構築手順については以下を参照すること。
パーソナルデータ連携基盤_構築ガイド.docx

###  5.1. <a name='Docker-1'></a>Dockerコンテナイメージを作成する
以下のコマンドを実行する。

| Linux | Windows（PowerShell） |
|---|---|
| `cd ~/pxr-operator-service`<br/>`docker build -t {イメージ名}:{タグ} .` | `cd ~/pxr-operator-service`<br/>`docker build -t {イメージ名}:{タグ} .` |

###  5.2. <a name='Docker-1'></a>Dockerコンテナイメージをレジストリに登録する
以下のコマンドを実行する。

| Linux | Windows（PowerShell） |
|---|---|
| `cd ~/pxr-operator-service`<br/>`docker tag {イメージ名}:{タグ} {Dockerリポジトリ名}/{イメージ名}:{タグ}`<br/>`docker push {Dockerリポジトリ名}/{イメージ名}:{タグ}` | `cd ~/pxr-operator-service`<br/>`docker tag {イメージ名}:{タグ} {Dockerレジストリ名}/{イメージ名}:{タグ}`<br/>`docker push {Dockerレジストリ名}/{イメージ名}:{タグ}` |
