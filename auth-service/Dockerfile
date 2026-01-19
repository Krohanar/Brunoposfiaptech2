# =========================
# STAGE 1 — BUILD
# =========================
FROM golang:1.21-alpine AS builder

RUN apk add --no-cache ca-certificates

WORKDIR /app

# Copia tudo primeiro (pra tidy enxergar os imports)
COPY . .

# Gera/atualiza go.sum com base no código real
RUN go mod tidy

# Compila binário
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -o auth-service .

# =========================
# STAGE 2 — RUNTIME
# =========================
FROM alpine:latest

RUN apk add --no-cache ca-certificates

WORKDIR /app
COPY --from=builder /app/auth-service .

EXPOSE 8001
CMD ["./auth-service"]
