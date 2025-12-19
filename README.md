# Thanos, Grafana, Alertmanager & Telegraf Demo

This setup demonstrates a complete observability and alerting pipeline using:
- **Telegraf**: Collects system metrics (CPU, Mem) and pushes them via Prometheus Remote Write to Thanos.
- **Thanos Receive**: Ingests metrics from Telegraf.
- **Thanos Query**: Queries data from Receive.
- **Thanos Ruler**: Evaluates Prometheus-style recording/alerting rules against Thanos Query and forwards alerts to Alertmanager.
- **Grafana**: Visualizes metrics, manages alerts (Unified Alerting), and renders panel screenshots.
- **Alertmanager**: Receives alerts from Grafana and manages notifications.
- **Mailpit**: Captures and displays email notifications (SMTP) from Alertmanager.
- **Grafana Image Renderer**: Generates PNG screenshots for alert notifications.

## Usage

1. Start the stack:
   ```bash
   docker-compose up -d
   ```

2. Access the Services:
   - **Grafana**: [http://localhost:3000](http://localhost:3000)
     - Login: Auto-logged in as Admin (Anonymous auth enabled).
     - **Dashboards**: Go to Dashboards to see the provisioned "System Metrics" dashboard.
     - **Alerting**: Go to Alerting > Alert rules to see "High Memory Usage Demo".
   - **Mailpit**: [http://localhost:8025](http://localhost:8025)
     - View captured alert emails. Emails should contain an embedded screenshot of the Grafana panel.
   - **Thanos Query**: [http://localhost:9090](http://localhost:9090)
   - **Thanos Ruler**: [http://localhost:10912](http://localhost:10912)
   - **Alertmanager**: [http://localhost:9093](http://localhost:9093)

## Features Demonstrated

*   **Remote Write**: Telegraf sends metrics to Thanos Receive.
*   **Datasource Provisioning**: Grafana is auto-configured with Thanos (Prometheus) and Alertmanager datasources.
*   **Alert Provisioning**: An alert rule is provisioned via file (`config/grafana/alert_rules.yaml`).
*   **Dashboard Provisioning**: A "System Metrics" dashboard is provisioned from JSON (`config/grafana/dashboards/`).
*   **Image Rendering**: Alerts include a rendered screenshot of the associated panel.
*   **External Alertmanager**: Grafana is configured (via API sidecar) to send alerts to the external Alertmanager.
*   **SMTP Notifications**: Alertmanager sends emails to Mailpit.

## Configuration Files

- **Docker Compose**: `docker-compose.yaml` defines all services.
- **Grafana**:
    - `config/grafana/grafana.ini`: General settings (SMTP, Rendering).
    - `config/grafana/datasources.yaml`: Connects to Thanos and Alertmanager.
    - `config/grafana/alert_rules.yaml`: Defines the alert rules.
    - `config/grafana/dashboards.yaml` & `config/grafana/dashboards/`: Dashboard provisioning.
- **Thanos Ruler**:
    - `config/thanos/rules/cpu_alerts.yaml`: Example CPU usage alert evaluated by Thanos Ruler and sent to Alertmanager.
- **Alertmanager**: `config/alertmanager.yaml` defines receivers and routing (email).
- **Telegraf**: `config/telegraf.conf` defines input plugins and remote write output.
