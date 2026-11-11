FROM golang:alpine AS builder
ARG VERSION="0.0.1"
RUN apk add --no-cache git
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o "./build/backend-${VERSION}/app" ./cmd/api/main.go ./cmd/api/router.go

FROM golang:alpine AS runtime
ARG VERSION="0.0.1"
WORKDIR /opt/sandbox/backend

RUN addgroup -S sandbox-backend && adduser -S sandbox-backend -G sandbox-backend

COPY --from=builder /app/build/backend-${VERSION}/app ./app

RUN chown -R sandbox-backend:sandbox-backend /opt/sandbox/backend

USER sandbox-backend

EXPOSE 8081
CMD ["./app"]