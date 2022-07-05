ARG VERSION=0.28.0-kafka-3.1.1

FROM quay.io/strimzi/kafka:${VERSION}

ARG CONFLUENT_VERSION=7.1.1
ARG APICURIO=2.1.5.Final
ARG S3_SINK_VERSION=10.0.9
ARG DEBEZIUM_VERSION=1.9.4.Final

LABEL maintainer=loustler<dev.loustler@gmail.com>
LABEL confluent_version=${CONFLUENT_VERSION}
LABEL s3_sink_connector_version=${S3_SINK_VERSION}
LABEL debezium_version=${DEBEZIUM_VERSION}

ARG TEMP_PLUGIN_PATH=/tmp/plugins

USER root:root

COPY docker-maven-download.sh /usr/local/bin/docker-maven-download

ENV KAFKA_PLUGIN_DESTINATION=${KAFKA_HOME}/plugins

ENV MAVEN_DEP_DESTINATION=${KAFKA_HOME}/libs \
    EXTERNAL_LIBS_DIR=${KAFKA_HOME}/libs

RUN mkdir -p ${KAFKA_PLUGIN_DESTINATION} ${MAVEN_DEP_DESTINATION} ${EXTERNAL_LIBS_DIR} ${TEMP_PLUGIN_PATH}

RUN microdnf update

# See confluent packages' md5 in https://packages.confluent.io/maven/io/confluent/PACKAGE_NAME/${CONFLUENT_VERSION}/
RUN docker-maven-download confluent kafka-connect-avro-converter "${CONFLUENT_VERSION}" 273bee6b6be10e803e924320e984ae83 \
    && docker-maven-download confluent kafka-connect-avro-data "${CONFLUENT_VERSION}" 2558d58909813e9683952875f2757eb4 \
    && docker-maven-download confluent kafka-avro-serializer "${CONFLUENT_VERSION}" 907f02f30ee9d2730abbc90d0fe86707 \
    && docker-maven-download confluent kafka-schema-serializer "${CONFLUENT_VERSION}" 23ae2867f4a7e848bbeab43f00f6ad5f \
    && docker-maven-download confluent kafka-schema-registry-client "${CONFLUENT_VERSION}" 830fa1ab4e5077633569457ef7d79d49 \
    && docker-maven-download confluent common-config "${CONFLUENT_VERSION}" 1564ce087377c391dcc39a4d87109c8d \
    && docker-maven-download confluent common-utils "${CONFLUENT_VERSION}" 03fb1892bb2cb6931c312a04295a743f \
    && docker-maven-download central org/apache/avro avro 1.11.0 27b149b13a0843d28bd935151db87403 \
    && docker-maven-download confluent kafka-connect-storage-common 11.0.5 1b28fc57f166e20582d7d384899c21c8 \
    && docker-maven-download apicurio "${APICURIO}" bd7adf3f599968db4529fe4592d07bc8 \
    && docker-maven-download central com/google/guava guava 31.1-jre e37782d974104aa3b0a7bee9927c8042

# Install Kafka S3 sync connector
RUN curl https://d1i4a15mxbxib1.cloudfront.net/api/plugins/confluentinc/kafka-connect-s3/versions/${S3_SINK_VERSION}/confluentinc-kafka-connect-s3-${S3_SINK_VERSION}.zip -o ${TEMP_PLUGIN_PATH}/s3-kafka-connector.zip \
  && unzip ${TEMP_PLUGIN_PATH}/s3-kafka-connector.zip -d ${TEMP_PLUGIN_PATH} \
  && mkdir -p ${KAFKA_PLUGIN_DESTINATION}/confluentinc-kafka-connect-s3/${S3_SINK_VERSION} \
  && cp -R ${TEMP_PLUGIN_PATH}/confluentinc-kafka-connect-s3-${S3_SINK_VERSION}/* ${KAFKA_PLUGIN_DESTINATION}/confluentinc-kafka-connect-s3/${S3_SINK_VERSION} \
  && rm -rf ${TEMP_PLUGIN_PATH}/confluentinc-kafka-connect-s3-${S3_SINK_VERSION}

RUN docker-maven-download debezium mysql "${DEBEZIUM_VERSION}" 7e4a7183961b43bd276dce4f8a67065e \
   && docker-maven-download debezium mongodb "${DEBEZIUM_VERSION}" 27656349cf38d03e36eca911368f7655 \
   && docker-maven-download debezium postgres "${DEBEZIUM_VERSION}" 31d2525c2eb34d886830ac6ec962e073 \
   && docker-maven-download debezium sqlserver "${DEBEZIUM_VERSION}" f5fff7ca73338de5a56e46366fee0757 \
   && docker-maven-download debezium oracle "${DEBEZIUM_VERSION}" 1653d398d2b88185e7e6d56b25b032d8
