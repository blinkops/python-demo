FROM golang:1.16.3 AS builder

WORKDIR /go/src/github.com/blinkops/blink-python-template
COPY .. .

RUN go build -tags netgo -ldflags '-w -extldflags "-static"' -o /go/bin/plugin ./main.go

FROM python:3.7-alpine as plugin

WORKDIR /blink-base
COPY --from=builder /go/bin/plugin .
COPY config.yaml plugin.yaml ./
COPY actions/ ./actions/

# Expose the gRPC port
EXPOSE 1337

RUN chmod a+x plugin

ENTRYPOINT ./plugin