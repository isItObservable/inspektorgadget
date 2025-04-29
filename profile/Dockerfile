FROM golang:1.22-alpine3.19 as builder

RUN apk --update add ca-certificates git

WORKDIR /build
COPY otelcol-epbf-profiler.yaml .

ENV GO111MODULE=on
ENV CGO_ENABLED=0

# install ocb and build
RUN go install go.opentelemetry.io/collector/cmd/builder@v0.123.0
RUN builder --config=./otelcol-epbf-profiler.yaml

FROM scratch

ARG USER_UID=10001

USER ${USER_UID}

COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=builder /build/observable/observable-otel-collector /otelcol



ENTRYPOINT ["/otelcol"]
CMD ["--config", "/etc/otelcol/config.yaml"]