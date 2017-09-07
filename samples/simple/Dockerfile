FROM python:3.6.2-alpine
MAINTAINER Ivan Pedrazas <ipedrazas@gmail.com>

RUN apk add --update bash curl && \
    rm -rf /var/cache/apk/*

RUN pip install  \
    flask

COPY . /src
WORKDIR /src

ENV SERVICE_VERSION Version-Tw2

CMD ["python", "./app.py"]
