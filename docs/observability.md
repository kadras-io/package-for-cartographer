# Configuring Observability

Monitor and observe the operation of Cartographer using logs and metrics.

## Logs

The log verbosity for the Cartographer Controller can be configured.

```yaml
logging:
  level: info
```

## Metrics

The Cartographer controllers expose Prometheus metrics by default. This package comes pre-configured with the necessary annotations to let Prometheus scrape metrics automatically from the Cartographer controllers.
