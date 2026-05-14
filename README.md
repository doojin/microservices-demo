| service | runtime | environment variables | external dependencies |
|---|---|---|---|
| adservice | Java 21 | `PORT=9555`, `DISABLE_STATS`, `DISABLE_TRACING` | none |
| cartservice | .NET 10 | `PORT=5000`, `REDIS_ADDR`, `SPANNER_PROJECT`, `SPANNER_CONNECTION_STRING`, `ALLOYDB_PRIMARY_IP` | optional: Redis/Spanner/AlloyDB |
| checkoutservice | Go 1.25 | `PORT=5050`, `SHIPPING_SERVICE_ADDR`, `PRODUCT_CATALOG_SERVICE_ADDR`, `CART_SERVICE_ADDR`, `CURRENCY_SERVICE_ADDR`, `EMAIL_SERVICE_ADDR`, `PAYMENT_SERVICE_ADDR` | required: shippingservice, productcatalogservice, cartservice, currencyservice, emailservice, paymentservice |
| currencyservice | node 18.19.0 | `PORT`, `GCLOUD_PROJECT`, `DISABLE_PROFILER`, `ENABLE_TRACING`, `COLLECTOR_SERVICE_ADDR`, `OTEL_SERVICE_NAME` | none |

## Ad Service

From `src/adservice/`:

```bash
# install dependencies
./gradlew installDist

# run executable
build/install/hipstershop/bin/AdService
```

## Cart Service

From `src/cartservice/`:

```bash
# build and run the service
dotnet run
```

## Checkout Service

From `src/checkoutservice/`:

```bash
export SHIPPING_SERVICE_ADDR=localhost:50051
export PRODUCT_CATALOG_SERVICE_ADDR=localhost:50052
export CART_SERVICE_ADDR=localhost:50053
export CURRENCY_SERVICE_ADDR=localhost:50054
export EMAIL_SERVICE_ADDR=localhost:50055
export PAYMENT_SERVICE_ADDR=localhost:50056

# build and run the service
go run main.go
```
## Currency Service

From `src\currencyservice`:

```bash
# install dependencies
npm install
```

```bash
export PORT=localhost:3000
export GCLOUD_PROJECT="microservices-demo"

# run executable
node server.js
```