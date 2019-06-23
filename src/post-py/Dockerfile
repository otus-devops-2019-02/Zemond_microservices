FROM python:3.6.0-alpine


WORKDIR /app
ADD . /app


RUN apk update && apk add --no-cache --virtual .build-deps gcc musl-dev \
    && pip install -r /app/requirements.txt \
    && apk del .build-deps  \
    && rm -rf /var/cache/apk/* \
    && rm -f /sbin/apk \
    && rm -rf /etc/apk \
    && rm -rf /lib/apk \
    && rm -rf /usr/share/apk \
    && rm -rf /var/lib/apk \
    && rm -rf ~/.cache/pip



#RUN apk del py-pip
#RUN apk add --no-cache build-base gcc
#RUN pip install -r /app/requirements.txt

ENV POST_DATABASE_HOST post_db
ENV POST_DATABASE posts

ENTRYPOINT ["python3", "post_app.py"]

