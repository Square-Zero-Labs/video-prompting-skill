FROM mcr.microsoft.com/devcontainers/base:bookworm

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    ffmpeg \
    jq \
    ripgrep \
  && rm -rf /var/lib/apt/lists/*
