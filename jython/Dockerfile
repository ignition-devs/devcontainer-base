FROM quay.io/coatldev/six:3.12 AS base

ENV JYTHON_HOME=/opt/jython

# install Java
RUN set -eux; \
    apt-get update --quiet; \
    apt-get install --yes --no-install-recommends \
        openjdk-17-jre-headless \
    ; \
    rm -rf /var/lib/apt/lists/*

# >============================================================================<

FROM base AS jython-builder

ENV JYTHON_VERSION=2.7.3

WORKDIR /tmp

RUN set -eux; \
    \
    wget -q "https://repo1.maven.org/maven2/org/python/jython-installer/${JYTHON_VERSION}/jython-installer-${JYTHON_VERSION}.jar"; \
    \
    java -jar "jython-installer-${JYTHON_VERSION}.jar" \
        --silent \
        --type standard \
        --directory "${JYTHON_HOME}"

# >============================================================================<

FROM base AS final

COPY --from=jython-builder ${JYTHON_HOME}/ ${JYTHON_HOME}/

ENV PATH="${JYTHON_HOME}/bin:$PATH"

COPY requirements /tmp/requirements/

RUN set -eux; \
    \
    python3 -m pip install --requirement \
        /tmp/requirements/requirements.txt

CMD ["/bin/bash"]
