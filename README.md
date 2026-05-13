# microservices-demo

## adservice

### Description

The Ad service provides advertisements based on context keys. If no context keys are provided, it returns random ads.

### Language and Runtime

- Java
- Java 21

### Environment Variables

#### Required
None for basic local execution.

#### Optional
- `PORT` (default: `9555`)
- `DISABLE_STATS`
- `DISABLE_TRACING`

### External Dependencies

No external service dependencies were identified for basic local execution.

### Install and Run

From `src/adservice`:

```bash
./gradlew installDist
```

Run the generated executable:

#### Linux/macOS/Git Bash

```bash
build/install/hipstershop/bin/AdService
```

#### Windows

```powershell
build\install\hipstershop\bin\AdService.bat

### Validation

Validated locally using Postman gRPC with:
- `demo.proto`
- `AdService/GetAds`

### Troubleshooting

Upgrade Gradle wrapper if needed:

```
./gradlew wrapper --gradle-version <new-version>