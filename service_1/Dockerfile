FROM golang:1.20-alpine
WORKDIR /app
COPY . .
RUN go build -o main main.go
EXPOSE 8001
CMD ["./main"]
