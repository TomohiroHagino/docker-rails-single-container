# Docker練習用タイプ

# サービスに関して

- 名称：プロジェクト名


# RailsのGitリポジトリをクローンして、すぐにDocker化しやすくするファイル群

```
git cloneしてきて、３コマンドくらい打って、docker-compose upとすれば起動できます！
```


# 使用方法

```
0.このリポジトリをクローンして.gitファイルを削除する。
$ rm -rf .git

1. docker desktopをインストールしたあと、Railsのリポジトリをクローンする。
$ git clone <クローンしたいRailsリポジトリ>
$ mv <クローンしたいRailsリポジトリ名>/* .

2. そして、/docker/railsディレクトリに入っているDockerfileの1行目にかかれたRubyのバージョンをお好みのものに変更する。

3. そして、/docker/railsディレクトリに入っているGemfileをクローンしたプロジェクトのGemfileで上書きする。Gemfile.lockは空っぽにする。
$ cp Gemfile ./docker/rails/
$ cp Gemfile.lock ./docker/rails/

(Railsのバージョンは5、開発時はsqlite3、本番時はpostgresqlを使用するのを想定してます。)
(Rails6をインストールしたい場合はDockerfileとentrypoint.shのyarnのコメントアウトを外すこと)

4. ここから環境構築。　初回時はこれ（imageがなければimageビルドから）コンテナの起動までを行う。
$ docker-compose up -d

railsコンテナのターミナルに直接アクセス
コンテナから抜ける時はCommand+P,Q
$ docker-compose exec rails ash

5. サーバーの立ち上げ
rails s -b 0.0.0.0

6. DB関連 コンテナに直接アクセスして以下のコマンドをやればOK
#(コンテナ内で) rails db:reset
#(コンテナ内で) rails db:create
#(コンテナ内で) rails db:migrate
#(コンテナ内で) rails s -b 0.0.0.0

作業を終了する時はコンテナを終了させます。
$ docker-compose stop

2回目以降からは以前に作ったコンテナを起動させます。
$ docker-compose start

```

