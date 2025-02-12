FROM socrata/runit-focal-ecs
MAINTAINER Socrata <sysadmin@socrata.com>

# Add the NodeSource apt repository. Instructions taken from:
# https://github.com/nodesource/distributions/blob/master/README.md#debmanual
ENV NODE_VERSION=node_20.x
RUN mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg && \
    echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/$NODE_VERSION nodistro main" | \
    tee /etc/apt/sources.list.d/nodesource.list && \
    echo "deb-src [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/$NODE_VERSION nodistro main" | \
    tee -a /etc/apt/sources.list.d/nodesource.list
RUN apt-get -y update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y libyajl2 nodejs git

# LABEL must be last for proper base image discoverability
LABEL repository.socrata/runit-nodejs-focal:20x-ecs=""
