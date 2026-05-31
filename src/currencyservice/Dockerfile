FROM node:18.19.0-slim

WORKDIR /app

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

RUN useradd nodejs \
    --no-create-home \
    --uid 1001 \
    --shell /usr/sbin/nologin \
    && chown -R nodejs:nodejs /app

USER nodejs

ENV PORT=3000

EXPOSE ${PORT}

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD /bin/grpc_health_probe -addr=localhost:${PORT} || exit 1

CMD ["node", "server.js"]