ARG BASE_CONTAINER=python:3.11.4-slim-bullseye
FROM $BASE_CONTAINER

LABEL maintainer="Bastian Aballay <bastian.aballayleiva@gmail.com>"

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME && \
    useradd --uid $USER_UID --gid $USER_GID -m $USERNAME && \
    apt-get update && \
    apt-get install -y sudo && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME && \
    apt-get install -y --no-install-recommends \
        sudo \
        curl \
        pandoc \
        pandoc-citeproc \
        htop \
        gdebi-core \
        git \
        ssh \
        zsh \
        nano && \
    rm -rf /var/lib/apt/lists/*

ENV QUARTO_VERSION="1.3.450"
    
RUN curl -o quarto-linux-amd64.deb -L https://github.com/quarto-dev/quarto-cli/releases/download/v${QUARTO_VERSION}/quarto-${QUARTO_VERSION}-linux-amd64.deb && \
    gdebi --non-interactive quarto-linux-amd64.deb && \
    rm quarto-linux-amd64.deb

RUN quarto check

ENV PATH="/home/${USERNAME}/.local/bin:${PATH}"

USER $USERNAME

COPY --chown=${USERNAME}:1000 requirements.txt /tmp/requirements.txt

RUN pip install --no-cache-dir --upgrade pip && \
    pip install -r /tmp/requirements.txt && \
    rm /tmp/requirements.txt

WORKDIR /home/${USERNAME}/workspace

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

EXPOSE 8888

ENTRYPOINT ["jupyter", "lab","--ip=0.0.0.0","--allow-root"]