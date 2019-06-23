FROM alpine:3.9

RUN apk update \
&& apk add ruby \
           ruby-bigdecimal \
           ruby-bundler \
           ruby-io-console \
           ruby-irb \
           ca-certificates \
           libressl \
           less \
           ruby-json\
&& apk add --virtual build-dependencies \
           build-base \
           ruby-dev \
           libressl-dev \
\
&& bundle config build.nokogiri --use-system-libraries \
#&& python -m pip install –upgrade pip \
&& bundle config git.allow_insecure true \
&& gem install json --no-rdoc --no-ri \
&& gem install bson --no-rdoc --no-ri -v '4.3.0' --source 'https://rubygems.org/' \
#&& gem install bson_ext  --no-rdoc --no-ri -v '1.5.1' --source 'https://rubygems.org/' \
&& gem install bson_ext  --no-rdoc --no-ri -v '1.12.5' --source 'https://rubygems.org/' \
&& gem install thrift  --no-rdoc --no-ri -v '0.9.3.0' --source 'https://rubygems.org/' \
&& gem install puma --no-rdoc --no-ri -v '3.12.0' --source 'https://rubygems.org/' \
&& gem install bundler --no-ri --no-rdoc \
&& gem cleanup \
&& apk del build-dependencies \
&& rm -rf /usr/lib/ruby/gems/*/cache/* \
          /var/cache/apk/* \
          /tmp/* \
          /var/tmp/*

ENV APP_HOME /app
RUN mkdir $APP_HOME

WORKDIR $APP_HOME
ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME

ENV POST_SERVICE_HOST post
ENV POST_SERVICE_PORT 5000
ENV COMMENT_SERVICE_HOST comment
ENV COMMENT_SERVICE_PORT 9292

CMD ["puma"]
