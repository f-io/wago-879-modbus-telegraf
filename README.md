# Telegraf Configuration for WAGO 879 with Modbus

This repository contains a sample configuration file for Telegraf, designed to collect metrics from a WAGO 879 device using the Modbus protocol. It also includes settings for forwarding these metrics to an InfluxDB instance and provides a Docker setup for running Telegraf as a container.

  <tbody>
    <tr>
      <th rowspan="4"><img src="/images/wago879.png" alt="wago 879" style="width:667px;height:auto;"></th>
    </tr>
  </tbody>

## Features
- **Modbus Input**:
  - Collects data from a WAGO 879 device via Modbus/TCP.
  - Predefined metrics for voltage, current, power, frequency, and energy.
- **InfluxDB Output**:
  - Sends collected metrics to an InfluxDB instance.
- **Docker Support**:
  - Includes a Docker Compose configuration for running Telegraf as a container.

## Usage
### Standalone Setup
1. Update the placeholders with the appropriate values for your environment.
2. Ensure Telegraf is installed on your system.
3. Place the configuration file in the appropriate Telegraf configuration directory (e.g., `/etc/telegraf/telegraf.d/`).
4. Start the Telegraf service:
   ```bash
   sudo systemctl start telegraf
   ```

### Docker Setup
1. Update the Telegraf configuration file (`telegraf.conf`) with your settings.
2. Use the provided `docker-compose.yml` file to run Telegraf as a container:
   ```bash
   docker-compose up -d
   ```

## Configuration
### Telegraf Agent Settings
```toml
[agent]
  interval = "1s"
  round_interval = true
  metric_batch_size = 1000
  metric_buffer_limit = 10000
  collection_jitter = "0s"
  flush_interval = "10s"
  flush_jitter = "0s"
  precision = ""
  hostname = "YOUR_HOSTNAME_OR_IP"
  omit_hostname = false
```

### InfluxDB Output
```toml
[[outputs.influxdb_v2]]
  urls = ["http://YOUR_INFLUX_HOSTNAME_OR_IP:8086"]
  token = "YOUR_TOKEN"
  organization = "YOUR.ORG"
  bucket = "YOUR_BUCKET"
  timeout = "5s"
```

### Modbus Input Configuration
```toml
[[inputs.modbus]]
  name = "WAGO 879"
  slave_id = 1
  timeout = "5s"
  controller = "tcp://MODBUS_CONTROLLER_IP:502"
  debug_connection = false
  configuration_type = "register"

 holding_registers = [
    { measurement = "modbus", name = "power SUM",               byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1000.0, address = [20498,20499]},
    { measurement = "modbus", name = "power L1",                byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1000.0, address = [20500,20501]},
    { measurement = "modbus", name = "power L2",                byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1000.0, address = [20502,20503]},
    { measurement = "modbus", name = "power L3",                byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1000.0, address = [20504,20505]},
    { measurement = "modbus", name = "voltage L1",              byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20482,20483]},
    { measurement = "modbus", name = "voltage L2",              byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20484,20485]},
    { measurement = "modbus", name = "voltage L3",              byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20486,20487]},
    { measurement = "modbus", name = "frequency",               byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20488,20489]},
    { measurement = "modbus", name = "current L1",              byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20492,20493]},
    { measurement = "modbus", name = "current L2",              byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20494,20495]},
    { measurement = "modbus", name = "current L3",              byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20496,20497]},
    { measurement = "modbus", name = "power_factor SUM",        byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20522,20523]},
    { measurement = "modbus", name = "power_factor L1",         byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20524,20525]},
    { measurement = "modbus", name = "power_factor L2",         byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20526,20527]},
    { measurement = "modbus", name = "power_factor L3",         byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [20528,20529]},
    { measurement = "modbus", name = "energy SUM",              byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [24576,24577]},
    { measurement = "modbus", name = "energy L1",               byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [24582,24583]},
    { measurement = "modbus", name = "energy L2",               byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [24584,24585]},
    { measurement = "modbus", name = "energy L3",               byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [24586,24587]},
    { measurement = "modbus", name = "energy delivered SUM",    byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [24600,24601]},
    { measurement = "modbus", name = "energy drawn SUM",        byte_order = "ABCD", data_type = "FLOAT32-IEEE", scale=1.0, address = [24588,24589]},
```

### Docker Compose Configuration
```yaml
services:
  telegraf:
    container_name: telegraf
    image: telegraf:latest
    restart: unless-stopped
    volumes:
      - /opt/docks/telegraf/telegraf.conf:/etc/telegraf/telegraf.conf:ro
    ports:
      - 8125:8125
```

This setup ensures a flexible deployment of Telegraf for collecting and forwarding Modbus data from the WAGO 879 device.

