FROM codercom/code-server:3.9.1

USER root

# uid
# RUN apt-get update && apt-get -y install gosu
# COPY entrypoint.sh /usr/local/bin/entrypoint.sh
# RUN chmod +x /usr/local/bin/entrypoint.sh
# ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

# Install wget and other module
RUN apt-get update && apt-get install -y \
    wget \
    git

# build nuget error temporary workaround
# https://github.com/NuGet/Home/issues/10491#issuecomment-768450905
RUN curl -o /usr/local/share/ca-certificates/verisign.crt -SsL https://crt.sh/?d=1039083 && update-ca-certificates

# Install DotNet Package Key
RUN wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && rm packages-microsoft-prod.deb

# Install Dotnet SDK
RUN apt-get update; \
    apt-get install -y apt-transport-https && \
    apt-get update && \
    apt-get install -y dotnet-sdk-5.0

# Install Dotnet Runtime
RUN apt-get update; \
    apt-get install -y apt-transport-https && \
    apt-get update && \
    apt-get install -y aspnetcore-runtime-5.0

# Install nodejs
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && apt install -y \
    nodejs \
    gcc \
    g++ \
    make \
    && curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt install -y yarn

# Download the Samsung Debugger
RUN curl -sL https://github.com/Samsung/netcoredbg/releases/download/1.2.0-672/netcoredbg-linux-bionic-amd64.tar.gz | tar -zx -C /home/coder
ENV PATH "${PATH}:/home/coder/netcoredbg"

# code-server
EXPOSE 8080

# Replace 1000 with your user / group id
ARG uid=1000
ARG gid=1000

RUN chown ${uid}:${gid} -R /home/coder/

USER coder