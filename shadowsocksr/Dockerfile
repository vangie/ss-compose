FROM alpine
MAINTAINER Vangie Du <duwan@live.com>

RUN set -ex \
    && if [ $(wget -qO- ipinfo.io/country) == CN ]; then echo "http://mirrors.aliyun.com/alpine/latest-stable/main/" > /etc/apk/repositories ;fi \
    && apk add --no-cache libsodium py-pip curl \
    && pip install cymysql

RUN mkdir -p /opt; cd /opt  \
    && curl -sS https://codeload.github.com/shadowsocksr/shadowsocksr/tar.gz/manyuser | tar zxvf - \
    && cd /opt/shadowsocksr-manyuser \
    && chmod +x *.sh && chmod +x shadowsocks/*.sh && chmod +x server.py

ENTRYPOINT ["/opt/shadowsocksr-manyuser/server.py"]
