FROM quay.io/coatldev/six:3.13 AS base

ENV JYTHON_HOME=/opt/jython

RUN set -eux; \
    apt-get update --quiet; \
    apt-get install --yes --no-install-recommends \
        gnupg2 \
        iproute2 \
        openjdk-17-jre-headless \
        ssh \
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

FROM base AS starship-installer

RUN set -eux; \
    curl --silent --show-error --output /tmp/starship.sh https://starship.rs/install.sh; \
    chmod +x /tmp/starship.sh; \
    /tmp/starship.sh --yes

# hadolint ignore=SC2028
RUN set -eux; \
    echo '\neval "$(starship init bash)"' >> /root/.bashrc; \
    mkdir -p /root/.config; \
    starship preset nerd-font-symbols > /root/.config/starship.toml

# >============================================================================<

FROM base AS final

COPY --from=jython-builder ${JYTHON_HOME}/ ${JYTHON_HOME}/
COPY --from=starship-installer /usr/local/bin/starship /usr/local/bin/starship
COPY --from=starship-installer /root/.bashrc /root/.bashrc
COPY --from=starship-installer /root/.config/starship.toml /root/.config/starship.toml

ENV PATH="/root/.local/bin:${JYTHON_HOME}/bin:$PATH"

COPY requirements /tmp/requirements/

RUN set -eux; \
    \
    python -m pip install --requirement \
        /tmp/requirements/tools.txt; \
    \
    while IFS= read -r package_spec || [ -n "$package_spec" ]; do \
        if [ -n "$package_spec" ]; then \
            pipx install "$package_spec"; \
        fi; \
    done < /tmp/requirements/pipx.txt; \
    \
    rm -rf /tmp/requirements

CMD ["/bin/bash"]
