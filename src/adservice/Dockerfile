FROM amazoncorretto:21 AS builder

WORKDIR /app

COPY gradlew settings.gradle build.gradle .
COPY gradle/ gradle/

RUN chmod +x gradlew

RUN ./gradlew dependencies

COPY . .

RUN chmod +x gradlew && ./gradlew installDist

RUN yum update -y && yum install -y wget

RUN GRPC_HEALTH_PROBE_VERSION=v0.4.48 && \
    wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 && \
    chmod +x /bin/grpc_health_probe

FROM eclipse-temurin:26 AS starter

WORKDIR /app

COPY --from=builder /app/build/install/hipstershop .
COPY --from=builder /bin/grpc_health_probe /bin/grpc_health_probe

RUN adduser user && chown -R user:user /app

ENV PORT=9555

EXPOSE ${PORT}

USER user

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD /bin/grpc_health_probe -addr=localhost:${PORT} || exit 1

CMD ["bin/AdService"]