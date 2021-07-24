FROM golang:1.16.5-alpine3.14 AS build

WORKDIR /src/main
COPY hello.go go.* /src/main

WORKDIR /src
RUN go mod init mt5225.com/main/v2
RUN CGO_ENABLED=0 go build -o /bin/hello /src/main/hello.go 

FROM scratch
COPY --from=build /bin/hello /bin/hello
ENTRYPOINT ["/bin/hello"]