FROM coatldev/six:3.12

COPY requirements /tmp/requirements

RUN set -eux; \
    \
    python3 -m pip install --requirement \
        /tmp/requirements/requirements.txt

CMD ["/bin/bash"]
