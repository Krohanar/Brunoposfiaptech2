# =========================
# STAGE 1 — BUILD
# =========================
FROM golang:1.25-alpine AS builder

RUN apk add --no-cache ca-certificates

WORKDIR /app

COPY . .

RUN go mod tidy

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -o auth-service .

# =========================
# STAGE 2 — RUNTIME
# =========================
FROM alpine:3.22

RUN apk add --no-cache ca-certificates

WORKDIR /app
COPY --from=builder /app/auth-service .

EXPOSE 8001
CMD ["./auth-service"]