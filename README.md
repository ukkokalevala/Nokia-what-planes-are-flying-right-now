Cape Town Pocket Radar — ESP32-C3 Flight Tracker
A real-time, button-operated aircraft tracker built on the ESP32-C3 and a classic Nokia 5510 LCD (84×48 monochrome, PCD8544). Press a button, and the device scans the sky near Cape Town, finds the closest commercial aircraft, looks up its flight route, and displays the flight number, departure/arrival airports, and current altitude.

No expensive radar equipment required—just a tiny microcontroller, a retro LCD, and free, keyless ADS-B APIs.

Features
Live Aircraft Tracking: Fetches real-time ADS-B data within a 30 Nautical Mile radius of Cape Town (customizable).

Flight Route Lookup: Automatically resolves flight callsigns (e.g., FA123) to their origin and destination IATA codes (e.g., CPT > JNB).

Button-Operated Scan: No constant polling. Press the button to scan the sky, saving power and API limits.

Smart Filtering: Scans up to 10 nearby aircraft to find a commercial flight, skipping private/local aircraft without route data.

Retro Nokia Display: Beautifully formatted UI on an 84×48 pixel screen, featuring large IATA airport codes and live altitude.

Error Handling: Displays clear on-screen errors if WiFi fails or APIs are unreachable.

Hardware Requirements
Component	Quantity	Notes
Ai-Thinker NodeMCU ESP-C3-32S-Kit	1	ESP32-C3 microcontroller
Nokia 5510 LCD	1	84×48 pixel monochrome display (PCD8544 controller)
Push Button	1	Momentary tactile switch
Resistors	2	220Ω for LED (optional), 47Ω–330Ω for LCD Backlight
Jumper Wires & Breadboard	-	For prototyping
🔌 Wiring Diagram
Nokia 5510 LCD (SPI)
LCD Pin	ESP32-C3 Pin	Note
RST	IO9	
CE / CS	IO8	
DC	IO7	
DIN / MOSI	IO6	
CLK / SCK	IO4	
VCC	3V3	
GND	GND	
BL (Backlight)	3V3	Via a 47Ω–330Ω resistor
Push Button
Button Pin	ESP32-C3 Pin	Note
Leg 1	IO2	Internal pull-up used in code
Leg 2	GND	
Software Setup
1. Arduino IDE Configuration
Install the ESP32 Board Package via the Arduino Boards Manager (URL: https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json).

Select Tools > Board > ESP32C3 Dev Module.

Set the Flash Size appropriately (usually 4MB).

2. Required Libraries
Install the following libraries via the Arduino Library Manager:

Adafruit PCD8544 Nokia 5110 LCD library

Adafruit GFX Library

ArduinoJson (by Benoit Blanchon) — Version 6.x (Do not use v7 for this code).

HTTPClient (built-in ESP32 core)

WiFi (built-in ESP32 core)

3. Configuration
Open the .ino file and update the following lines at the top:

const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";

// Change these to your exact coordinates if you are not in Cape Town
const float HOME_LAT = -33.9249;
const float HOME_LON = 18.4241;
Note: If the display appears upside down, keep display.setRotation(2); in the setup() function.

How It Works
WiFi Connection: On boot, the ESP32 connects to your local WiFi network.

Button Press: When you press the button, the ESP32 queries the free adsb.lol API for all aircraft within 30 nautical miles of your location.

Data Parsing: It parses the JSON response, skipping aircraft without valid callsigns or private flights.

Route Lookup: It takes the callsign of the closest commercial flight and queries the free adsbdb.com API to find the departure and arrival airports.

Display: The results are drawn on the Nokia screen. If no commercial flight is found, it falls back to showing the closest private aircraft and its altitude.

API Limits & Troubleshooting
This project uses free, keyless APIs. Please respect their rate limits.

adsb.lol: Free for non-commercial use. May rate-limit if abused.

adsbdb.com: Free, but be gentle. The code limits searches to 10 planes per button press to avoid overloading the API.

Common Errors on Screen:

HTTP Err: -1: WiFi connection lost.

HTTP Err: 403 / 429: API rate limit exceeded. Wait a few minutes and try again.

JSON Error: The API returned malformed data (rare).

Screenshots & Demo
(Add a photo or video link of your setup here!)

License
This project is open-source and available under the MIT License. Feel free to modify and build upon it.

Acknowledgments
adsb.lol for providing free ADS-B flight data.

adsbdb.com for the free flight route database.

Adafruit for the excellent PCD8544 and GFX libraries.
