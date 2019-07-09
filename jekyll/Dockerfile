FROM ruby:2.6

LABEL maintainer="dev.loustler@gmail.com"

RUN apt update && \
    apt upgrade -y && \
    gem install github-pages

EXPOSE 4000