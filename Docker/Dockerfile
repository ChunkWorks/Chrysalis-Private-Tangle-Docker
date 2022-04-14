FROM debian:bullseye-slim
MAINTAINER Peter Willemsen <peter@tanglehub.eu>
RUN echo "Installing dependencies..." && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y wget nano && \
    apt-get dist-upgrade -y && \
	rm -rf /var/lib/apt/lists/*

WORKDIR /opt/hornet

RUN /bin/bash -c 'set -ex && \
    ARCH=`uname -m` && \
    echo "Installing Hornet Binaries..." && \
    if [ "$ARCH" == "x86_64" ]; then \
       wget https://github.com/gohornet/hornet/releases/download/v1.2.0/HORNET-1.2.0_Linux_x86_64.tar.gz -O hornet.tar.gz; \
    else \
       wget https://github.com/gohornet/hornet/releases/download/v1.2.0/HORNET-1.2.0_Linux_ARM64.tar.gz -O hornet.tar.gz; \
    fi && \
    tar --strip-components=1 -xvf hornet.tar.gz && rm hornet.tar.gz'

WORKDIR /opt/hornet/private_tangle

RUN echo "Patching a few things..." && \
    sed -i 's/go run ..\/main.go/..\/hornet/' run_coo_bootstrap.sh create_snapshot_private_tangle.sh && \
    sed -i 's/localhost:8081/0.0.0.0:8081/' run_coo_bootstrap.sh && \
    sed -i 's/Spammer,//' run_coo_bootstrap.sh

ENTRYPOINT /opt/hornet/private_tangle/run_coo_bootstrap.sh