FROM python:3.12.1-slim-bullseye

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        htop \
        gdebi-core \
        nano \
        openssh-client \
        git && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN pip install --no-cache-dir virtualenv

ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
    
WORKDIR /usr/workspace

COPY requirements.txt /usr/workspace/requirements.txt
RUN pip3 install -r /usr/workspace/requirements.txt

ENV QUARTO_VERSION="1.3.450"
    
RUN curl -o quarto-linux-amd64.deb -L https://github.com/quarto-dev/quarto-cli/releases/download/v${QUARTO_VERSION}/quarto-${QUARTO_VERSION}-linux-amd64.deb && \
        gdebi --non-interactive quarto-linux-amd64.deb && \
    rm quarto-linux-amd64.deb

RUN quarto check

EXPOSE 8888

CMD ["jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root"]