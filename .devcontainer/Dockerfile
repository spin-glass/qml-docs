# TeXLive 環境のステージ
FROM ghcr.io/paperist/texlive-ja:latest AS tex

# Python 環境のステージ
FROM python:3.13-slim AS python

# ユーザーを root に設定
USER root

# 必要なシステムパッケージをインストール
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    poppler-utils locales vim fonts-ipaexfont imagemagick procps texlive-luatex && \
    rm -rf /var/lib/apt/lists/* /tmp/*.deb

RUN locale-gen ja_JP.UTF-8 && localedef -f UTF-8 -i ja_JP ja_JP
ENV LANG=ja_JP.UTF-8
ENV LANGUAGE=ja_JP:ja
ENV LC_ALL=ja_JP.UTF-8

# TeXLive を python ステージにコピー
COPY --from=tex /usr/local/texlive /usr/local/texlive

# TeXLive のバイナリパスを PATH に追加
ENV PATH=/usr/local/texlive/2024/bin/aarch64-linux:$PATH
ENV PATH=/usr/local/texlive/2024/bin/x86_64-linux:$PATH

RUN apt-get -y purge texlive-base texlive-latex-base texlive-luatex texlive-lang-japanese && \
    rm -f /usr/bin/tlmgr

# 作業ディレクトリの設定
WORKDIR /workspace

# Pythonライブラリの依存関係をインストール
COPY pyproject.toml pdm.lock /workspace/
RUN pip install --no-cache-dir pdm \
    && pdm config python.use_venv True

ENV PATH=/workspace/.venv/bin:$PATH
