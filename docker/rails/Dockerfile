FROM ruby:2.7.1-alpine3.12
# FROM ruby:2.6.6-alpine3.12
# FROM ruby:2.5-alpine3.12

# Warning: apt-key output should not be parsed (stdout is not a terminal)とか出る対策
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1

# alpine環境に必要なパッケージをインストール
RUN apk update && \
    apk upgrade && \
    apk add --no-cache \
        gcc \
        g++ \
        libc-dev \
        libxml2-dev \
        linux-headers \
        make \
#       使用するSQLを追加する。
#        postgresql \
#        postgresql-dev\
#        postgresql-client \
#        mysql-client \
#        mysql-dev \
        sqlite-libs \
        sqlite-dev \
        nodejs \
        nodejs-npm \
        tzdata \
        build-base \
        curl-dev \
        graphviz \
#       rails6をインストールしたい場合はyarnを追加する。
        yarn \
        vim

RUN mkdir /myapp
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
COPY . /myapp

COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh

# Bundlerの設定
# 失敗したネットワーク要求を再試行する回数。デフォルトは3
# bundlerが並行してインストールできるgemの数。デフォルトは1
RUN bundle config --global retry 5 \
  && bundle config --global jobs 4
# bundle config --global path vendor/bundle

# 文字コードの設定
# 日本語を受け付けるように設定(rails consoleで日本語を使うなど) => LANG=C.UTF-8
# 設定したロケールの値をC.UTF-8で上書きする => LC_ALL=C.UTF-8
# 文字コードのカテゴリをutf-8に設定 => LC_CTYPE="utf-8"
ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    LC_CTYPE="utf-8"

# 使用するエディタ
ENV EDITOR vim

# タイムゾーン
ENV TZ="Asia/Tokyo"

CMD ["entrypoint.sh"]
EXPOSE 3000
