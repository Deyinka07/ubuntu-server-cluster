FROM mcr.microsoft.com/devcontainers/base:ubuntu

# 1. Install core dependencies and LazyVim requirements
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    zsh curl git \
    ripgrep fd-find build-essential unzip \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/* \
    # Symlink 'fd'
    && ln -s $(which fdfind) /usr/local/bin/fd \
    # Manual Neovim (x86_64) installation
    && curl -LO https://github.com/neovim/neovim/releases/download/stable/nvim-linux-x86_64.tar.gz \
    && rm -rf /opt/nvim \
    && tar -C /opt -xzf nvim-linux-x86_64.tar.gz \
    && rm nvim-linux-x86_64.tar.gz

# Add Neovim to PATH for all users
ENV PATH="$PATH:/opt/nvim-linux-x86_64/bin"

# Switch to the non-root 'vscode' user for user-specific configs
USER vscode
WORKDIR /home/vscode

# 2. Configure Zsh and Aliases
RUN sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="agnoster"/' /home/vscode/.zshrc \
    && echo 'alias k=kubectl' >> /home/vscode/.bashrc \
    && echo 'alias k=kubectl' >> /home/vscode/.zshrc \
    && echo 'export EDITOR=nvim' >> /home/vscode/.zshrc

# 3. Install LazyVim
RUN git clone https://github.com/LazyVim/starter ~/.config/nvim \
    && rm -rf ~/.config/nvim/.git

USER root
