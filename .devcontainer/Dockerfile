FROM ubuntu as builder
USER root
RUN apt-get update && apt-get install -y wget
WORKDIR /app
ENV TSFILE=tailscale_1.32.3_amd64.tgz
RUN mkdir -p binaries && \
  wget https://pkgs.tailscale.com/stable/${TSFILE} && \
  tar xzf ${TSFILE} --strip-components=1 -C binaries

FROM mcr.microsoft.com/vscode/devcontainers/universal:linux as main
USER root

RUN apt-get update && apt-get install -y curl gpg dnsutils sudo
COPY .devcontainer/tailscaled /etc/init.d/tailscaled
RUN chmod +x /etc/init.d/tailscaled
COPY --from=builder /app/binaries/tailscaled /usr/sbin/tailscaled
COPY --from=builder /app/binaries/tailscale /usr/bin/tailscale

RUN mkdir -p /var/run/tailscale /var/cache/tailscale /var/lib/tailscale
