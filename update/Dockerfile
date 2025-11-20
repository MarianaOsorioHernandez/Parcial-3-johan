# Etapa de compilación
FROM golang:1.25 AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN go build -o main .

# Etapa final mínima
FROM gcr.io/distroless/base-debian12

COPY --from=builder /app/main .

EXPOSE 8000

CMD ["./main"]
