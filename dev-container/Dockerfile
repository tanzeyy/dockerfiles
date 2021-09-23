FROM ubuntu:focal

ARG DEBIAN_FRONTEND=noninteractive

# Install packages
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        apt-utils \
        automake \
        autotools-dev \
        build-essential \
        ca-certificates \
        cmake \
        curl \
        ffmpeg \
        g++ \
        gcc \
        git \
        libbz2-dev \
        libcurl4-gnutls-dev \
        libffi-dev \
        libfuse-dev \
        libglib2.0-0 \
        liblzma-dev \
        libncurses5-dev \
        libreadline-dev \
        libsm6 \
        libsqlite3-dev \
        libssl-dev \
        libxext6 \
        libxml2-dev \
        libxmlsec1-dev \
        libxrender-dev \
        llvm \
        make \
        mecab-ipadic-utf8 \
        net-tools \
        ninja-build \
        pkg-config \
        sudo \
        tk-dev \
        vim \
        wget \
        xz-utils \
        zlib1g-dev \
        zsh

# Change Time Zone
RUN TZ=Asia/Shanghai && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata

# Setup default user
ARG USERNAME=work
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # Add sudo support
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME

# Install Oh-My-Zsh
RUN echo "exec $(which zsh)" >> /home/$USERNAME/.bashrc && \
    wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
COPY .zshrc /home/$USERNAME/.zshrc

# Install FZF
RUN git clone --depth 1 https://github.com/junegunn/fzf.git /home/$USERNAME/.fzf && \
    /home/$USERNAME/.fzf/install

# Configure vim
COPY vimrc /etc/vim/vimrc

# Install PyEnv
# Set-up necessary Env vars for PyEnv
ENV PYENV_ROOT /home/$USERNAME/.pyenv
ENV PATH $PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH
ENV PYTHON_VERSION 3.8.12
RUN set -ex \
    && curl https://pyenv.run | bash \
    && pyenv update \
    && pyenv install $PYTHON_VERSION \
    && pyenv global $PYTHON_VERSION \
    && pyenv rehash


WORKDIR /home/$USERNAME
CMD ["zsh"]
