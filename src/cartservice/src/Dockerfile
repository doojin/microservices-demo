FROM mcr.microsoft.com/dotnet/sdk:10.0 AS builder

WORKDIR /build

COPY appsettings.json cartservice.csproj ./

RUN dotnet restore

COPY . . 

RUN dotnet publish -c Release -o /build/publish

RUN apt-get update -y \
    && apt-get install -y --no-install-recommends wget \
    && GRPC_HEALTH_PROBE_VERSION=v0.4.48 \
    && wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 \
    && chmod +x /bin/grpc_health_probe \
    && apt-get remove -y wget \
    && rm -rf /var/lib/apt/lists/*

FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS starter

WORKDIR /app

COPY --from=builder /build/publish .
COPY --from=builder /bin/grpc_health_probe /bin/grpc_health_probe

RUN useradd appuser \
  --no-create-home \
  --uid 1001 \
  --shell /usr/sbin/nologin \
  && chown -R appuser:appuser /app

ENV PORT=8080

EXPOSE ${PORT}

USER appuser

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD /bin/grpc_health_probe -addr=localhost:${PORT} || exit 1

ENTRYPOINT ["dotnet", "cartservice.dll"]