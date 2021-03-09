ARG VERSION=2.0.1

FROM apache/airflow:${VERSION}

LABEL maintainer="dev.loustler@gmail.com"

ARG requirements_path=/home/airflow

USER root

COPY requirements.txt ${requirements_path}

USER 50000