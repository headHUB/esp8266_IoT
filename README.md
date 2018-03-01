# What's esp8266_IoT

This is ESP8266 based IoT (Internet of Things) base code.

This base code includes:
 - WiFi Client configuration through Web browser
 - WiFi SoftAP for initial settings (initially it is kicked or you can forcely invoke this mode by GPIO0 Low at boot time.)
 - NTP client when WiFi client is successfully connected, NTP will be starting.
 - Basic demo to print current time periodically.
 - I2C driver support 
 - Sensor Framework
   - BMP180 (Pressure and Temperature sensor)
   - DHT11 (Humidity and Temperature sensor)
 - OTA (Over The Air firmware update. Push Mode button for more than 5 sec, it's enabled for 5 minutes. The threshold are configurable.)
 - SSDP & UPnP

# Instruction

This base code depends on followings. Please install them on your environment.

Thank you so much for the great libraries.

## General steps to include zip library

On Arduino IDE,

1. Sketch
2. Include library
3. Install Zip library
4. Specify the following zip-ed libraries.

## Time library (by PaulStoffregen-san)

```
$ git clone https://github.com/PaulStoffregen/Time.git Time
$ cd Time
$ git archive HEAD --output=../Time.zip
```

## NTP library (by exabugs-san)

```
$ git clone https://github.com/exabugs/sketchLibraryNTP NTP
$ cd NTP
$ git archive HEAD --output=../NTP.zip
```

# Confirmed environment

* ESP WROOM02

## Arduino configuration

* Arduino IDE : 1.6.8

* MCU board : Generic ESP8266 Module
* Flash Mode : DIO (default)
* Flash Frequency : 40MHz (default)
* CPU Frequency : 80MHz (default)
* Flash Size : 512k (64K SPIFFS)
  * Please check your devcice's flash size
    * ESP-07: 512k
    * ESP-12E: 1M
    * WROOM-02/ESP-12F: 1M or 4M
    * etc.
* Debug port : disabled (default)
* Debug level : nothing (default)
* Reset Method : ck (default)
* Upload Speed : 115200 (default)

## Hardware

* GPIO0: High (execution) / Low (flashing)
* GPIO2: High
* GPIO15: Low

* GPIO2/14 : I2C (SDA:GPIO2, SCL:GPIO14)
* GPIO4 : DHT11 Single Wire Data

* GPIO12, GPIO13: Example is used for Servo Ch0/1.

See ```void initializeGPIO(void)``` in ```config.cpp```


# Configuration

See ```config.h``` and ```config.cpp```

```
// --- config
const int MODE_PIN = 0; // GPIO0 {Low: WiFi AP / High: WiFi Client}

// --- config: WIFI
const char* WIFI_CONFIG = "/wifi_config";
const char* WIFIAP_PASSWORD = "1234567890"; // you can see WiFi Mac Address's SSID and this is password for setup SSID/Password from web.

// --- config: NTP
const char* NTP_SERVER = "jp.pool.ntp.org";

// --- config: httpd
int HTTP_SERVER_PORT = 80;
const char* HTML_HEAD = "<html><head><title>hidenorly's ESP8266</title></head><body>";

// --- config: sensor support
#define ENABLE_I2C_BUS
#define ENABLE_SENSOR
#define ENABLE_SENSOR_PRESSURE 1
#define ENABLE_SENSOR_TEMPERATURE 1
#define ENABLE_SENSOR_HUMIDITY 1

// --- config: enable SSDP & UPnP
#define ENABLE_UPNP

// --- config: enable OTA
#define ENABLE_OTA
```

# How to setup

GPIO0 : Low within 1sec after deasserting reset will bring you to set WiFi AP mode.
Please no that uou can configure the pin by ```MODE_PIN``` defined in ```ESP8266_IoT.ino```

And you can configure the device's SSID/Password through your broswer which URL is ```http://192.168.4.1``` from your connected PC to this device. Please note that your PC needs to connect to the device via WiFi which SSID is mac address of your device and the password is ```1234567890``` that you can configure it by ```config.cpp```

