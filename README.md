| service | runtime | environment variables | external dependencies |
|---|---|---|---|
| adservice | Java 21 | `PORT=9555`, `DISABLE_STATS`, `DISABLE_TRACING` | none |
| cartservice | .NET 10 | `PORT=5000`, `REDIS_ADDR`, `SPANNER_PROJECT`, `SPANNER_CONNECTION_STRING`, `ALLOYDB_PRIMARY_IP` | optional: Redis/Spanner/AlloyDB |

## Ad Service

```bash
# install dependencies
./gradlew installDist

# run executable
build/install/hipstershop/bin/AdService
```

## Cart Service

From `src/cartservice/`:

```
# build and run the service
dotnet run
```