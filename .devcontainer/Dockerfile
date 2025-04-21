# Use the official .NET SDK image as a base.
# Choose an LTS version like 8.0 for stability.
# See https://mcr.microsoft.com/dotnet/sdk/tags for other versions.
FROM mcr.microsoft.com/dotnet/sdk:8.0

# Prevent prompts during apt-get install
ENV DEBIAN_FRONTEND=noninteractive

# Install common tools needed for development and by Dev Containers
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    curl \
    sudo \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Optional: Install Node.js and npm if you might need them for frontend tooling or Blazor WebAssembly scenarios
# (Uncomment the following lines if needed)
# ARG NODE_VERSION="lts/*"
# RUN curl -fsSL https://deb.nodesource.com/setup_$NODE_VERSION.x | bash - && \
#    apt-get install -y --no-install-recommends nodejs && \
#    rm -rf /var/lib/apt/lists/*

# --- User Setup (for VS Code Dev Containers) ---
# Create a non-root user with the same UID/GID as the host user
# This helps with file permissions when mounting volumes
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000

RUN groupadd --gid $USER_GID $USERNAME || true && \
    useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME || true && \
    usermod -aG sudo $USERNAME

# Configure sudo to not ask for a password for the created user
# Use tee for more reliable writing to privileged files
RUN echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" | tee /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME # Corrected line

# Set the working directory
WORKDIR /workspaces

# Switch to the non-root user
USER $USERNAME

# Set the shell for the created user
RUN echo "export PATH=\$PATH:/home/$USERNAME/.dotnet/tools" >> /home/$USERNAME/.bashrc