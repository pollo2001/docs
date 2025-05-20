---
title: ESP32-S2 Wi-Fi LED Toggle Tutorial
date: 2025-05-19
authors:
  - name: Genaro Salazar Ruiz
---

![ESP32 DevKit Board](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/ESP32-DevKitC.jpg/800px-ESP32-DevKitC.jpg)

## Introduction

This tutorial teaches you how to build a simple SoftAP-hosted webpage that lets you control two onboard LEDs on the ESP32-S2 DevKit. The board creates its own Wi-Fi network, and you can control the hardware through your phone or browser—no internet required.

### Learning Objectives

- Create a Soft Access Point with the ESP32
- Serve an HTML page from the ESP32 itself
- Toggle GPIO outputs through basic web buttons

### Background Information

The ESP32-S2 is a Wi-Fi-enabled microcontroller that supports hosting its own network using SoftAP mode. Using this, it can serve HTML pages and respond to URL requests. The project focuses on basic GPIO control through the web—a foundational use case for embedded web servers in IoT.

## Getting Started

### Required Downloads and Installations

- [Arduino IDE](https://www.arduino.cc/en/software)
- ESP32 Board Definitions  
  Board Manager URL:

### Required Components

| Component Name   | Quantity |
|------------------|----------|
| ESP32-S2 DevKit  | 1        |
| USB-C Cable      | 1        |

### Required Tools and Equipment

- Computer with Arduino IDE installed
- Web browser (Chrome, Firefox, etc.)

## Part 01: Wi-Fi LED Control via Web Page

### Introduction

In this section, you'll configure the ESP32-S2 as a Wi-Fi access point and host an HTML page to control two GPIOs connected to LEDs.

### Objective

- Set up SoftAP mode
- Control GPIO 17 and GPIO 18 from a browser

### Background Information

You'll use the ESP32 libraries `<WiFi.h>` and `<WebServer.h>` to set up a server. The client browser will connect to the ESP32's network and send GET requests to specific routes, triggering the LEDs.

### Components

- ESP32-S2 DevKit (LEDs on GPIO 17 and 18)

### Instructional

```cpp
#include <WiFi.h>
#include <WebServer.h>

#define LED1_PIN 17
#define LED2_PIN 18

WebServer server(80);

void handleRoot() {
server.send(200, "text/html", "<html><body>\
  <h1>LED Control</h1>\
  <a href='/led1/on'>LED 1 ON</a><br>\
  <a href='/led1/off'>LED 1 OFF</a><br>\
  <a href='/led2/on'>LED 2 ON</a><br>\
  <a href='/led2/off'>LED 2 OFF</a>\
  </body></html>");
}

void handleLED1On()  { digitalWrite(LED1_PIN, HIGH); server.send(200, "text/plain", "LED1 ON"); }
void handleLED1Off() { digitalWrite(LED1_PIN, LOW);  server.send(200, "text/plain", "LED1 OFF"); }
void handleLED2On()  { digitalWrite(LED2_PIN, HIGH); server.send(200, "text/plain", "LED2 ON"); }
void handleLED2Off() { digitalWrite(LED2_PIN, LOW);  server.send(200, "text/plain", "LED2 OFF"); }

void setup() {
pinMode(LED1_PIN, OUTPUT);
pinMode(LED2_PIN, OUTPUT);
WiFi.softAP("ESP32_LED_AP", "genny123");
server.on("/", handleRoot);
server.on("/led1/on", handleLED1On);
server.on("/led1/off", handleLED1Off);
server.on("/led2/on", handleLED2On);
server.on("/led2/off", handleLED2Off);
server.begin();
}

void loop() {
server.handleClient();
}

```
## Example

### Introduction

This example shows the complete code that creates an ESP32-based SoftAP, serves a webpage, and allows the user to control two LEDs.

### Example

Connect to the ESP32 Wi-Fi network (`ESP32_LED_AP`) using the password `genny123`, then visit `http://192.168.4.1` in your browser. You’ll see LED toggle buttons.

### Analysis

This example uses HTML served directly from the microcontroller and leverages the WebServer library to handle URL routing. It’s a lightweight, no-backend-required method to control hardware in real time—perfect for rapid prototyping and IoT demos.

## Additional Resources

### Useful links

- [ESP32 Arduino Docs](https://docs.espressif.com/projects/arduino-esp32/en/latest/)
- [ESP32 GitHub Repo](https://github.com/espressif/arduino-esp32)
- [WebServer Library](https://www.arduino.cc/reference/en/libraries/webserver/)
