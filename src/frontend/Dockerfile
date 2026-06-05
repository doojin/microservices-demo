FROM golang:1.25 AS builder

WORKDIR /build 

COPY go.mod go.sum ./

RUN go mod download

COPY . . 

RUN CGO_ENABLED=0 go build -o /frontend .

FROM gcr.io/distroless/static-debian12 AS starter

WORKDIR /app 

COPY --from=builder --chown=nonroot:nonroot /frontend /frontend
COPY --from=builder /build/templates /app/templates

ENV PORT=8080

EXPOSE 8080

USER nonroot:nonroot

CMD ["/frontend"]