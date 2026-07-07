FROM mcr.microsoft.com/devcontainers/base:resolute

USER vscode
RUN curl -fsSL --retry 5 "https://mise.run" \
    | MISE_INSTALL_PATH=/home/vscode/.local/bin/mise sh \
    && git config --global safe.directory '*' \
    && git config --global core.autocrlf input

RUN sudo apt update \
    && sudo apt install -y locales fonts-noto-cjk bash-completion \
    && sudo locale-gen zh_CN.UTF-8 \
    && sudo update-locale LANG=zh_CN.UTF-8 LC_ALL=zh_CN.UTF-8

ENV PATH="/home/vscode/.local/bin:/home/vscode/.local/share/mise/shims:/home/vscode/.cargo/bin:${PATH}"
ENV MISE_DATA_DIR="/home/vscode/.local/share/mise"

RUN mise use -g "conda:opencode[channel=https://prefix.dev/github-releases]" \
    && mise use -g conda:starship \
    && echo 'eval "$(mise completion bash)"' >> /home/vscode/.bashrc \
    && echo 'eval "$(starship init bash)"' >> /home/vscode/.bashrc

WORKDIR /workspace