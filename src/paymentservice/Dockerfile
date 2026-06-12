FROM node:24-slim AS builder

WORKDIR /build

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    make \
    g++ \
    wget \
    ca-certificates

COPY package*.json ./

RUN npm ci --omit=dev \
    && npm cache clean --force

COPY . .

RUN GRPC_HEALTH_PROBE_VERSION=v0.4.48 \
    && wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 \
    && chmod +x /bin/grpc_health_probe

FROM gcr.io/distroless/nodejs24-debian12 AS starter

WORKDIR /app

COPY --from=builder --chown=nonroot:nonroot /build .
COPY --from=builder /bin/grpc_health_probe /bin/grpc_health_probe

ENV PORT=3001

EXPOSE 3001

USER nonroot:nonroot

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD ["/bin/grpc_health_probe", "-addr=localhost:3001"]

CMD ["index.js"]