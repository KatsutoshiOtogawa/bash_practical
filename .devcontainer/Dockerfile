# [Choice] Ubuntu Version 18.04,20.04
ARG VARIANT=20.04
FROM ubuntu:${VARIANT}

ENV PYTHONUNBUFFERED 1

# Update args in docker-compose.yaml to set the UID/GID of the "vscode" user.
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then \
        groupmod --gid $USER_GID vscode \
        && usermod --uid $USER_UID --gid $USER_GID vscode \
        && chmod -R $USER_UID:$USER_GID /home/vscode; \
    fi


RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
     && apt-get -y install --no-install-recommends git postgresql-client



