# 🔎 Object Detector Sensor (Arduino Project)

An ultrasonic object detection system built with **Arduino**, designed to scan an area using a servo-mounted ultrasonic sensor. When an object is detected within 20 cm, a buzzer alerts the user. If the path is clear, a green LED indicates safety.

---

## 📌 Project Overview

This project uses an ultrasonic distance sensor mounted on a servo motor that continuously rotates between **0° and 180°**, scanning the surrounding area.

### Behavior:
- If an object is detected at a distance **less than 20 cm**:
  - 🔔 The buzzer turns ON  
  - 💡 The green LED turns OFF  

- If no object is detected within 20 cm:
  - 🔕 The buzzer turns OFF  
  - 💚 The green LED turns ON  

Distance readings are displayed in the **Serial Monitor**.

---

## 🧩 Components Used

- Arduino Board (e.g., Arduino Uno)
- Ultrasonic Sensor (HC-SR04)
- Servo Motor (e.g., SG90)
- Buzzer
- Green LED
- Resistor (220Ω recommended for LED)
- Push Button
- Jumper wires
- Breadboard

---

## ⚙️ How It Works

1. The servo motor rotates the ultrasonic sensor from 0° to 180° and back.
2. The ultrasonic sensor:
   - Sends an ultrasonic pulse.
   - Waits for the echo signal.
3. The Arduino calculates the distance using:

```cpp
distance = duration * 0.034 / 2;
```

4. If the distance is less than 20 cm, the buzzer activates.
5. If the distance is greater than or equal to 20 cm, the LED stays on to indicate a clear path.

---

## 🔌 Pin Configuration

| Component           | Arduino Pin |
|---------------------|------------|
| Trig (Ultrasonic)   | 9          |
| Echo (Ultrasonic)   | 10         |
| Servo Motor         | 8          |
| Buzzer              | 7          |
| Green LED           | 6          |
| Button              | 5          |

---

## 💻 Arduino Code

```cpp
#include <Servo.h>

Servo myServo;

const int trigPin = 9;
const int echoPin = 10;
const int buzzerPin = 7;
const int ledVerde = 6;
const int buttonPin = 5;

int pos = 0;
int x = 10;

long duration;
float distance;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(ledVerde, OUTPUT);
  pinMode(buttonPin, INPUT);

  myServo.attach(8);
}

void loop() {

  // Servo sweep logic
  if (pos >= 180) {
    x = -10;
  } else if (pos <= 0) {
    x = 10;
  }

  pos += x;
  myServo.write(pos);
  delay(5);

  // Trigger ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Measure echo duration
  duration = pulseIn(echoPin, HIGH);

  // Calculate distance in cm
  distance = duration * 0.034 / 2;

  // Detection logic
  if (distance < 20) {
    digitalWrite(buzzerPin, HIGH);
    digitalWrite(ledVerde, LOW);
  } else {
    digitalWrite(buzzerPin, LOW);
    digitalWrite(ledVerde, HIGH);
  }

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  delay(500);
}
```

---

## ▶️ How to Run the Project

1. Assemble the circuit according to the pin configuration.
2. Open Arduino IDE.
3. Copy and paste the code.
4. Select the correct board and port.
5. Upload the code to the Arduino.
6. Open the Serial Monitor (9600 baud rate).
7. Power the board and test object detection.

---

## 🚀 Possible Improvements

- Add an LCD display for real-time distance visualization.
- Create a radar-style visualization using Processing.
- Add adjustable detection distance.
- Improve scanning smoothness.
- Add enclosure for practical use.

---

## 📊 Applications

- Obstacle detection systems
- Parking assistance prototypes
- Robotics navigation
- Security alert systems
- Smart automation projects

---

## 👩‍💻 Authors

- Cîmpanu Anastasia  
- Sălăjan Denisa Patricia  

---

## 📄 License

This project is open-source and free to use for educational purposes.
