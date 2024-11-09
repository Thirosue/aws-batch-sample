# ベースイメージにPythonを指定
FROM public.ecr.aws/docker/library/python:3.12-slim

# システムパッケージのインストール（必要に応じて）
RUN apt-get update && apt-get install -y build-essential

# 作業ディレクトリの設定
WORKDIR /app

# Poetryのインストール
RUN pip install poetry

# 依存関係ファイルをコピー
COPY pyproject.toml poetry.lock /app/

# 依存関係のインストール
RUN poetry config virtualenvs.create false && \
    poetry install --no-dev

# アプリケーションコードをコピー
COPY lambda_handler/ lambda_handler/

# エントリーポイントの設定
CMD ["python", "lambda_handler/lambda_handler.py"]
