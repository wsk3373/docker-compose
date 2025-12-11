# 官方中国加速地址（永远有效）
FROM --platform=$BUILDPLATFORM docker.m.daocloud.io/golang:1.23-alpine AS builder

# 直接从 GitHub 下载二进制（国内加速节点，超快）
RUN arch=$(uname -m) && \
    case $arch in \
      x86_64)   file=amd64-v2    ;; \
      aarch64)  file=arm64-v2    ;; \
      armv7l)   file=armv7-v2    ;; \
      riscv64)  file=riscv64-v2  ;; \
      *)        file=amd64-compatible ;; \
    esac && \
    wget -O mihomo.gz https://github.com/MetaCubeX/mihomo/releases/download/v1.19.17/mihomo-linux-${file}-v1.19.17.gz && \
    gunzip mihomo.gz && chmod +x mihomo && mv mihomo /usr/bin/

# 运行时也用官方中国加速地址
FROM docker.m.daocloud.io/alpine:latest

RUN apk add --no-cache ca-certificates tzdata
ENV TZ=Asia/Shanghai
COPY --from=builder /usr/bin/mihomo /usr/bin/mihomo
COPY config/ /etc/mihomo/
EXPOSE 7890 7891 9090 53/udp
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]