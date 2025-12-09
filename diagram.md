# 📊 Architecture Diagram — GenLayer Monitoring Stack

```mermaid
flowchart TB

    subgraph Node["GenLayer Validator Node"]
        A["/metrics (Collectors via config.yaml)"]
        B["Logs (File-based)"]
    end

    subgraph Alloy["Grafana Alloy"]
        C["Scrape Metrics"]
        D["Read Logs"]
        E["Forward Metrics (remote_write)"]
        F["Forward Logs (Loki Push API)"]
    end

    Node --> A --> C
    Node --> B --> D
    C --> E
    D --> F

    subgraph Backend["Monitoring Backend"]
        subgraph PROM["Prometheus"]
            G["Metrics Storage"]
        end
        subgraph LOKI["Loki"]
            H["Log Storage"]
        end
        subgraph GRAF["Grafana"]
            I["Dashboards"]
            J["Explore (Logs)"]
        end
    end

    E --> G
    F --> H
    G --> I
    H --> J

    subgraph NGINX["NGINX Reverse Proxy"]
        K["/grafana"]
        L["/prometheus"]
        M["/loki"]
    end

    I --> K
    G --> L
    H --> M
