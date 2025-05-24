# TanGO – Automated Counting Machine for Chikoo Counting

## Problem Statement
Manual counting of chikoo fruits in bags or crates is slow, prone to human error, and labor-intensive. The producer needs a reliable automated system to count fruits quickly and accurately, improving operational efficiency.

---

## Project Objectives
1. Automate chikoo fruit counting using sensor-based detection.  
2. Use machine learning for reliable fruit recognition.  
3. Provide real-time count display and alerts.  
4. Implement a mechanical system for fruit flow and capacity management.  
5. Construct the machine with sustainable materials like laser-cut recycled wood.

---

## Functions
- **ML-based fruit recognition** using ESP32 CAM and Edge Impulse.  
- **Maximum capacity alert** with a piezo buzzer and automatic trapdoor operated by a servo motor.  
- **Real-time count display** on a 16x2 LCD screen.  
- **User input** via 4x4 keypad to set custom count limits.  
- **Mechanical design** includes a chute system and retrieval door for fruit collection.

---

## Hardware Components

| Component           | Purpose                                      |
|---------------------|----------------------------------------------|
| ESP32 CAM           | Captures images and runs ML fruit detection  |
| Arduino UNO SMD     | Controls sensors, display, buzzer, servo     |
| IR Sensor Module    | Detects passing chikoo fruits                 |
| 16x2 LCD Display    | Shows current count and alerts                 |
| Piezo Buzzer        | Sounds alert when max capacity reached        |
| 4x4 Matrix Keypad   | User input for custom count                    |
| MG996R Servo Motor  | Controls trapdoor opening/closing             |

---

## Software Components
- **Edge Impulse:** ML model training on 500+ chikoo images for object detection.  
- **Arduino IDE (C++):** Embedded software to integrate sensors, ML model output, display, and mechanical controls.

---

## Mechanical Design
- Laser-cut wooden panels form the enclosure for durability and sustainability.  
- Chute system mounted on top for controlled fruit entry.  
- Servo-operated trapdoor regulates fruit flow, closing temporarily when capacity is reached and reopening to resume counting.  
- Retrieval door at the bottom allows collection of counted fruits.

---

## Workflow
1. Fruits are placed in the chute.  
2. IR sensor detects each fruit passing through*.  
3. ESP32 CAM captures images to confirm fruit recognition via ML.  
4. Count increments on the LCD display in real-time.  
5. When count reaches user-set or default max (110), buzzer sounds.  
6. Trapdoor closes to temporarily halt flow.  
7. Operator retrieves counted fruits through the door.  
8. Trapdoor reopens to resume operation.

* The IR sensors emit infrared light and detect reflections to sense when a chikoo passes through the chute. When a single chikoo crosses, a single reflection is detected. If two or more chikoos attempt to pass simultaneously, the sensors detect multiple distinct reflections, signaling multiple objects. This sensor data is continuously transmitted to the microcontroller, which processes the signals in real time. Based on this input, the system updates the displayed count, ensuring an accurate tally by incrementing according to the number of detected fruits.

---

## Initial Constraints and How We Addressed Them

| Constraint                  | How Addressed                                |
|----------------------------|---------------------------------------------|
| Accurate fruit recognition  | Trained ML model with 500+ images on Edge Impulse |
| Handling fruit flow         | Servo-controlled trapdoor and chute design  |
| Real-time counting          | Integration of IR sensor and ESP32 CAM ML   |
| User interaction            | 4x4 keypad for custom counts                  |
| Durable enclosure           | Laser-cut wooden panels                       |
| Power and connectivity      | Arduino and ESP32 integration                 |

---

## Final Problem Statement
Develop a sensor-based automated counting machine that filters impurities, tracks the count in real time, and ensures efficient operations for chikoo fruit packing.

---

## Scope

### In Scope
- Designing and building the prototype automated counting machine.  
- Implementing ML-based fruit recognition.  
- Constructing enclosure using laser-cut wood.  
- Creating a real-time display and alert system.  
- Testing with actual chikoo fruits.

### Out of Scope
- Large scale production beyond prototype.  
- Integration with farm inventory systems.  
- Maintenance or after-sales service.

---

