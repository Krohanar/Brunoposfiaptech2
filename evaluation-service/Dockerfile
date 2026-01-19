# =========================
# STAGE 1 — BUILD
# =========================
FROM golang:1.21-alpine AS builder

RUN apk add --no-cache ca-certificates

WORKDIR /app

# Copia metadados do módulo primeiro (cache)
COPY go.mod go.sum ./

# Baixa deps do que já está travado
RUN go mod download

# Copia o restante do código
COPY . .

# Garante go.sum consistente com os imports reais
RUN go mod tidy

# Compila binário
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -o evaluation-service .

# =========================
# STAGE 2 — RUNTIME
# =========================
FROM alpine:latest
RUN apk add --no-cache ca-certificates

WORKDIR /app
COPY --from=builder /app/evaluation-service .

EXPOSE 8004
CMD ["./evaluation-service"]
