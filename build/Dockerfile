### Build stage
FROM golang:alpine3.9 AS build-env
ENV VERSION v2.2.1
# Install build deps
RUN apk update && apk --no-cache add git gcc build-base
# Get the source code
RUN git clone -b ${VERSION} https://github.com/vipnode/vipnode.git

WORKDIR /go/vipnode
RUN go get -d -v
RUN go mod download
RUN GOOS=linux GOARCH=amd64 go build -ldflags="-w -s" -o vipnode

### Final stage
FROM alpine:3.9
RUN apk update && apk --no-cache add ca-certificates
WORKDIR /app
COPY --from=build-env /go/vipnode/vipnode /usr/bin/
ENTRYPOINT vipnode host --rpc=http://my.ethchain.dnp.dappnode.eth:8545 -vv --payout=${PAYOUT_ADDRESS} --nodekey=/app/.ethchain/network/key
