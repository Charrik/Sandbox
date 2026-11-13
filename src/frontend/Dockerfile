FROM node:16.20.0-alpine3.18 AS builder
ARG VERSION="0.0.1"
WORKDIR /app
COPY . .
RUN npm install
ENV NODE_ENV=production
RUN npm run build -- --dest "/app/build/frontend-${VERSION}/site"

FROM node:16.20.0-alpine3.18 AS runtime
ARG VERSION="0.0.1"
WORKDIR /opt/sandbox/frontend

RUN addgroup -S sandbox-frontend && adduser -S sandbox-frontend -G sandbox-frontend

RUN npm install -g http-server

COPY --from=builder /app/build/frontend-${VERSION}/site ./site

RUN chown -R sandbox-frontend:sandbox-frontend /opt/sandbox/frontend

USER sandbox-frontend

EXPOSE 8888
CMD ["http-server", "./site", "-p", "8888", "--proxy", "http://backend:8081"]