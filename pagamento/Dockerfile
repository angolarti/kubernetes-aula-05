FROM golang:1.15

WORKDIR /go/src/pagamento

COPY . .

RUN GOOS=linux go build -o pagamento

EXPOSE 9091

ENTRYPOINT ["./pagamento"]