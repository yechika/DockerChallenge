# ----------- Stage 1: Build (Alpine) -------------
FROM golang:1.21-alpine AS builder

WORKDIR /app

# Install dependensi build (git + CA certificates)
RUN apk add --no-cache git ca-certificates

# Copy mod files dan download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy source code dan build
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o main .

# ----------- Stage 2: Runtime (Scratch) --------------
FROM scratch

# Copy binary dan CA certificates dari builder
COPY --from=builder /app/main /
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

# Jika aplikasi melakukan HTTPS request
ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt

# Jalankan aplikasi
CMD ["/main"]