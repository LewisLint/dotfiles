FROM archlinux:latest

# Step 1: Initialize keys and install core packages
RUN pacman-key --init \
    && pacman-key --populate archlinux \
    && pacman -Syu --noconfirm \
    && pacman -S --needed --noconfirm base-devel go go-tools gopls git shadow sudo zsh curl vim \
    && pacman -Scc --noconfirm

# Step 2: Set up user environment permissions
RUN useradd --create-home --uid 1000 --user-group --shell /usr/bin/zsh vscode \
    && printf 'vscode ALL=(ALL) NOPASSWD: ALL\n' > /etc/sudoers.d/vscode \
    && chmod 0440 /etc/sudoers.d/vscode

USER vscode
WORKDIR /home/vscode

COPY --chown=vscode:vscode .zshrc .vimrc /home/vscode/

# Build yay as an unprivileged user, then use it to install vim-plug.
RUN git clone https://aur.archlinux.org/yay.git /tmp/yay \
    && cd /tmp/yay \
    && makepkg -si --noconfirm \
    && rm -rf /tmp/yay \
    && yay -S --needed --noconfirm vim-plug
