FROM golang:1.25 AS builder

WORKDIR /build

COPY go.mod go.sum ./

RUN go mod download

COPY . . 

RUN CGO_ENABLED=0 go build -o /checkout-svc

RUN apt-get update -y \
    && apt-get install -y wget \
    && GRPC_HEALTH_PROBE_VERSION=v0.4.48 \
    && wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 \
    && chmod +x /bin/grpc_health_probe

FROM gcr.io/distroless/static-debian12 AS starter

WORKDIR /app 

COPY --from=builder --chown=nonroot:nonroot /checkout-svc /checkout-svc
COPY --from=builder /bin/grpc_health_probe /bin/grpc_health_probe

ENV PORT=5050

EXPOSE 5050

USER nonroot:nonroot

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD ["/bin/grpc_health_probe", "-addr=localhost:5050"]

CMD ["/checkout-svc"]