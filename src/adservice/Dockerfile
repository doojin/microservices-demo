FROM amazoncorretto:21 AS builder

WORKDIR /build

COPY gradlew settings.gradle build.gradle ./
COPY gradle/ gradle/

RUN chmod +x gradlew

RUN ./gradlew downloadRepos --no-daemon

COPY . .

RUN chmod +x gradlew && ./gradlew installDist --no-daemon

RUN yum update -y \
    && yum install -y wget \
    && GRPC_HEALTH_PROBE_VERSION=v0.4.48 \
    && wget -qO /bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 \
    && chmod +x /bin/grpc_health_probe \
    && yum remove -y wget \
    && yum clean all

FROM eclipse-temurin:21-jre AS starter

WORKDIR /app

COPY --from=builder /build/build/install/hipstershop .
COPY --from=builder /bin/grpc_health_probe /bin/grpc_health_probe

RUN useradd appuser \
  --no-create-home \
  --uid 1001 \
  --shell /usr/sbin/nologin \
  && chown -R appuser:appuser /app

ENV PORT=9555

EXPOSE ${PORT}

USER appuser

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD /bin/grpc_health_probe -addr=localhost:${PORT} || exit 1

CMD ["bin/AdService"]