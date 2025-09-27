# Scaphandre + Prometheus + Grafana Example
This example demonstrates how to deploy a monitoring stack using Scaphandre for power consumption metrics collection, Prometheus for metrics storage, and Grafana for visualization. The stack is orchestrated using Docker Compose.

## Understanding our components:

In order for us to get from metrics collection to visualization, we need a few components working together. Based on the blueprint, we can observe that the base components needed for our goal are comprised of the actual collector, a storage solution, and a visualization tool.

### Our use-case components:
- **Scaphandre (Collector)**: A power consumption monitoring tool that collects metrics from various hardware components and exposes them via an HTTP endpoint.
- **Prometheus (Storage)**: An open-source systems monitoring and alerting toolkit that scrapes and stores metrics data from configured targets at specified intervals.
- **Grafana (Visualisation)**: An open-source platform for monitoring and observability that allows you to visualize metrics data through customizable dashboards.

## Setup Process Flow:

1. Copy the `.env.example` file to `.env` and fill in the required environment variables, such as database credentials and Grafana admin credentials.

2. Copy the `prometheus.yml` file from the `storage/prometheus` directory to the current example's directory. 

3. Copy the Scaphandre target configuration (located in `collectors/scaphandre/scrape-config.yml`) inside the current example's `prometheus.yml` configuration.

*This file must contain the necessary scrape configuration for Prometheus to collect metrics from Scaphandre. This way, Prometheus knows where to find the data that is offered by the collector*

4. Create a `deployment.yml` file in the current example's directory and copy the contents of each service's deployment file into it. The deployment files are located in the following paths:
   - Scaphandre: `collectors/scaphandre/deployment.yml`
   - Prometheus: `storage/prometheus/deployment.yml`
   - Grafana: `visualization/grafana/deployment.yml`

*Make sure that all of the contents are copied but most importantly that all service specifications fall under a single `services` action. Otherwise, the services will not be deployed propperly.*

5. (Optional) Grafana has an interface that allows you to add Prometheus as a data source. However, to automate the process, you can use the provisioning feature of Grafana. The configuration file for provisioning Prometheus as a data source is located at `visualization/grafana/provisioning/datasources/datasource.yml`. This file is already set up to connect to the Prometheus instance defined in the stack. Copy this file to the current example's directory, maintaining the same folder structure (i.e., create the necessary subdirectories).

6. Now you can deploy the services using the generated deployment file using the following command:

```bash
docker compose --env-file .env -f examples/scaphandre-prometheus-grafana/deployment.yml up -d
```

7. Once the services are up and running, you can access the Grafana web interface by navigating to `http://localhost:13000` in your web browser. Log in using the admin credentials specified in the `.env` file.
