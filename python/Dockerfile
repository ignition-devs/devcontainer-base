FROM quay.io/coatldev/six:3.13 AS base

RUN set -eux; \
    apt-get update --quiet; \
    apt-get install --yes --no-install-recommends \
        bash-completion \
        gnupg2 \
        iproute2 \
        ssh \
    ; \
    rm -rf /var/lib/apt/lists/*

# Enable programmable completion features
# hadolint ignore=SC2028
RUN set -eux; \
    echo '\n. /etc/bash_completion' >> /root/.bashrc

# >============================================================================<

FROM base AS starship-installer

COPY --from=base /root/.bashrc /root/.bashrc

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
