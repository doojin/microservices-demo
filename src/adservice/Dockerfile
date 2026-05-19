FROM amazoncorretto:21 AS builder

WORKDIR /app

COPY . .   

RUN yum update -y && yum install -y wget

RUN chmod +x gradlew && ./gradlew installDist

RUN GRPC_HEALTH_PROBE_VERSION=v0.4.48 && \
    wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 && \
    chmod +x /bin/grpc_health_probe

FROM amazoncorretto:21-alpine3.23 AS starter

WORKDIR /app

COPY --from=builder /app/build/install/hipstershop .
COPY --from=builder /bin/grpc_health_probe /bin/grpc_health_probe

RUN adduser -D user && chown -R user:user /app

EXPOSE 9555

USER user

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD /bin/grpc_health_probe -addr=localhost:9555 || exit 1

CMD ["bin/AdService"]