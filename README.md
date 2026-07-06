# Grafana-.Net-API-monitoring

# .NET API Monitoring Dashboard

A Grafana dashboard for monitoring **ASP.NET Core APIs** instrumented with **OpenTelemetry** and **Prometheus**.

The dashboard follows the **RED methodology** (Rate, Errors, Duration) while also exposing **.NET runtime** and **Kubernetes** metrics to provide a complete operational view of a service.

## Features

### API Monitoring

* Request rate (Requests/sec)
* Error rate (4xx & 5xx responses)
* Response duration (P50 / P95 / P99)
* Top 10 most requested endpoints
* Error distribution by HTTP status code

<img width="573" height="351" alt="image" src="https://github.com/user-attachments/assets/b0714853-cc88-4205-913a-44253b1d3994" />


### Runtime Monitoring

* Peak GC heap allocation
* CPU usage per pod
* Thread pool queue length
* Managed exceptions
* Top exception types

<img width="754" height="231" alt="image" src="https://github.com/user-attachments/assets/122ce520-2ade-4bdf-9e62-7f1a73b19681" />

<img width="1131" height="335" alt="image" src="https://github.com/user-attachments/assets/b46d9f18-ae2d-47eb-a24c-877e12aaf5c7" />

### Kubernetes Monitoring

* Running pods count
* Pod lifecycle timeline
* Traffic distribution across pods

### Service Health

Quick health indicators based on:

* Traffic
* Error rate
* Latency (P95)

with configurable thresholds.

<img width="1128" height="171" alt="image" src="https://github.com/user-attachments/assets/77b7f593-0ef6-464d-9ca9-525e32cfb3ba" />

---

## Dashboard Structure

```
Dashboard
├── Overview
│   ├── Traffic
│   ├── Error Rate
│   └── P95 Latency
│
├── API
│   ├── Request Rate
│   ├── Error Rate
│   ├── Response Duration
│   ├── Top Routes
│   └── HTTP Status Distribution
│
├── Runtime
│   ├── GC Heap
│   ├── CPU Usage
│   ├── Exceptions
│   └── Thread Pool
│
└── Kubernetes
    ├── Running Pods
    ├── Pod Lifecycle
    └── Traffic Distribution
```

---

## Technologies

* ASP.NET Core
* OpenTelemetry
* Prometheus
* Grafana
* Kubernetes

---

## Metrics Used

### HTTP

* `http_server_request_duration_seconds_bucket`
* `http_server_request_duration_seconds_count`

Used to calculate:

* Request rate
* Error rate
* Response latency
* Endpoint traffic

---

### .NET Runtime

* `dotnet_gc_heap_allocated_bytes_total`
* `dotnet_process_cpu_time_seconds_total`
* `dotnet_thread_pool_queue_length_total`
* `dotnet_exceptions_total`

---

### Kubernetes

* `k8s_pod_name`
* `k8s_pod_start_time`

---

## Dashboard Variables

| Variable                 | Description                            |
| ------------------------ | -------------------------------------- |
| `service_name`           | Service to monitor                     |
| `deployment_environment` | Environment (dev, staging, production) |
| `percentile`             | Latency percentile (P50 / P95 / P99)   |

---

## Health Thresholds

Default thresholds can be adjusted depending on the service.

### Latency

| Status      | Value      |
| ----------- | ---------- |
| 🟢 Good     | < 300 ms   |
| 🟠 Warning  | 300–500 ms |
| 🔴 Critical | > 500 ms   |

### Error Rate

| Status      | Value |
| ----------- | ----- |
| 🟢 Good     | < 2%  |
| 🟠 Warning  | 2–5%  |
| 🔴 Critical | > 5%  |

---

## Observability Approach

The dashboard combines several observability practices:

* **RED metrics** for API performance
* Runtime health monitoring
* Kubernetes operational visibility
* Service-level health indicators
* Endpoint-level performance analysis

This provides both high-level service health and detailed troubleshooting capabilities from a single dashboard.

---

## Requirements

The dashboard expects metrics generated through **OpenTelemetry** and stored in **Prometheus**.

Typical metric labels include:

* `service_name`
* `deployment_environment`
* `http_route`
* `http_request_method`
* `http_response_status_code`
* `k8s_pod_name`

---

## Notes

This dashboard is designed as a reusable template for monitoring ASP.NET Core services deployed on Kubernetes. Thresholds and variables can be customized to match the operational characteristics of individual applications. You can add services in the service_name variable

