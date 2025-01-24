# Telegraf Configuration for WAGO 879 with Modbus

This repository contains a sample configuration file for Telegraf, designed to collect metrics from a WAGO 879 device using the Modbus protocol. It also includes settings for forwarding these metrics to an InfluxDB instance.

  <tbody>
    <tr>
      <th rowspan="4"><img src="/images/wago879.png" alt="wago 879" style="width:667px;height:auto;"></th>
    </tr>
  </tbody>

## Features
- **Modbus Input**:
  - Collects data from a WAGO 879 device via Modbus/TCP.
  - Predefined metrics for voltage, current, power, frequency, and energy.

## Usage
1. Update the placeholders (`MODBUS_CONTROLLER_IP`, `YOUR_TOKEN`, `YOUR.ORG`, `YOUR_BUCKET`) with the appropriate values for your environment.
2. Ensure Telegraf is installed on your system.
3. Place the configuration file in the appropriate Telegraf configuration directory (e.g., `/etc/telegraf/telegraf.d/`).
4. Start the Telegraf service:
   ```bash
   sudo systemctl start telegraf
