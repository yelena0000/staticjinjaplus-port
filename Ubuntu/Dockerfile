FROM ubuntu:24.04

ARG VERSION=0.1.1
WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3.11 \
    python3-pip \
    curl \
    && rm -rf /var/lib/apt/lists/*

RUN if [ "$VERSION" = "0.1.0" ]; then \
      URL="https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.0.tar.gz" && \
      HASH="3555bcfd670e134e8360ad934cb5bad1bbe2a7dad24ba7cafa0a3bb8b23c6444"; \
    elif [ "$VERSION" = "0.1.1" ]; then \
      URL="https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.1.tar.gz" && \
      HASH="30d9424df1eddb73912b0e2ad5375fa2c876c8e30906bec91952dfb75dcf220b"; \
    elif [ "$VERSION" = "main" ]; then \
      URL="https://github.com/MrDave/StaticJinjaPlus/archive/refs/heads/main.tar.gz" && \
      HASH="9adccb8fe17a40252df1a3acdea7edef4633b4ecaa8ba2dd5e0270f87ae43eab"; \
    else \
      echo "Unsupported version: $VERSION" && exit 1; \
    fi && \
    curl -L "$URL" -o SJP.tar.gz && \
    echo "$HASH  SJP.tar.gz" | sha256sum -c - && \
    tar xzf SJP.tar.gz --strip-components=1 && \
    rm SJP.tar.gz

RUN pip3 install --no-cache-dir -r requirements.txt

ENTRYPOINT ["python3", "main.py"]
CMD ["--srcpath", "/app/templates", "--outpath", "/app/output"]