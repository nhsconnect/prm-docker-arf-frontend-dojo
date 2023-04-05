FROM node:18.15.0-bullseye-slim

# Install common Dojo scripts
ENV DOJO_VERSION=0.10.2
RUN userdel node
RUN apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
  git ca-certificates tini sudo && \
  git clone --depth 1 -b ${DOJO_VERSION} https://github.com/kudulab/dojo.git /tmp/dojo_git &&\
  /tmp/dojo_git/image_scripts/src/install.sh && \
  rm -r /tmp/dojo_git && \
  rm -rf /var/lib/apt/lists/*

# Install AWS CLI
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    curl groff zip unzip &&\
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" &&\
    unzip awscliv2.zip &&\
    sudo ./aws/install &&\
    rm -rf awscliv2.zip

# Install jq to parse json in bash
RUN apt-get install -y jq

# Cypress & start-server-and-test dependencies
ENV CYPRESS_VERSION=12.8.1
RUN apt-get update && apt-get install -y --no-install-recommends \
    libgtk2.0-0 \
    libgtk-3-0 \
    libgbm-dev \
    libnotify-dev \
    libgconf-2-4 \
    libnss3 libxss1 \
    libasound2 \
    libxtst6 \
    xauth \
    xvfb

RUN mkdir "/home/dojo/node_modules" "/home/dojo/.cache" "/home/dojo/.cache/Cypress"
RUN npm config set prefix "/home/dojo/node_modules"
RUN CYPRESS_CACHE_FOLDER=/home/dojo/.cache/Cypress npm i -g cypress@${CYPRESS_VERSION} --unsafe-perm

COPY profile /home/dojo/.profile
COPY bashrc /home/dojo/.bashrc
RUN chown dojo:dojo /home/dojo/.profile /home/dojo/.bashrc 
RUN chown -R dojo:dojo /home/dojo/node_modules /home/dojo/.cache/Cypress

ENTRYPOINT ["/usr/bin/tini", "-g", "--", "/usr/bin/entrypoint.sh"]
CMD ["/bin/bash"]