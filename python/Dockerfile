FROM quay.io/coatldev/six:3.13

COPY requirements /tmp/requirements

RUN set -eux; \
    \
    python -m pip install --requirement \
        /tmp/requirements/requirements.txt

CMD ["/bin/bash"]
