FROM alpine:3.9

#LABEL maintainer="Andrius Kairiukstis <k@andrius.mobi>"

#ENV NOKOGIRI_USE_SYSTEM_LIBRARIES=1

#ADD gemrc /root/.gemrc

RUN apk update \
&& apk add ruby \
           ruby-bigdecimal \
           ruby-bundler \
           ruby-io-console \
           ruby-irb \
           ca-certificates \
           libressl \
           less \
&& apk add --virtual build-dependencies \
           build-base \
           ruby-dev \
           libressl-dev \
\
&& bundle config build.nokogiri --use-system-libraries \
&& bundle config git.allow_insecure true \
&& gem install json --no-rdoc --no-ri \
&& gem install bson --no-rdoc --no-ri -v '4.3.0' --source 'https://rubygems.org/' \
&& gem install bson_ext  --no-rdoc --no-ri -v '1.5.1' --source 'https://rubygems.org/' \
&& gem install puma --no-rdoc --no-ri -v '3.12.0' --source 'https://rubygems.org/' \
&& gem cleanup \
#&& pip install –upgrade pip \
&& apk del build-dependencies \
&& rm -rf /usr/lib/ruby/gems/*/cache/* \
          /var/cache/apk/* \
          /tmp/* \
          /var/tmp/*
#RUN gem install bson -v '4.3.0' --source 'https://rubygems.org/'

ENV APP_HOME /app
RUN mkdir $APP_HOME
WORKDIR $APP_HOME

ADD Gemfile* $APP_HOME/

#RUN gem install bson -v '4.3.0' --source 'https://rubygems.org/'
RUN bundle install
ADD . $APP_HOME

ENV COMMENT_DATABASE_HOST comment_db
ENV COMMENT_DATABASE comments

CMD ["puma"]
