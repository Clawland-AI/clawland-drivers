# clawland-drivers

Unified sensor driver library for the Clawland edge AI ecosystem.

---

## Overview

`clawland-drivers` provides plug-and-play Python drivers for sensors and actuators used by Claw agents. Each driver follows a consistent interface so picclaw/nanoclaw can read any sensor with a single `exec` call.

## Supported Sensors

### Temperature & Humidity

| Sensor | Interface | Price | Driver |
|--------|-----------|-------|--------|
| DHT22 / AM2302 | GPIO | $3 | `drivers/temperature/dht22.py` |
| SHT40 | I2C | $2 | `drivers/temperature/sht40.py` |
| DS18B20 | 1-Wire | $1 | `drivers/temperature/ds18b20.py` |
| BME280 | I2C/SPI | $3 | `drivers/temperature/bme280.py` |

### Water Quality

| Sensor | Interface | Price | Driver |
|--------|-----------|-------|--------|
| Dissolved Oxygen | RS485 | $25 | `drivers/water/dissolved_oxygen.py` |
| pH Sensor | Analog/I2C | $15 | `drivers/water/ph.py` |
| Turbidity | Analog | $8 | `drivers/water/turbidity.py` |

### Motion & Vibration

| Sensor | Interface | Price | Driver |
|--------|-----------|-------|--------|
| HC-SR501 PIR | GPIO | $1 | `drivers/motion/pir.py` |
| SW-420 Vibration | GPIO | $0.5 | `drivers/vibration/sw420.py` |
| ADXL345 Accelerometer | I2C | $2 | `drivers/vibration/adxl345.py` |

### Environmental

| Sensor | Interface | Price | Driver |
|--------|-----------|-------|--------|
| MQ-2 Smoke/Gas | Analog | $2 | `drivers/gas/mq2.py` |
| BH1750 Light | I2C | $1 | `drivers/light/bh1750.py` |
| Soil Moisture | Analog | $1 | `drivers/soil/moisture.py` |

### Actuators

| Device | Interface | Price | Driver |
|--------|-----------|-------|--------|
| Relay Module | GPIO | $2 | `drivers/actuators/relay.py` |
| Servo Motor | PWM | $3 | `drivers/actuators/servo.py` |
| Buzzer | GPIO | $0.5 | `drivers/actuators/buzzer.py` |

## Quick Start

```bash
# Install
pip install clawland-drivers

# Read temperature from DHT22 on GPIO pin 4
python -m clawland_drivers.temperature.dht22 --pin 4
# Output: {"temperature": 25.3, "humidity": 62.1, "timestamp": "2026-02-16T12:00:00Z"}

# Control relay on GPIO pin 17
python -m clawland_drivers.actuators.relay --pin 17 --action on
# Output: {"pin": 17, "state": "on", "timestamp": "2026-02-16T12:00:01Z"}
```

## Unified Interface

Every driver outputs JSON to stdout and follows the same pattern:

```python
# Read sensor
python -m clawland_drivers.<category>.<sensor> --pin <PIN> [--bus <BUS>]

# Output format (always JSON):
{
  "sensor": "dht22",
  "values": {"temperature": 25.3, "humidity": 62.1},
  "unit": {"temperature": "°C", "humidity": "%"},
  "timestamp": "2026-02-16T12:00:00Z",
  "status": "ok"
}
```

This makes it trivial for picclaw to read any sensor via `exec`:

```
exec: python -m clawland_drivers.temperature.dht22 --pin 4
```

## Directory Structure

```
clawland-drivers/
├── clawland_drivers/
│   ├── __init__.py
│   ├── base.py                # Base driver interface
│   ├── temperature/
│   │   ├── dht22.py
│   │   ├── sht40.py
│   │   ├── ds18b20.py
│   │   └── bme280.py
│   ├── water/
│   │   ├── dissolved_oxygen.py
│   │   ├── ph.py
│   │   └── turbidity.py
│   ├── motion/
│   │   └── pir.py
│   ├── vibration/
│   │   ├── sw420.py
│   │   └── adxl345.py
│   ├── gas/
│   │   └── mq2.py
│   ├── light/
│   │   └── bh1750.py
│   ├── soil/
│   │   └── moisture.py
│   └── actuators/
│       ├── relay.py
│       ├── servo.py
│       └── buzzer.py
├── tests/
├── pyproject.toml
├── LICENSE                    # Apache 2.0
└── README.md
```

## Contributing

New sensor drivers are highly valued — **12 base points per driver** in the contribution scoring system. See [CONTRIBUTING.md](https://github.com/Clawland-AI/.github/blob/main/CONTRIBUTING.md).

## License

Apache 2.0 — See [LICENSE](LICENSE)
