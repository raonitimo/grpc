############################
# STEP 1 build executable binary
############################

FROM golang:alpine AS builder

RUN apk update && apk add --no-cache dev86 gcc musl-dev git
RUN go get -v google.golang.org/grpc
RUN sed -i -e 's/localhost/0.0.0.0/' $GOPATH/src/google.golang.org/grpc/examples/route_guide/server/server.go

WORKDIR $GOPATH/src/google.golang.org/grpc/examples/route_guide/server

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -ldflags '-w -s -extldflags "-static"' -o /go/bin/grpc_server
RUN ls -lh /go/bin/

############################
# STEP 2 build a small image
############################
FROM scratch

# Copy our static executable.
COPY --from=builder /go/bin/grpc_server /go/bin/grpc_server

EXPOSE 10000

# Run the hello binary.
ENTRYPOINT ["/go/bin/grpc_server"]
