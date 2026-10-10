# PMS7003 Room Air Quality Monitor (ESP32 – Soft AP)

This project uses an ESP32 and a PMS7003 particulate matter sensor to monitor indoor air quality.  
The ESP32 runs in Soft AP mode and hosts a live web dashboard that updates in real time using Server-Sent Events (SSE). No internet connection and no external Arduino libraries are required.

---

## Features

- Soft AP mode (works without Wi-Fi or internet)
- Measurement of PM1.0, PM2.5, and PM10
- Overall AQI calculation based on PM2.5 and PM10 (EPA method)
- One-minute rolling average for stable readings
- Live updates using Server-Sent Events (no page reload)
- Built-in responsive web dashboard
- Accessible from phone, tablet, or computer

---

## Wi-Fi Configuration (Soft AP)

| Parameter | Value |
|---------|-------|
| SSID | PMS7003 |
| Password | 12345678 |
| Dashboard URL | http://192.168.4.1/ |

---

## Hardware Requirements

- ESP32 development board
- PMS7003 air quality sensor
- USB cable
- 5V power supply
- Jumper wires

---

## Wiring Diagram

| PMS7003 Pin | ESP32 Pin |
|------------|-----------|
| VCC | 5V |
| GND | GND |
| TX | GPIO 16 |
| RX | GPIO 17 |
| SET | 3.3V |
| RESET | 3.3V |

Important: The PMS7003 must be powered with 5V.

---

## How It Works

1. The PMS7003 sends particulate matter data over UART at 9600 baud.
2. The ESP32 reads the data and stores it in a circular buffer of 60 samples.
3. A one-minute rolling average is calculated for PM1.0, PM2.5, and PM10.
4. AQI values are calculated using EPA breakpoint interpolation.
5. The ESP32 creates a Wi-Fi access point.
6. A web dashboard is served at http://192.168.4.1/
7. Live sensor updates are pushed to the browser using Server-Sent Events (SSE).

---

## Web Endpoints

| Endpoint | Description |
|--------|------------|
| `/` | Live web dashboard |
| `/events` | Server-Sent Events (live data stream) |
| `/data` | JSON snapshot of current readings |

Example response from `/data`:

```json
{
  "pm1_avg": 12.3,
  "pm25_avg": 45.6,
  "pm10_avg": 78.1,
  "aqi_pm25": 121,
  "aqi_pm10": 68,
  "aqi_overall": 121,
  "dominant": "PM2.5",
  "last_update_ms": 123456
}
```

## How to Use

1. Open Arduino IDE.

2. Select the correct ESP32 board and port.

3. Upload the sketch to the ESP32.

4. Power the ESP32.

5. Connect your phone or computer to the Wi-Fi network named PMS7003.

6. Open a browser and go to http://192.168.4.1/

7. View live room air quality data.