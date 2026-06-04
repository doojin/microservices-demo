| service | runtime | environment variables | external dependencies |
|---|---|---|---|
| adservice | Java 21 |- `PORT=9555`<br>- `DISABLE_STATS`<br>- `DISABLE_TRACING` | none |
| cartservice | .NET 10 |- `PORT=8080`<br>- `REDIS_ADDR`<br>- `SPANNER_PROJECT`<br>- `SPANNER_CONNECTION_STRING`<br>- `ALLOYDB_PRIMARY_IP` | optional:<br>- Redis<br>- Spanner<br>- AlloyDB |
| checkoutservice | Go 1.25 |  - `PORT=5050`<br>- `SHIPPING_SERVICE_ADDR`<br>- `PRODUCT_CATALOG_SERVICE_ADDR`<br>- `CART_SERVICE_ADDR`<br>- `CURRENCY_SERVICE_ADDR`<br>- `EMAIL_SERVICE_ADDR`<br>- `PAYMENT_SERVICE_ADDR` | required: <br>- shippingservice <br>- productcatalogservice <br>- cartservice<br>- currencyservice <br>- emailservice <br>- paymentservice |
| currencyservice | Node.js 24 | - `PORT`<br>- `GCLOUD_PROJECT`<br>- `DISABLE_PROFILER`<br>- `ENABLE_TRACING`<br>- `COLLECTOR_SERVICE_ADDR`<br>- `OTEL_SERVICE_NAME` | none |
| emailservice | Python 3.11 | - `PORT=8080`<br>- `ENABLE_TRACING`<br>- `COLLECTOR_SERVICE_ADDR`<br>- `DISABLE_PROFILER`<br>- `GCP_PROJECT_ID` | none |
| frontend | Go 1.25 | - `PORTL`<br>- `PACKAGING_SERVICE_URL`<br>- `ENABLE_SINGLE_SHARED_SESSION`<br>- `BASE_URL`<br>- `LISTEN_ADDR`<br>- `PRODUCT_CATALOG_SERVICE_ADDR`<br>- `CURRENCY_SERVICE_ADDR`<br>- `CART_SERVICE_ADDR`<br>- `RECOMMENDATION_SERVICE_ADDR`<br>- `CHECKOUT_SERVICE_ADDR`<br>- `SHIPPING_SERVICE_ADDR`<br>- `AD_SERVICE_ADDR`<br>- `FRONTEND_MESSAGE`<br>- `CYMBAL_BRANDING`<br>- `ENV_PLATFORM`<br>- `ENABLE_TRACING`<br>- `ENABLE_PROFILER` |required:<br>- productcatalogservice<br>- currencyservice<br>- cartservice<br>- recommendationservice<br >- checkoutservice<br>- shippingservice<br>- adservice |
| loadgenerator | Python 3.11 | none | none |
| paymentservice | Node.js 20 | - `PORT`<br>- `DISABLE_PROFILER`<br>- `ENABLE_TRACING`<br>- `COLLECTOR_SERVICE_ADDR`<br>- `OTEL_SERVICE_NAME` | none |
| productcatalogservice | Go 1.25 | - `PORT=3550`<br>- `ENABLE_TRACING`<br>- `DISABLE_PROFILER`<br>- `EXTRA_LATENCY`<br>- `ALLOYDB_CLUSTER_NAME`<br>- `PROJECT_ID`<br>- `REGION`<br>- `ALLOYDB_INSTANCE_NAME`<br>- `ALLOYDB_DATABASE_NAME`<br>- `ALLOYDB_TABLE_NAME`<br>- `ALLOYDB_SECRET_NAME` | none |
| recommendationservice | Python 3.11 | - `PORT=8080`<br>- `GCP_PROJECT_ID`<br>- `DISABLE_PROFILER`<br>- `ENABLE_TRACING`<br>- `COLLECTOR_SERVICE_ADDR`<br>- `PRODUCT_CATALOG_SERVICE_ADDR` | required:<br>- productcatalogservice |
| shippingservice | Go 1.25 | - `PORT=50051`<br>- `DISABLE_TRACING`<br>- `DISABLE_PROFILER`<br>- `DISABLE_STATS` | none |

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
export DISABLE_PROFILER=1

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
