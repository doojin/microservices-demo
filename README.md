| service | runtime | environment variables | external dependencies |
|---|---|---|---|
| adservice | Java 21 | `PORT=9555`, `DISABLE_STATS`, `DISABLE_TRACING` | none |
| cartservice | .NET 10 | `PORT=5000`, `REDIS_ADDR`, `SPANNER_PROJECT`, `SPANNER_CONNECTION_STRING`, `ALLOYDB_PRIMARY_IP` | optional: Redis/Spanner/AlloyDB |
| checkoutservice | Go 1.25 | `PORT=5050`, `SHIPPING_SERVICE_ADDR`, `PRODUCT_CATALOG_SERVICE_ADDR`, `CART_SERVICE_ADDR`, `CURRENCY_SERVICE_ADDR`, `EMAIL_SERVICE_ADDR`, `PAYMENT_SERVICE_ADDR` | required: shippingservice, productcatalogservice, cartservice, currencyservice, emailservice, paymentservice |
| currencyservice | Node.js 18.19.0 | `PORT`, `GCLOUD_PROJECT`, `DISABLE_PROFILER`, `ENABLE_TRACING`, `COLLECTOR_SERVICE_ADDR`, `OTEL_SERVICE_NAME` | none |
| emailservice | Python 3.11 | `PORT=8080`, `ENABLE_TRACING`, `COLLECTOR_SERVICE_ADDR`, `DISABLE_PROFILER`, `GCP_PROJECT_ID` | none |
| frontend | Go 1.25 | `BASE_URL`, `PACKAGING_SERVICE_URL`, `ENABLE_SINGLE_SHARED_SESSION`, `PORT`, `LISTEN_ADDR`, `PRODUCT_CATALOG_SERVICE_ADDR`, `CURRENCY_SERVICE_ADDR`, `CART_SERVICE_ADDR`, `RECOMMENDATION_SERVICE_ADDR`, `CHECKOUT_SERVICE_ADDR`, `SHIPPING_SERVICE_ADDR`, `AD_SERVICE_ADDR`, `FRONTEND_MESSAGE`, `CYMBAL_BRANDING`, `ENV_PLATFORM`, `ENABLE_TRACING`, `ENABLE_PROFILER` | required: productcatalogservice, currencyservice, cartservice, recommendationservice, checkoutservice, shippingservice, adservice |
| loadgenerator | Python 3.11 | none | none |
| paymentservice | Node.js 20 | `PORT`, `DISABLE_PROFILER`, `ENABLE_TRACING`, `COLLECTOR_SERVICE_ADDR`, `OTEL_SERVICE_NAME` | none |
| productcatalogservice | Go 1.25 | `PORT=3550`, `ENABLE_TRACING`, `DISABLE_PROFILER`, `EXTRA_LATENCY`, `ALLOYDB_CLUSTER_NAME`, `PROJECT_ID`, `REGION`, `ALLOYDB_INSTANCE_NAME`, `ALLOYDB_DATABASE_NAME`, `ALLOYDB_TABLE_NAME`, `ALLOYDB_SECRET_NAME` | none |
| recommendationservice | Python 3.11 | `PORT=8080`, `GCP_PROJECT_ID`, `DISABLE_PROFILER`, `ENABLE_TRACING`, `COLLECTOR_SERVICE_ADDR`, `PRODUCT_CATALOG_SERVICE_ADDR` | required: productcatalogservice |
| shippingservice | Go 1.25 | `PORT=50051`, `DISABLE_TRACING`, `DISABLE_PROFILER`, `DISABLE_STATS` | none |

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
# required service dependencies
export SHIPPING_SERVICE_ADDR=localhost:50051
export PRODUCT_CATALOG_SERVICE_ADDR=localhost:50052
export CART_SERVICE_ADDR=localhost:50053
export CURRENCY_SERVICE_ADDR=localhost:50054
export EMAIL_SERVICE_ADDR=localhost:50055
export PAYMENT_SERVICE_ADDR=localhost:50056

# run the service
go run main.go
```
## Currency Service

From `src/currencyservice/`:

```bash
# install dependencies
npm install

# required environment variables
export PORT=3000
export GCLOUD_PROJECT="microservices-demo"

# run executable
node server.js
```
## Email Service

From `src/emailservice/`:

```bash
# create virtual environment
python -m venv .venv

# activate virtual environment
source .venv/bin/activate

# install dependencies
pip install -r requirements.txt

# run service
python email_server.py
```
## Frontend

From `src/frontend/`:

```bash
# required service dependencies
export PRODUCT_CATALOG_SERVICE_ADDR=localhost:50051
export CURRENCY_SERVICE_ADDR=localhost:50052
export CART_SERVICE_ADDR=localhost:50053
export RECOMMENDATION_SERVICE_ADDR=localhost:50054
export CHECKOUT_SERVICE_ADDR=localhost:50055
export SHIPPING_SERVICE_ADDR=localhost:50056
export AD_SERVICE_ADDR=localhost:50057

# run the service
go run .
```
## Load Generator

From `src/loadgenerator/`:

```bash
# create virtual environment
python -m venv .venv

# activate virtual environment
source .venv/bin/activate

# install dependencies
pip install -r requirements.txt

# run the service
python locustfile.py

```
 This component is not a standalone API or frontend service. Running the Python file directly will not expose an HTTP endpoint or UI

## Payment Service

From `src/paymentservice/`:

```bash
# install dependencies
npm install

# required environment variables
export PORT=3001

# run executable
node .
```

## Product Catalog Service

From `src/productcatalogservice/`:

```bash
# run the service
go run .
```

## Recommendation Service

From `src/recommendationservice/`:

```bash
# create virtual environment
python -m venv .venv

# activate virtual environment
source .venv/bin/activate

# install dependencies
pip install -r requirements.txt

# required service dependencies
export PRODUCT_CATALOG_SERVICE_ADDR=localhost:50051

# run the service
python recommendation_server.py
```
## Shipping Service

From `src/shippingservice/`:

```bash
# run the service
go run .
```