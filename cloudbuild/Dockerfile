FROM alpine:3.9

ARG PACKER_VERSION=1.4.5
ARG PACKER_VERSION_SHA256SUM=30da8dab9c526a6d15b037e2234f6f12cf3accfad77eb2c130738ec1a54cab6d
ARG PACKER_GOSS_VERSION=v0.3.0

COPY packer_${PACKER_VERSION}_linux_amd64.zip .
COPY packer-provisioner-goss-${PACKER_GOSS_VERSION}-linux-amd64 .
RUN echo "${PACKER_VERSION_SHA256SUM}  packer_${PACKER_VERSION}_linux_amd64.zip" > checksum && sha256sum -c checksum

RUN /usr/bin/unzip packer_${PACKER_VERSION}_linux_amd64.zip
RUN chmod +x packer-provisioner-goss-${PACKER_GOSS_VERSION}-linux-amd64 && mv packer-provisioner-goss-${PACKER_GOSS_VERSION}-linux-amd64 packer-provisioner-goss

FROM ubuntu
RUN apt-get -y update && apt-get -y install ca-certificates && rm -rf /var/lib/apt/lists/*
COPY --from=0 packer /usr/bin/packer
COPY --from=0 packer-provisioner-goss  /usr/bin/packer-provisioner-goss
ENTRYPOINT ["/usr/bin/packer"]
