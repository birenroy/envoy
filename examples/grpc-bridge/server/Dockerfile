FROM golang:1.19.4-bullseye@sha256:9d235ae41bff699b44255bf912e77acac5046b06e61b018561239a35d9bd2ebb as builder

WORKDIR /build

# Resolve and build Go dependencies as Docker cache
COPY go.mod /build/go.mod
COPY go.sum /build/go.sum
COPY kv/go.mod /build/kv/go.mod

ENV GO111MODULE=on
RUN go mod download

COPY service.go /build/main.go
COPY kv/ /build/kv

# Build for linux
ENV GOOS=linux
ENV GOARCH=amd64
ENV CGO_ENABLED=0
RUN go build -o server

# Build the main container (Linux Runtime)
FROM debian:bullseye-slim@sha256:98d3b4b0cee264301eb1354e0b549323af2d0633e1c43375d0b25c01826b6790
WORKDIR /root/

# Copy the linux amd64 binary
COPY --from=builder /build/server /bin/

ENTRYPOINT /bin/server
