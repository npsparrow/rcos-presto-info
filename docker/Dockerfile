# Start with Ubuntu as the base image
# Ubuntu 22.04 LTS (Jammy Jellyfish) provides a stable foundation
FROM ubuntu:22.04

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Set the working directory in the container
WORKDIR /workspace

# Update package lists and install essential development tools
# We combine commands using && to create fewer layers and reduce image size
RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    python3 \
    python3-pip \
    python3-venv \
    openjdk-8-jdk \
    maven \
    libaio1 libaio-dev \
    libnuma1 libnuma-dev \
    curl \
    vim \
    zsh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN echo 'root:Docker!' | chpasswd

# Create a non-root user for better security
# Replace 'developer' with your preferred username if desired
RUN useradd -m -s /bin/bash developer && \
    chown -R developer:developer /workspace
USER developer

# Install Oh My Zsh for enhanced ZSH functionality
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# Customize ZSH configuration
# Note: Using COPY would require root permissions, so we echo the contents instead
RUN echo '\
# Set ZSH theme\n\
ZSH_THEME="robbyrussell"\n\
\n\
# Enable useful Oh My Zsh plugins\n\
plugins=(\n\
    git\n\
    python\n\
    docker\n\
    command-not-found\n\
    zsh-autosuggestions\n\
    zsh-syntax-highlighting\n\
)\n\
\n\
# Load Oh My Zsh\n\
source $ZSH/oh-my-zsh.sh\n\
\n\
# Custom aliases\n\
alias ll="ls -la"\n\
alias python="python3"\n\
' >> ~/.zshrc

# Install popular ZSH plugins for better functionality
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# Set ZSH as the default shell when container starts
CMD ["zsh"]

# Note: To build this image:
# docker build -t ubuntu-presto .
