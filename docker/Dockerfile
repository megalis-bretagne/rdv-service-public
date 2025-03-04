FROM ruby:3.3.7-bookworm

WORKDIR /app
COPY . .

ARG NODEJS=v22.14.0

RUN apt update && \
    apt install -y graphviz && \
    curl -fsSLO --compressed https://nodejs.org/dist/${NODEJS}/node-${NODEJS}-linux-x64.tar.xz && \
    tar -xJf node-${NODEJS}-linux-x64.tar.xz -C /usr/local --strip-components=1 --no-same-owner && \
    rm "node-${NODEJS}-linux-x64.tar.xz" && \
    ln -s /usr/local/bin/node /usr/local/bin/nodejs \
    node --version && \
    npm --version && \
    corepack enable && \
    make install

CMD [ "make", "run" ]
