FROM golang:1.23.0-alpine3.20 AS builder

RUN apk add --no-cache ca-certificates
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -ldflags="-s -w" -o tesoro-backend .


FROM scratch

COPY --from=builder /app/tesoro-backend /app/tesoro-backend
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
ENTRYPOINT ["/app/tesoro-backend"]
EXPOSE 8080