FROM golang:1.17 AS builder
RUN groupadd -g 999 user && \
    useradd -r -u 999 -g user user
COPY --chown=user:user . /build
ENV GO111MODULE=on \
    GOPROXY=https://goproxy.oneitfarm.com,https://goproxy.cn,direct \
    CGO_ENABLED=0
WORKDIR /build/cmd/ssl-expiry-check
RUN go build
FROM scratch
COPY --from=builder /etc/passwd /etc/passwd
USER user
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /build/cmd/ssl-expiry-check/ssl-expiry-check /app/ssl-expiry-check
ENTRYPOINT ["/app/ssl-expiry-check"]