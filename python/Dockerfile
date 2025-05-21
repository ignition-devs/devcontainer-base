FROM quay.io/coatldev/six@sha256:c21ef0a793f9ce3ecc3cd7f5ba09a20ffbec3ebb56c7c09dd16497deb79bc9ae AS base

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

COPY --from=starship-installer /usr/local/bin/starship /usr/local/bin/starship
COPY --from=starship-installer /root/.bashrc /root/.bashrc
COPY --from=starship-installer /root/.config/starship.toml /root/.config/starship.toml

ENV PATH="/root/.local/bin:$PATH"

COPY requirements /tmp/requirements

RUN set -eux; \
    \
    python2 -m pip install --requirement \
        /tmp/requirements/2.7/virtualenv.txt; \
    \
    python -m pip install --requirement \
        /tmp/requirements/3.13/pipx.txt; \
    \
    while IFS= read -r tool || [ -n "$tool" ]; do \
        if [ -n "$tool" ]; then \
            pipx install "$tool"; \
        fi; \
    done < /tmp/requirements/pipx/tools.txt; \
    \
    rm -rf /tmp/requirements

CMD ["/bin/bash"]
