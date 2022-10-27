FROM ubuntu:latest

ARG QL_MAINTAINER="Timiyu"
LABEL maintainer="${QL_MAINTAINER}"
ARG QL_URL=https://github.com/${QL_MAINTAINER}/qinglong.git
ARG QL_BRANCH=master

ENV DEBIAN_FRONTEND=noninteractive \
    PNPM_HOME=/root/.local/share/pnpm \
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/root/.local/share/pnpm:/root/.local/share/pnpm/global/5/node_modules:$PNPM_HOME \
    LANG=C.UTF-8 \
    SHELL=/bin/bash \
    PS1="\u@\h:\w \$ " \
    QL_DIR=/ql \
    QL_BRANCH=${QL_BRANCH}

WORKDIR ${QL_DIR}

RUN set -x \
    && sed -i 's@http://archive.ubuntu.com/ubuntu/@http://mirrors.aliyun.com/ubuntu/@g' /etc/apt/sources.list \
    && apt-get update -qqy \
    && apt-get install -y python3 \
                          python3-dev \
                          python3-pip \
                          libpcre3 \
                          libpcre3-dev \
                          g++ \
                          libmysqlclient-dev \
                          uwsgi-plugin-python3 \
                          libopenblas-dev \
                          liblapack-dev \
                          libatlas-base-dev  \
                          libblas-dev \
                          gfortran  \
                          libpng-dev \
    && apt-get install -y bash \
                        cron \
                        coreutils \
                        moreutils \
                        git \
                        curl \
                        wget \
                        vim \
                        tzdata \
                        perl \
                        openssl \
                        nginx \
                        jq \
                        openssh-server \
                        openssh-client \
                        libffi-dev \
                        make \
                        automake \
                        libcec-dev \
                        libtool \
    && ln -sf /usr/bin/pip3 /usr/bin/pip \
    && ln -sf /usr/bin/python3 /usr/bin/python \
    && pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/ \
    && pip config set install.trusted-host mirrors.aliyun.com \
    && curl -fsSL https://deb.nodesource.com/setup_16.x | bash - \
    && apt-get install -y nodejs \
    && apt-get clean -y \
    && apt-get autoclean -y \
    && apt-get update -y \
    && apt-get autoremove -y\
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && git config --global user.email "qinglong@@users.noreply.github.com" \
    && git config --global user.name "qinglong" \
    && git config --global http.postBuffer 524288000 \
    && npm install -g pnpm \
    && pnpm add -g pm2 ts-node typescript tslib \
    && git clone -b ${QL_BRANCH} ${QL_URL} ${QL_DIR} \
    && cd ${QL_DIR} \
    && cp -f .env.example .env \
    && chmod 777 ${QL_DIR}/shell/*.sh \
    && chmod 777 ${QL_DIR}/docker/*.sh \
    && pnpm install --prod \
    && rm -rf /root/.pnpm-store \
    && rm -rf /root/.local/share/pnpm/store \
    && rm -rf /root/.cache \
    && rm -rf /root/.npm \
    && git clone -b ${QL_BRANCH} https://github.com/${QL_MAINTAINER}/qinglong-static.git /static \
    && mkdir -p ${QL_DIR}/static \
    && cp -rf /static/* ${QL_DIR}/static \
    && rm -rf /static
    
ENTRYPOINT ["./docker/docker-entrypoint.sh"]
