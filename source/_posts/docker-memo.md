---
title: Docker メモ
date: 2019-11-05 18:31:19
tags: ['docker', 'container', 'development']
---

## Dockerfile

### インストラクション

#### FROM
　FROMは作成するDockerイメージのベースとなるイメージを指定でいる。
　FROMで指定できるDockerイメージはDocker Hubに公開されているものから指定できる。

#### LABEL
　イメージ作者の情報などを記述。

### ENV
　Dockerコンテナ内で使える環境変数を設定できる。

### ARG
　ビルド時に指定する引数から値を受け取ることができる。
　Dockerfile内で指定したARGの引数と、ビルド時に指定する引数はそろえる必要がある。

```
#コマンド：sampleargがそろえるべき名前
> docker image build --build-arg samplearg=okok -t example/dockerimage .

#Dockerfileでは以下のように名前を指定して二行目のように値を取り出すことができる。
ARG samplearg
ENV BUILD_ARG=${samplearg}
```



#### RUN
　RUNはDockerイメージビルド時に、Dockerコンテナ内で実行するコマンドを定義する。
　RUNの引数には実行したいコマンドをそのまま記述する。

#### COPY
　COPYはDockerを動作させているホストマシン上のファイルやディレクトリをDockerコンテナ内にコピーするためのインストラクション。

#### CMD
　CMDはDockerコンテナとして実行する際に、コンテナ内で実行するプロセスを指定する。
　RUNはイメージをビルドするために使い、CMDはコンテナ起動時に一度実行される。

#### ENTRYPOINT
　ENTRYPOINTにはコマンドを記述する。CMDはENTRYPOINTへの引数となる。--entrypointでrun時に指定することで上書きできる。

## ビルド

```
docker image build -t イメージ[:タグ名] Dockerfileまでのパス
```

タグ名は省略するとlatestとなる。

## イメージからコンテナの実行

```
docker [container] run [-d] [-i] [-t] [-rm] [-v ホスト側ディレクトリ:コンテナ側ディレクトリ] イメージ名[/追加のパス]:タグ名
```

`-d`オプションをつけないとフォアグランドで実行されてしまう。-dをつけて起動した場合はコンテナIDが標準出力に出力される。
containerは省略可能だが、最近の風潮ではタイプが長くなるがつけて記述するほうが好まれるらしい。

`-i`起動後にコンテナの標準入力との結びつきを維持する。

`-t`疑似端末を有効にする。大抵の場合`-i`と同時に用いられ、同時指定の`-it`が省略形として存在する。コンテナ起動後に、疑似端末が有効となり標準入力からコマンドを実行できるようになる。

`-rm`コンテナの終了時にコンテナを破棄する。

`-v`ホストとコンテナ間でディレクトリやファイルの共有を行う。

### そのほかオプション

`-f` Dockerfileのファイル名を指定できる。省略時は`Dockerfile`

`--pull=true` ビルド時に`FROM`で指定したイメージをレジストリからダウンロードを必ず行う。省略時は、すでにダウンロード済みであれば、ホスト内のキャッシュからビルドを行い、存在しない場合はレジストリからダウンロードする。

## 起動中のコンテナの確認

```
docker container ls
docker ps
```

上記二つはどちらも同じ動作。
表示項目のフィルタリングもできる。コンテナIDだけ抽出する場合は以下。

```
docker container ls --filter "ancestor=[イメージ名]" [-q]
```
-q でコンテナIDのみの抽出

## 起動中のコンテナの停止

```
docker container stop コンテナID/コンテナ名
```

`docker container ls`と組み合わせることができる。

```
docker container stop $(docker container ls --filter "ancestor=[イメージ名" -q])
```

## 停止中のコンテナの再起動

```
docker container restart コンテナID/コンテナ名
```

## 停止中のコンテナの破棄

```
docker container rm [-f] コンテナID/コンテナ名
```

`-f`オプションで実行中のコンテナの削除も可能。

## dockerイメージの検索

```
docker search [options] 検索キーワード
```

`-f, --filter filter` 出力をフィルターする。
`--format 文字列` 出力をフォーマットする。
`--limit 数値` 表示件数を指定の数値に制限する。
`--no-trunc` 出力を省略せずに表示する。

## dockerイメージの取得

```
docker image pull [options] リポジトリ名[:タグ名]
```

例によってタグ名を省略した場合は`latest`が指定される。

## ホスト内のイメージの一覧を取得

```
docker image ls [options] [リポジトリ[:タグ名]]
```

ダウンロード済み、ビルド済みのイメージの一覧を表示する。

## dockerイメージのタグ付け

```
docker image tag 元のイメージ名[:タグ名] 新しいイメージ名[:タグ名]
```

タグ名を省略時は`latest`

## docker Hub へログイン

```
docker login [-u id] [-p パスワード]
```

オプションを省略した場合は対話式に進めることができる。ID&PASSを入力して認証する。

## docker hub イメージの登録

```
docker image push [options] リポジトリ名[:タグ名]
```

ログイン済みであれば、自分のリポジトリにイメージを登録することができる。

## ポートフォワーディング

ポートフォワーディングは`docker container run`の起動コマンド時に`-p`オプションで指定できる。

```
docker container run -d -p [ホストポート]:コンテナポート イメージの指定
```
