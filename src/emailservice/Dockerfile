FROM python:3.11-slim AS builder

WORKDIR /build 

COPY requirements.txt .

RUN pip install --no-cache-dir \
  --target=/python \
  -r requirements.txt

RUN apt-get update && apt-get install -y --no-install-recommends wget \
    && GRPC_HEALTH_PROBE_VERSION=v0.4.48 \
    && wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 \
    && chmod +x /bin/grpc_health_probe 

COPY . .

FROM gcr.io/distroless/python3-debian12 AS starter

WORKDIR /app
 
COPY --from=builder --chown=nonroot:nonroot /build .
COPY --from=builder /bin/grpc_health_probe /bin/grpc_health_probe
COPY --from=builder /python /python

ENV PYTHONPATH=/python
ENV PORT=8080

EXPOSE 8080

USER nonroot:nonroot

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD ["/bin/grpc_health_probe", "-addr=localhost:8080"]

CMD ["email_server.py"]