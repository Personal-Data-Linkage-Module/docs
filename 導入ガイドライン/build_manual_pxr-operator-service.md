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
- Node（18.16.10）がインストールされていること
- PostgreSQL（12.x）がインストールされていること
- Docker（20.x）がインストールされていること

※Dockerコンテナを使用したパーソナルデータ連携基盤を構築する場合

##  2. <a name='-1'></a>ビルド手順
pxr-operator-serviceのビルド手順について記載する。
※本書では作業ディレクトリ（ [pxr-operator-service](https://github.com/Personal-Data-Linkage-Module/pxr-operator-service) ）をホームディレクトリ配下に配置した場合の例を記載する。

###  2.1. <a name='-1'></a>サービスをビルドする
Linux 環境と Windows 環境でコマンドラインを利用する例を示す。
以下のコマンドを実行し、エラーが出ないことを確認する。

#### Linux環境のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ npm install --save-dev --legacy-peer-deps
$ npm run build
```

#### Windows環境（PowerShell）のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ npm install --save-dev --legacy-peer-deps
$ npm run build
```

##  3. <a name='UnitTest'></a>Unit Test 手順
pxr-operator-service の Unit Test 手順について記載する。

###  3.1. <a name='DB1'></a>DB を作成する（1環境につき初回のみ）
Linux 環境でコマンドラインを利用する例と Windows 環境で pgAdmin4 を利用する例を示す。

#### Linux環境のコマンド実行手順
```
$ psql -U postgres
postgres=# CREATE DATABASE pxr_pod WITH
postgres-# OWNER = postgres
postgres-# ENCODING = 'UTF8'
postgres-# LC_COLLATE = 'C'
postgres-# LC_CTYPE = 'C'
postgres-# TABLESPACE = pg_default
postgres-# CONNECTION LIMIT = -1
postgres-# TEMPLATE = template0
postgres-# ;
```

#### Windows環境のpgAdmin4操作手順
1. pgAdmin4を起動する
1. 左のメニューからServers＞PostgreSQL12＞データベースの順に開き、データベースを右クリックして作成＞データベースを選択する
1. データベースに「pxr_pod」と入力して保存する

###  3.2. <a name='SchemaTable1'></a>Schema, Tableを作成する（1環境につき初回のみ）
Linux 環境でコマンドラインを利用する例と Windows 環境で pgAdmin4 を利用する例を示す。

#### Linux環境のコマンド実行手順
```
$ cd ~/pxr-operator-service/ddl
$ psql -U postgres -d pxr_pod -f createDB.sql
$ psql -U pxr_operator_user -d pxr_pod -f createTable.sql
$ psql -U pxr_operator_user -d pxr_pod -f operator_5170_alter.sql
```

#### Windows環境のpgAdmin4操作手順
1. 2.2で作成したpxr_podを右クリックして、クエリツールを選択する
1. 右側に表示された画面で、ファイルを開くを選択し、~\pxr-operator-service\ddl 配下にある createDB.sql を開く
1. 実行を選択し、「ログイン/グループロール」に pxr_operator_user が作成されていること、pxr_pod のスキーマ配下に pxr_operator が作成されていることを確認する
1. クエリツール画面で、 ~\pxr-operator-service\ddl 配下にあるcreateTable.sqlを開いて実行する

###  3.3. <a name='UnitTest-1'></a>Unit Testを実行する
以下のコマンドを実行し、エラーが出ないことを確認する。

#### Linux環境のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ npm run jest-clear
$ npm run test:unit
```

#### Windows環境（PowerShell）のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ npm run jest-clear
$ npm run test:unit
```

##  4. <a name='pxr-operator-service'></a>pxr-operator-service起動手順
pxr-operator-serviceの起動手順について記載する。

###  4.1. <a name='pxr-operator-service-1'></a>pxr-operator-serviceを起動する
Linux 環境と Windows 環境でコマンドラインを利用する例を示す。
以下のコマンドを実行し、エラーが出ないことを確認する。

#### Linux環境のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ npm run start
```

#### Windows環境（PowerShell）のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ npm run start
```

###  4.2. <a name='Web'></a>Webブラウザでアクセスする
Webブラウザで http://localhost:3000/api-docs/ へアクセスし、Swaggerが表示されることを確認する。


##  5. <a name='Docker'></a>Dockerコンテナイメージ作成手順
Dockerコンテナイメージを作成する手順について記載する。
コンテナを使用したパーソナルデータ連携基盤の構築手順については以下を参照すること。
パーソナルデータ連携基盤_構築ガイド.docx

###  5.1. <a name='Docker-1'></a>Dockerコンテナイメージを作成する
Linux 環境と Windows 環境でコマンドラインを利用する例を示す。

#### Linux環境のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ docker build -t {イメージ名}:{タグ} .
```

#### Windows環境（PowerShell）のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ docker build -t {イメージ名}:{タグ} .
```

###  5.2. <a name='Docker-1'></a>Dockerコンテナイメージをレジストリに登録する
Linux 環境と Windows 環境でコマンドラインを利用する例を示す。

#### Linux環境のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ docker tag {イメージ名}:{タグ} {Dockerリポジトリ名}/{イメージ名}:{タグ}
$ docker push {Dockerリポジトリ名}/{イメージ名}:{タグ}
```

#### Windows環境（PowerShell）のコマンド実行手順
```
$ cd ~/pxr-operator-service
$ docker tag {イメージ名}:{タグ} {Dockerリポジトリ名}/{イメージ名}:{タグ}
$ docker push {Dockerリポジトリ名}/{イメージ名}:{タグ}
```