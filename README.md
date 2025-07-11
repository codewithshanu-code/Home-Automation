# Home-Automation
🏠 Home Automation System using Arduino and IoT This project is an IoT-based Home Automation System built using an Arduino microcontroller. It allows users to control home appliances such as lights, fans, or other electronic devices remotely via the Internet using a mobile app or web interface.
# 🏠 IoT-Based Home Automation System using Arduino

This Arduino project demonstrates a **smart home automation system** that integrates multiple sensors and actuators to enhance safety, energy efficiency, and convenience in a home environment. The system automatically controls appliances based on **light**, **motion**, **gas leakage**, and **distance detection**.

---

## 🚀 Features

- 🔆 **Automatic Light Control** using LDR sensor
- 🚶 **Motion Detection** with PIR sensor to control fan or load
- 🌫️ **Gas Leak Detection** with buzzer alert
- 🚪 **Smart Door Automation** using ultrasonic sensor and servo motor
- 🔔 **Real-time Notifications** via Serial Monitor
- 🔴🟢 LED indicators for motion status
- 🧠 Efficient use of relays and digital/analog I/O

---

## 🔧 Hardware Components

| Component                | Description                     |
|--------------------------|---------------------------------|
| Arduino UNO / Nano       | Main microcontroller board      |
| LDR (Light Sensor)       | Detects light intensity         |
| PIR Motion Sensor        | Detects human movement          |
| Gas Sensor (MQ-2)        | Detects LPG/gas in the air      |
| Ultrasonic Sensor (HC-SR04) | Measures distance for door control |
| Servo Motor (SG90)       | Opens/closes door based on distance |
| Piezo Buzzer             | Sounds alarm on gas detection   |
| Relay Module             | Controls AC appliance           |
| Red & Green LEDs         | Motion status indication        |
| Resistors, wires, breadboard | For circuit connections     |

---

## 🧠 code
#include <Servo.h>

// Define sensor and output pins
int output1Value = 0;
int sen1Value = 0;
int sen2Value = 0;

const int gas_sensor = A1;
const int LDR = A0;
const int limit = 400;

Servo servo_7;

// Function to read distance using ultrasonic sensor
long readUltrasonicDistance(int triggerPin, int echoPin) {
  pinMode(triggerPin, OUTPUT);
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  pinMode(echoPin, INPUT);
  return pulseIn(echoPin, HIGH);
}

void setup() {
  Serial.begin(9600);            // Initialize serial communication

  pinMode(A0, INPUT);            // LDR sensor
  pinMode(A1, INPUT);            // Gas sensor
  pinMode(13, OUTPUT);           // Relay for light control

  servo_7.attach(7, 500, 2500);  // Servo motor for door

  pinMode(8, OUTPUT);            // Buzzer
  pinMode(9, INPUT);             // PIR sensor
  pinMode(10, OUTPUT);           // Fan (via NPN transistor)
  pinMode(4, OUTPUT);            // Red LED (no motion)
  pinMode(3, OUTPUT);            // Green LED (motion detected)
}

void loop() {
  // --- Light Intensity Control via LDR ---
  int val1 = analogRead(LDR);
  if (val1 > 500) {
    digitalWrite(13, HIGH);  // Turn ON light
    Serial.print("Bulb ON = ");
    Serial.print(val1);
  } else {
    digitalWrite(13, LOW);   // Turn OFF light
    Serial.print("Bulb OFF = ");
    Serial.print(val1);
  }

  // --- Motion Detection via PIR Sensor ---
  sen2Value = digitalRead(9);
  if (sen2Value == 0) {
    digitalWrite(10, LOW);    // Turn OFF fan
    digitalWrite(4, HIGH);    // Red LED ON
    digitalWrite(3, LOW);     // Green LED OFF
    Serial.print("     || NO Motion Detected    ");
  }

  if (sen2Value == 1) {
    digitalWrite(10, HIGH);   // Turn ON fan
    delay(5000);              // Keep fan on for 5 seconds
    digitalWrite(4, LOW);     // Red LED OFF
    digitalWrite(3, HIGH);    // Green LED ON
    Serial.print("     || Motion Detected!      ");
  }

  // --- Gas Detection ---
  int val = analogRead(gas_sensor);
  Serial.print(" || Gas Sensor Value = ");
  Serial.print(val);

  if (val > limit) {
    tone(8, 650);   // Turn ON buzzer
  }
  delay(300);
  noTone(8);        // Turn OFF buzzer

  // --- Servo Door Control via Ultrasonic Sensor ---
  sen1Value = 0.01723 * readUltrasonicDistance(6, 6);  // Convert pulse time to cm

  if (sen1Value < 100) {
    servo_7.write(90);  // Open door
    Serial.print("     || Door Open! ; Distance = ");
    Serial.println(sen1Value);
  } else {
    servo_7.write(0);   // Close door
    Serial.print("     || Door Closed! ; Distance = ");
    Serial.println(sen1Value);
  }

  delay(10);  // Slight delay for performance
}


### ✅ Light Control with LDR
- Turns ON a bulb (via relay on pin 13) when light level is below threshold (value > 500).
- Controlled through analog input `A0`.

### ✅ Motion Detection with PIR
- Detects motion through PIR sensor on pin `9`.
- Activates load via NPN transistor (pin 10).
- LEDs indicate motion state: Red = No Motion, Green = Motion Detected.

### ✅ Gas Detection
- Monitors gas level via analog input `A1`.
- If gas reading > `400`, activates piezo buzzer (`pin 8`) as an alert.

### ✅ Smart Door with Ultrasonic & Servo
- Measures distance using ultrasonic sensor on pin `6`.
- If distance < 100 cm, servo rotates to 90° (open door).
- If distance ≥ 100 cm, servo resets to 0° (closed door).

---

## 🛠️ Pin Configuration

| Component          | Arduino Pin |
|-------------------|-------------|
| LDR               | A0          |
| Gas Sensor        | A1          |
| Relay             | 13          |
| Servo Motor       | 7           |
| Piezo Buzzer      | 8           |
| PIR Sensor        | 9           |
| Load (via NPN)    | 10          |
| Red LED           | 4           |
| Green LED         | 3           |
| Ultrasonic Sensor | 6 (shared trigger/echo) |

---

## 💻 How to Use

1. Connect the components as per the pin configuration.
2. Upload the provided code to your Arduino board using **Arduino IDE**.
3. Open the **Serial Monitor** at 9600 baud to see sensor data and status messages.
4. Test each module (light control, motion, gas detection, door automation) individually.
5. Power the board and watch the automation in action!

---

## 📂 Files Included

- `home_automation.ino` - Main Arduino code
- `README.md` - Project documentation
- *(Add circuit diagram, images, or video links if available)*

---



