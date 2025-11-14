FROM mcr.microsoft.com/devcontainers/base:ubuntu

# 1. Install core dependencies, Flux, Kubens, Lazygit, LazyVim, and Neovim
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    zsh curl git \
    ripgrep fd-find build-essential unzip \
    software-properties-common ca-certificates \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/* \
    # Install Flux
    && curl -s https://fluxcd.io/install.sh | bash \
    # Install Kubens/Kubectx
    && curl -L -o /usr/local/bin/kubens https://raw.githubusercontent.com/ahmetb/kubectx/master/kubens \
    && curl -L -o /usr/local/bin/kubectx https://raw.githubusercontent.com/ahmetb/kubectx/master/kubectx \
    && chmod +x /usr/local/bin/kubens /usr/local/bin/kubectx \
    # Manual Lazygit (x86_64) install
    && curl -L -o lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/download/v0.41.0/lazygit_0.41.0_Linux_x86_64.tar.gz" \
    && tar xf lazygit.tar.gz lazygit \
    && install lazygit /usr/local/bin \
    && rm lazygit lazygit.tar.gz \
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

# 3. Install LazyVim (clones the starter config)
RUN git clone https://github.com/LazyVim/starter ~/.config/nvim \
    && rm -rf ~/.config/nvim/.git

# 4. Add Kubectl Autocompletion
RUN echo '' >> /home/vscode/.zshrc \
    && echo '# kubectl autocompletion' >> /home/vscode/.zshrc \
    && echo 'if command -v kubectl &> /dev/null; then' >> /home/vscode/.zshrc \
    && echo '  source <(kubectl completion zsh)' >> /home/vscode/.zshrc \
    && echo 'fi' >> /home/vscode/.zshrc \
    && echo 'compdef _kubectl k' >> /home/vscode/.zshrc

# --- Enable kubectl autocomplete for BASH (permanent, safe) ---
USER vscode
RUN sudo apt-get update && sudo apt-get install -y bash-completion && \
    mkdir -p /etc/bash_completion.d && \
    kubectl completion bash | sudo tee /etc/bash_completion.d/kubectl >/dev/null && \
    { grep -qxF 'source /etc/bash_completion.d/kubectl' /home/vscode/.bashrc || echo 'source /etc/bash_completion.d/kubectl' >> /home/vscode/.bashrc; } && \
    { grep -qxF 'alias k=kubectl' /home/vscode/.bashrc || echo 'alias k=kubectl' >> /home/vscode/.bashrc; }

# Install dnsutils (for nslookup), git, and build-essential
RUN sudo apt-get update && sudo apt-get install -y \
    dnsutils \
    git \
    build-essential

# Install dnsutils (for nslookup), git, and build-essential
RUN sudo apt-get update && sudo apt-get install -y \
    dnsutils \
    git \
    build-essential
USER root

USER root
