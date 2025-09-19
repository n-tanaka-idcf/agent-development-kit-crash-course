FROM ghcr.io/astral-sh/uv:0.8.19 AS uv
FROM python:3.13.7-slim-bookworm

ARG USER=vscode
ARG GROUP=${USER}
ARG UID
ARG GID
ARG WORKDIR=/app

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    bash-completion=1:2.11-6 \
    sudo=1.9.13p3-1+deb12u2 \
    procps=2:4.0.2-3 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
RUN groupadd --gid ${GID} ${GROUP} && \
    useradd --uid ${UID} --gid ${GID} --groups sudo --shell /bin/bash --create-home ${USER} && \
    echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
COPY --from=uv /uv /uvx /bin/

USER ${USER}
WORKDIR ${WORKDIR}
COPY --chown=${USER}:${GROUP} ../requirements*.txt ./
# shellcheck disable=SC1091
RUN uv venv .venv && \
    uv pip install --no-cache-dir -r requirements-dev.txt --python .venv/bin/python
