FROM ruby:2.6.3
ENV LANG C.UTF-8

# yarnパッケージ管理ツールインストール
RUN apt-get update && apt-get install -y curl apt-transport-https wget && \
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
apt-get update && apt-get install -y yarn

# Node.jsをインストール
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && \
apt-get install nodejs

RUN apt-get update -qq && \
 apt-get install -y --no-install-recommends \
 vim \
 build-essential \
 mysql-client \
 libfontconfig1 && \
 rm -rf /var/lib/apt/lists/*

ENV ENTRYKIT_VERSION 0.4.0

RUN wget https://github.com/progrium/entrykit/releases/download/v${ENTRYKIT_VERSION}/entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
&& tar -xvzf entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
&& rm entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
&& mv entrykit /bin/entrykit \
&& chmod +x /bin/entrykit \
&& entrykit --symlink

RUN mkdir /app

WORKDIR /app

RUN bundle config build.nokogiri --use-system-libraries

ENTRYPOINT [ \
  "prehook", "ruby -v", "--", \
  "prehook", "bundle install -j3 --quiet", "--"]