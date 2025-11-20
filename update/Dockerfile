FROM golang:1.22

WORKDIR /app

# Evitar que Go use Google storage
ENV GOPROXY=https://goproxy.io,direct
ENV GOSUMDB=off

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN go build -o main .
