FROM --platform=$BUILDPLATFORM golang:alpine AS builder

RUN apk add --no-cache make bash git npm unzip ca-certificates && update-ca-certificates

WORKDIR /app

COPY . .

ARG SKIP_WEB=false
ENV SKIP_WEB=$SKIP_WEB

RUN make build-hub

FROM scratch

COPY --from=builder /app/build/beszel_linux_amd64 /beszel
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

EXPOSE 8090

ENTRYPOINT ["/beszel"]
CMD ["serve", "--http=0.0.0.0:8090"]