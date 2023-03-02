FROM airbyte/integration-base-java:dev AS build

WORKDIR /airbyte

ENV APPLICATION destination-kafka

COPY build/distributions/${APPLICATION}*.tar ${APPLICATION}.tar

RUN tar xf ${APPLICATION}.tar --strip-components=1 && rm -rf ${APPLICATION}.tar

FROM airbyte/integration-base-java:dev

WORKDIR /airbyte

ENV APPLICATION destination-kafka

COPY --from=build /airbyte /airbyte

ENV YANDEX_CERT_PATH /usr/local/share/ca-certificates/Yandex

RUN mkdir --parents ${YANDEX_CERT_PATH} && \
    curl "https://storage.yandexcloud.net/cloud-certs/CA.pem" \
        --output ${YANDEX_CERT_PATH}/YandexInternalRootCA.crt && \
    chmod 655 ${YANDEX_CERT_PATH}/YandexInternalRootCA.crt

RUN ${JAVA_HOME}/bin/keytool -import -file ${YANDEX_CERT_PATH}/YandexInternalRootCA.crt -keystore ${JAVA_HOME}/lib/security/cacerts -storepass changeit -noprompt

LABEL io.airbyte.version=0.1.10-yandex
LABEL io.airbyte.name=airbyte/destination-kafka