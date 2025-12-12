FROM golang:1.21-alpine AS builder

RUN apk add --no-cache git
RUN go install tailscale.com/cmd/derper@main

FROM alpine:latest

RUN apk add --no-cache ca-certificates

COPY --from=builder /go/bin/derper /usr/local/bin/derper

EXPOSE 3478/udp 3479/udp 5349/tcp

ENTRYPOINT ["derper"]
