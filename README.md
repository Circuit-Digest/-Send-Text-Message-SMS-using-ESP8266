# ESP8266 SMS Sender Project

Send text messages (SMS) to any registered mobile number using ESP8266 and IFTTT platform.

## Project Overview

This project demonstrates how to send SMS messages from an ESP8266 microcontroller without requiring any additional microcontroller like Arduino or PIC. The ESP8266 is programmed directly using the Arduino IDE and leverages the IFTTT (If This Then That) platform to send text messages to predefined mobile numbers via WiFi connectivity.

**Source Tutorial:** [Sending SMS using ESP8266 - CircuitDigest](https://circuitdigest.com/microcontroller-projects/sending-sms-using-esp8266)

## Features

- **Standalone Operation**: No additional microcontroller required
- **WiFi Connectivity**: Uses ESP8266's built-in WiFi capabilities
- **IFTTT Integration**: Leverages IFTTT platform for SMS delivery
- **Programmable Messages**: Send custom text messages
- **Easy Configuration**: Simple setup process with detailed instructions

## How It Works

The ESP8266 connects to your WiFi router and makes HTTPS requests to IFTTT's Maker Webhooks service. When triggered, IFTTT sends a predefined text message to your registered mobile number through their SMS service.

**Concept Flow:**
1. ESP8266 connects to WiFi network
2. Makes HTTPS GET request to IFTTT webhook URL
3. IFTTT receives the trigger and executes the SMS action
4. Text message is delivered to the registered mobile number

## Prerequisites

### Hardware Requirements
- ESP8266 Development Board (NodeMCU, Wemos D1 Mini, or similar)
- USB cable for programming
- WiFi network access

### Software Requirements
- Arduino IDE with ESP8266 board package
- IFTTT account (free)
- Mobile phone for receiving SMS

### Online Services
- IFTTT account with SMS applet configured
- Active WiFi internet connection

## IFTTT Setup Instructions

### Step 1: Create IFTTT Account
1. Visit [www.ifttt.com](https://www.ifttt.com)
2. Sign up for a new account or log in to existing account
3. Verify your email address

### Step 2: Configure SMS Service
1. Search for "SMS" or visit the [SMS applet page](https://ifttt.com/sms)
2. Register your mobile number with country code (format: 00[country_code][mobile_number])
   - Example: 00919612365489 (India: 00 + 91 + 9612365489)
3. Click "Send Pin" and verify your mobile number

### Step 3: Setup Maker Webhooks
1. Search for "Maker Webhooks" or visit [this link](https://ifttt.com/maker_webhooks)
2. Click "Connect" to enable the service

### Step 4: Create Custom Applet
1. Navigate to "My Applets" → "New Applet" or use [this link](https://ifttt.com/create)
2. Click on "this" (IF THIS part)
3. Search and select "Maker Webhooks"
4. Choose "Receive a web request" trigger
5. Enter event name (e.g., "ESP") - remember this name
6. Click "Create Trigger"

### Step 5: Configure SMS Action
1. Click on "that" (THEN THAT part)
2. Search and select "SMS"
3. Choose "Send me an SMS" action
4. Enter your custom message text
5. Click "Create Action"
6. Review and click "Finish"

### Step 6: Get Webhook URL
1. Go to Maker Webhooks settings or use [this link](https://ifttt.com/maker_webhooks)
2. Click "Documentation"
3. Note your unique API key (keep it confidential)
4. Replace {event} with your event name from Step 4
5. Test the URL in browser - you should receive SMS

**Example URL format:**
```
https://maker.ifttt.com/trigger/ESP/with/key/YOUR_API_KEY_HERE
```

## Hardware Setup

### ESP8266 Connections
- **Power**: Connect ESP8266 to USB for power and programming
- **WiFi**: No additional connections needed (built-in)
- **Programming**: Use USB-to-Serial converter if not using development board

### Programming Setup
1. Install Arduino IDE
2. Add ESP8266 board package:
   - File → Preferences → Additional Board Manager URLs
   - Add: `http://arduino.esp8266.com/stable/package_esp8266com_index.json`
3. Install ESP8266 board from Board Manager
4. Select your ESP8266 board model
5. Select appropriate COM port

## Arduino Code Configuration

### WiFi Credentials
Update these lines with your WiFi network details:
```cpp
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";
```

### IFTTT Configuration
Update the URL with your event name and API key:
```cpp
String url = "/trigger/YOUR_EVENT_NAME/with/key/YOUR_API_KEY";
```

**Example:**
```cpp
String url = "/trigger/ESP/with/key/b8h22xlElZvP27lrAXS3ljtBa0092_aAanYN1IXXXXX";
```

## Installation and Usage

### Upload Process
1. Connect ESP8266 to computer via USB
2. Open Arduino IDE
3. Copy and paste the provided code
4. Configure WiFi credentials and IFTTT URL
5. Select correct board and port
6. Click "Upload"

### Testing
1. Open Serial Monitor (115200 baud rate)
2. Reset ESP8266 or power cycle
3. Monitor connection status:
   - WiFi connection confirmation
   - IFTTT server connection
   - HTTP request status
   - Success message: "Congratulations! You've fired the ESP event"
4. Check mobile phone for received SMS

### Expected Serial Output
```
connecting to YOUR_WIFI_NAME
....
WiFi connected
IP address: 192.168.1.xxx
connecting to maker.ifttt.com
requesting URL: /trigger/ESP/with/key/YOUR_API_KEY
request sent
headers received
reply was:
==========
Congratulations! You've fired the ESP event
==========
closing connection
```

## Code Structure

### Key Components
- **WiFi Connection**: Establishes connection to local network
- **HTTPS Client**: Secure connection to IFTTT servers
- **HTTP Request**: GET request to trigger webhook
- **Response Handling**: Processes IFTTT response
- **Serial Debugging**: Provides status information

### Main Functions
- `setup()`: Initializes WiFi and sends SMS trigger
- `loop()`: Empty (single execution project)

## Customization Options

### Multiple Triggers
- Create multiple IFTTT applets with different event names
- Modify code to call different URLs based on conditions

### Sensor Integration
- Add sensors to trigger SMS based on environmental conditions
- Integrate with GPIO pins for hardware-based triggers

### Message Customization
- Use IFTTT's value1, value2, value3 parameters for dynamic content
- Send sensor readings or status information in messages

### Scheduled Messages
- Modify loop() to send messages at specific intervals
- Add timer-based functionality

## Troubleshooting

### Common Issues

**Connection Failed Error:**
- Check WiFi credentials
- Verify network connectivity
- Ensure HTTPS port 443 is not blocked

**Invalid Key Error:**
- Verify API key from IFTTT documentation page
- Ensure URL format is correct
- Check event name matches IFTTT applet

**No SMS Received:**
- Verify mobile number registration in IFTTT
- Check IFTTT applet is enabled
- Test webhook URL in browser first
- Check for IFTTT service limits

**WiFi Connection Issues:**
- Verify SSID and password
- Check WiFi signal strength
- Try different ESP8266 board if persistent

### Debugging Tips
1. Always test IFTTT webhook in browser first
2. Use Serial Monitor for detailed debugging
3. Check IFTTT activity log for triggered events
4. Verify mobile number format with country code

## Limitations

- **IFTTT Limits**: Free accounts have usage restrictions
- **Single Message**: Current code sends one message per power cycle
- **No SMS Reception**: Cannot receive or read incoming messages
- **Internet Dependency**: Requires active WiFi and internet connection

## Enhancements and Improvements

### Possible Upgrades
- Add multiple sensor inputs
- Implement message queuing system
- Create web interface for configuration
- Add support for multiple recipient numbers
- Integrate with other IFTTT services (email, notifications)

### Advanced Features
- JSON payload support for dynamic messages
- Two-way communication using other services
- Integration with home automation systems
- Battery-powered operation with deep sleep

## Safety and Security

### Important Considerations
- **API Key Security**: Keep IFTTT API key confidential
- **Network Security**: Use secure WiFi networks
- **SMS Costs**: Be aware of potential SMS charges through IFTTT
- **Rate Limiting**: Avoid excessive API calls

## License and Credits

This project is based on the tutorial from [CircuitDigest](https://circuitdigest.com/microcontroller-projects/sending-sms-using-esp8266).

## Support and Contributions

For questions, issues, or contributions:
- Visit the original tutorial for detailed explanations
- Check IFTTT documentation for service-related issues
- Use Arduino community forums for programming help

---

**Note**: IFTTT services and APIs may change over time. Please refer to current IFTTT documentation for the most up-to-date information.
