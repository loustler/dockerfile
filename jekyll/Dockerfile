FROM ruby:2.6

LABEL maintainer="dev.loustler@gmail.com"

RUN groupadd --gid 1001 rubyuser && useradd --gid 1001 --uid 1001 rubyuser --shell /bin/bash
RUN chown -R rubyuser:rubyuser /opt
RUN mkdir /home/rubyuser && chown -R rubyuser:rubyuser /home/rubyuser
RUN mkdir /logs && chown -R rubyuser:rubyuser /logs

WORKDIR /home/rubyuser

RUN apt update && \
    apt upgrade -y && \
    gem install github-pages

USER rubyuser

EXPOSE 4000