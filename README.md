# LineController
## 📌 Project Overview
<p>This project implements a Line Follower Robot using an Arduino microcontroller, a 6-channel analog IR sensor array, and an L298N H-bridge motor driver.<br/>
The robot detects a black line on a white surface and follows it autonomously by adjusting the speed and direction of two DC motors.</p>

## 🧩 Components Used
- Arduino Development Board
- 6-Channel Analog IR Sensor Array (or 8-channel as well)
- L298N Dual H-Bridge Motor Driver
- 2 × DC Motors
- Battery Pack
- Chassis
- Connecting (Jumper) wires

## 🔌 Hardware Connections
### 1️⃣ IR Sensor Array → ESP32
Each IR sensor provides an analog output corresponding to surface reflectivity.

| Sensor | ESP32 GPIO |
| ------ | ---------- |
| IR1    | GPIO 32    |
| IR2    | GPIO 33    |
| IR3    | GPIO 34    |
| IR4    | GPIO 35    |
| IR5    | GPIO 36    |
| IR6    | GPIO 39    |

#### Note:
- GPIOs 34–39 are input-only, ideal for analog sensors
- Only ADC1 pins are used to avoid Wi-Fi conflicts

### 2️⃣ L298N Motor Driver → ESP32

| L298N Pin | ESP32 GPIO    | Function          |
| --------- | ------------- | ----------------- |
| IN1       | GPIO 25       | Motor A direction |
| IN2       | GPIO 26       | Motor A direction |
| IN3       | GPIO 27       | Motor B direction |
| IN4       | GPIO 14       | Motor B direction |
| ENA       | GPIO 12 (PWM) | Motor A speed     |
| ENB       | GPIO 13 (PWM) | Motor B speed     |

### Power Connections:
- L298N +12V / +Vs → Battery
- GND shared between ESP32 and L298N

## 🧠 Working Principle
- The IR sensors continuously scan the surface.
- Black line reflects less IR → lower analog value<br/>White surface reflects more IR → higher analog value
- The ESP32 reads sensor values and determines the line position.
- Motor speeds are adjusted using PWM to steer the robot.

## ⚙️ Motor Control Logic
### Forward Motion
- Both motors rotate forward at equal speed
### Left Turn
- Left motor slows down
- Right motor speeds up
### Right Turn
- Left motor speeds up
- Right motor slows down
#### Speed control is achieved using PWM signals on ENA and ENB pins

## 🔄 PWM Configuration (Arduino)
The Arduino uses LEDC hardware PWM:
  - Frequency: 1 kHz
  - Resolution: 8-bit (0–255 duty cycle)<br/>
<p>PWM allows smooth motor speed variation for accurate line tracking.</p>

## 🛠️ Calibration
Before running the robot:
1. Place sensors on white surface → note readings
2. Place sensors on black line → note readings
3. Set a threshold value between black and white
4. Use this threshold for line detection

## ⚠️ Important Notes
- Ensure common ground between all components
- Avoid using ESP32 boot-sensitive pins (GPIO 0, 2, 15)
- Use stable power supply for motors
- Start with threshold-based logic before PID control

## 🚀 Future Improvements
- Implement PID control for smoother turns
- Add junction detection
- Display sensor values on Serial Monitor or OLED
- Optimize speed dynamically based on curvature
