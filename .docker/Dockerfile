# docker build --build-arg http_proxy=http://172.16.4.26:17777 --build-arg https_proxy=http://172.16.4.26:17777
FROM registry.paas.ptcloud.t.home/paas/gobase:1.13.4-alpine3.10 AS gobuild

ENV GO111MODULE=on
WORKDIR /go-fluentd
COPY go.mod .
COPY go.sum .
RUN go mod download

# static build
ADD . .
RUN go build -a --ldflags '-extldflags "-static"' entrypoints/main.go


# copy executable file and certs to a pure container
FROM alpine:3.10
COPY --from=gobuild /etc/ssl/certs /etc/ssl/certs
COPY --from=gobuild /go-fluentd/main go-fluentd

CMD ["./go-fluentd", "--config=/etc/go-fluentd/settings"]
