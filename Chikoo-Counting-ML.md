# TanGO – Automated Counting Machine for Chikoo Counting
*Project Based Learning*

## Problem Statement
Manual counting of chikoo fruits in bags or crates is slow, prone to human error, and labor-intensive. We developed a sensor-based automated counting machine that filters out impurities, uses infrared sensors and an ESP32-CAM running a machine learning model to detect and count individual chikoos in real time, and ensures efficient, hands-free operation through an integrated control and display system.

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
## Initial Constraints and How We Addressed Them

| Constraint                  | How Addressed                                |
|----------------------------|---------------------------------------------|
| Size  | Designed an enclosure to hold upto 130 chikoos.|
| Budget        | Kept costs low by selecting affordable components and using recycled scrap wood.  |
| Environmental conditions         | urrently used in dry indoor settings; future versions may include waterproofing.   |
| Sensor accuracy |Fine-tuned the image recognition model to handle variations in lighting and chikoo appearance by training it on 500+ images.|
| Data Processing           | Used ESP32 CAM’s onboard capabilities for edge processing without needing a laptop or cloud.|
| Inefficient packing       | Introduced a farmer-controlled batch limit via keypad; buzzer alerts when the limit is reached.|

```python
import math

# Container box dimensions in cm
length = 50
width = 30
height = 30
V_container = length * width * height  # in cm^3

# Chikoo radius in cm (average diameter = 7.5 cm, so radius = 3.75 cm)
r_chikoo = 3.75
V_chikoo = (4/3) * math.pi * (r_chikoo ** 3)

# Random Close Packing (RCP) efficiency
rcp_percentage = 0.64
effective_volume = rcp_percentage * V_container

# Estimate number of chikoos
N_chikoos = effective_volume / V_chikoo

# Display results
print("Volume of container box:", V_container, "cm³")
print("Volume of one chikoo:", round(V_chikoo, 3), "cm³")
print("Effective usable volume (RCP 64%):", round(effective_volume, 2), "cm³")
print("Estimated number of chikoos:", round(N_chikoos), "≈", int(round(N_chikoos)))

```
```yaml
Volume of container box: 45000 cm³
Volume of one chikoo: 220.893 cm³
Effective usable volume (RCP 64%): 28800.0 cm³
Estimated number of chikoos: 130
```
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
![Image](https://github.com/user-attachments/assets/9dc42fe5-ea81-493f-b624-25210ab797f1)
- **Arduino IDE (C++):** Embedded software to integrate sensors, ML model output, display, and mechanical controls.
[Machine Learning Code](https://www.youtube.com/watch?v=uDzeQ00ZcTI)
[ML Running (Mac)](https://www.youtube.com/watch?v=ONefa4cJxFY)

---

## Mechanical Design
- Laser-cut wooden panels form the enclosure for durability and sustainability.  
- Chute system mounted on top for controlled fruit entry.  
- Servo-operated trapdoor regulates fruit flow, closing temporarily when capacity is reached and reopening to resume counting.  
- Retrieval door at the bottom allows collection of counted fruits.

## AutoCAD Model- Isometric View

![Image](https://github.com/user-attachments/assets/33754803-0c90-4e26-95b4-3becb76a52ea)
![Image](https://github.com/user-attachments/assets/ca4c80ee-3805-40ad-8a07-b3404253b5f0)
![Image](https://github.com/user-attachments/assets/43f94dd4-0ab3-4257-ad04-ea13d4088e53)

## Prototype

![Image](https://github.com/user-attachments/assets/3a492f54-e54d-48a0-b1a0-8ddf7a200ff6)
![Image](https://github.com/user-attachments/assets/93954f74-71e5-4049-8490-8e5cd4d1b344)
![Image](https://github.com/user-attachments/assets/18dda535-8cd2-40ca-997e-bdaa16c658cd)
![Image](https://github.com/user-attachments/assets/61a29925-c179-4b1f-a371-db5c28b47b3a)
![Image](https://github.com/user-attachments/assets/7eabab05-d564-439e-a389-0c2d25321f3c)
![Image](https://github.com/user-attachments/assets/25507f69-8c3f-40d7-8ab7-eda575ff8110)
![Image](https://github.com/user-attachments/assets/50cdb4aa-1f46-4167-b981-60b16235575e)

---

## Workflow
1. Fruits are placed in the chute.  
2. IR sensor detects each fruit passing through*.  
3. ESP32 CAM captures images to confirm fruit recognition via ML.  
4. Count increments on the LCD display in real-time.  
5. When count reaches user-set or default max (130), buzzer sounds.  
6. Trapdoor closes to temporarily halt flow.  
7. Operator retrieves counted fruits through the door.  
8. Trapdoor reopens to resume operation.

* The IR sensors emit infrared light and detect reflections to sense when a chikoo passes through the chute. When a single chikoo crosses, a single reflection is detected. If two or more chikoos attempt to pass simultaneously, the sensors detect multiple distinct reflections, signaling multiple objects. This sensor data is continuously transmitted to the microcontroller, which processes the signals in real time. Based on this input, the system updates the displayed count, ensuring an accurate tally by incrementing according to the number of detected fruits.

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


