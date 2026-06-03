FROM jenkins/jenkins:lts

USER root

RUN apt-get update && apt-get install -y \
    wget \
    apt-transport-https \
    ca-certificates \
    gnupg \
    curl \
    docker.io

# .NET repo
RUN wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb

RUN mkdir -p /etc/apt/keyrings \
    && curl -fsSL https://packages.microsoft.com/keys/microsoft.asc \
    | gpg --dearmor -o /etc/apt/keyrings/microsoft.gpg

RUN echo "deb [signed-by=/etc/apt/keyrings/microsoft.gpg] https://packages.microsoft.com/debian/12/prod bookworm main" \
    > /etc/apt/sources.list.d/microsoft-prod.list

RUN apt-get update && apt-get install -y dotnet-sdk-8.0

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

USER jenkins