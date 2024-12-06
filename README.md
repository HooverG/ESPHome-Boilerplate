# ESPHome Configuration

The configuration assigns a name and friendly name for the device.  
It sets the ESP8266 board type (e.g., `nodemcuv2`, `d1_mini`, etc.).

### ESPHome Configuration
```yaml
esphome:
  name: ${name}
  friendly_name: ${friendly_name}

esp8266:
  board: ${board}
```

### WiFi Setup
The device is configured to connect to a WiFi network using the provided credentials. In case of a failure, a fallback access point is created.
```yaml
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  ap:
    ssid: ${friendly_name} Fallback AP
    password: !secret project_name_ap_password
```
- **ssid**: Your WiFi network's SSID.
- **password**: The password for your WiFi network.
- **Fallback AP**: If the device can't connect to the WiFi network, it will create a temporary access point for configuration.

### OTA Updates
To enable Over-the-Air updates, this configuration includes a password-protected OTA platform:
```yaml
ota:
  - platform: esphome
    password: !secret project_name_ota_password
```
This allows you to upload firmware updates without needing to connect the device via USB.

### API Encryption
API communication between the device and Home Assistant (or other services) is encrypted using a secret key.
```yaml
api:
  encryption:
    key: !secret project_name_api_encryption_key
```

### Logging and Debugging
The `logger` component enables logging to the console, which helps with debugging and troubleshooting.
```yaml
logger:
```

### Text Sensors
This configuration includes two text sensors:

- **ESPHome Version**: Displays the current version of ESPHome.
- **WiFi Info**: Shows the IP and MAC addresses of the device.
```yaml
text_sensor:
  - platform: version
    name: ESPHome Version - ${friendly_name}

  - platform: wifi_info
    ip_address: 
      name: IP - ${friendly_name}
    mac_address:
      name: MAC - ${friendly_name}

```
These sensors provide useful information about the device’s status.

### Restart Button
This configuration adds a restart button to restart the device from Home Assistant or other systems.

```yaml
button:
  - platform: restart
    name: Restart ${friendly_name}
```
This allows you to trigger a restart of the device remotely.

### Customizing the Configuration
To customize the configuration:

- Replace `project_name` with your desired device's name.
- Update the `friendly_name` to a more readable name for your device.
- Choose the appropriate board type for your ESP8266 hardware.

### Secrets File
Ensure the `secrets.yaml` file contains the following entries:
```yaml
wifi_ssid: "YourWiFiSSID"
wifi_password: "YourWiFiPassword"
project_name_ap_password: "FallbackAPPassword"
project_name_api_encryption_key: "EncryptionKey"
project_name_ota_password: "OTAPassword"
```

This keeps sensitive information like WiFi credentials and passwords secure.

### Flashing the Device
1. Install ESPHome on your computer or use the web-based ESPHome dashboard.
2. Connect your ESP8266 device to your computer via USB.
3. Flash the device with this configuration file.
4. Once flashed, the device will attempt to connect to your WiFi network. If it fails, it will start the fallback access point for configuration.

### License
This project is licensed under the MIT License - see the LICENSE file for details.