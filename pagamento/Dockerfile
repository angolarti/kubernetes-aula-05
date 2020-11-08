FROM golang:1.15

WORKDIR /go/src/pagamento

COPY . .

RUN GOOS=linux go build

EXPOSE 9091

ENTRYPOINT ["./avancadev-microservice"]