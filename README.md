# AUTOMATIC-INDICATION-OF-BLOCKAGE-IN-PIPELINE-SYSTEM-BY-USING-LORA
              Sewer flooding in inhabited areas poses great encounters to sewer network operators. The damage caused by wastewater from manholes can be enormous due to high quality additions and installations in basements and underground car parks. Expanding sewer networks to prevent flooding during periods of extreme precipitation is not economically justified. Rather, a risk management program must be established, the associated risk must be evaluated, and the possible hazards that result from floods must be analyzed. To describe the potential hazards, area-related geographic data can be used as it is normally collected and managed by the microcontroller interfaced with ultrasonic sensors. In this system, implementation of Arduino based technology system interfacing with ultrasonic sensors for monitoring urban sewage manholes which may be useful for governments as well as local activities to take instant action on critical flooding situation. The sewage system is a key part of a city, and it is what causes rainwater and grey water from homes and businesses to build up. To keep from causing a lot of trouble, the sewage management system must have a way to track what's going on and a plan for how to grow. But in several overcrowded towns around the world, there is no way to keep track of how many people live there, and the process of growing takes a long time and is full of problems. This project shows a model for an intelligent sewerage management system that can provide real-time monitoring without making big changes to the old system. Through the sensors, the state of the sewer acts as an input. The microcontroller then stores the value, sends it to the lora wan module, and collects trash based on the current situation. In the meantime, the information enters the monitoring system after being processed. Trial installations of the proposed system have shown that it can be used to watch live conditions and prevent solid waste from clogging sewers.
              **************************************************************************************************************************************************************************************
              
#include <LiquidCrystal.h>
// Pin configuration for LCD
const int rs = 13, en = 12, d4 = 11, d5 = 10, d6 = 9, d7 = 8; LiquidCrystal lcd(rs, en, d4, d5, d6, d7);
// Sensor pins
const int flowSensorPin = A0;	// Flow sensor
const int ultrasonicSensorPin = A1; // Ultrasonic sensor const int pressureSensorPin = A2;	// Pressure sensor const int vibrationSensorPin = A3; // Vibration sensor
// Output pins for alerts
const int sprinklerPin = 2; // Water sprinkler (LED for visualization) const int alertPin = 3;	// Buzzer or secondary alert
const int systemStatusPin = 4; // System status LED
// Variables for sensor readings int flowValue = 0;
int ultrasonicValue = 0; int pressureValue = 0; int vibrationValue = 0;
// Thresholds for sensors
const int flowThreshold = 80;	// Example threshold for flow sensor
const int ultrasonicThreshold = 20; // Example threshold for ultrasonic (distance in cm) const int pressureThreshold = 90; // Example threshold for pressure sensor
const int vibrationThreshold = 100; // Example threshold for vibration sensor void setup() {
pinMode(flowSensorPin, INPUT); pinMode(ultrasonicSensorPin, INPUT); pinMode(pressureSensorPin, INPUT);
pinMode(vibrationSensorPin, INPUT);
 
pinMode(sprinklerPin, OUTPUT); pinMode(alertPin, OUTPUT); pinMode(systemStatusPin, OUTPUT);
lcd.begin(16, 2); // Initialize LCD with 16x2 dimensions Serial.begin(9600);
// Initial system status digitalWrite(systemStatusPin, HIGH); digitalWrite(sprinklerPin, LOW); digitalWrite(alertPin, LOW);
}
void loop() {
// Read sensor values
flowValue = analogRead(flowSensorPin); ultrasonicValue = analogRead(ultrasonicSensorPin); pressureValue = analogRead(pressureSensorPin); vibrationValue = analogRead(vibrationSensorPin);
// Map sensor values to meaningful ranges (e.g., percentage or real-world units) int flowPercent = map(flowValue, 0, 1023, 0, 300);
int ultrasonicDistance = map(ultrasonicValue, 1014, 1023, 0, 100); // Example mapping
int pressurePercent = map(pressureValue, 0, 1023, 0, 100);
int vibrationLevel = map(vibrationValue, -1000, 0, 0, 50);
// Display data on LCD lcd.setCursor(0, 0); lcd.print("Sewage System	"); lcd.setCursor(0, 1); lcd.print("Flow: "); lcd.print(flowPercent); lcd.print("%	");
delay(2000);
 
lcd.setCursor(0, 0); lcd.print("Ult:"); lcd.print(ultrasonicDistance); lcd.print(""); lcd.setCursor(0, 1); lcd.print("P:"); lcd.print(pressurePercent); lcd.print("%V:"); lcd.print(vibrationLevel); lcd.print("	");
// Send data to Serial Monitor Serial.print("Flow (%): "); Serial.println(flowPercent); Serial.print("Ultrasonic (cm): "); Serial.println(ultrasonicDistance); Serial.print("Pressure (%): "); Serial.println(pressurePercent); Serial.print("Vibration Level: "); Serial.println(vibrationLevel); delay(2000);
//Alert conditions bool isAlert = false;
if (flowPercent < flowThreshold) { isAlert = true;
Serial.println("Alert: Low flow detected!");
}
if (ultrasonicDistance < ultrasonicThreshold) { isAlert = true;
Serial.println("Alert: Blockage detected!");
 
}
if (pressurePercent > pressureThreshold) { isAlert = true;
Serial.println("Alert: High pressure detected!");
}
if (vibrationLevel >= vibrationThreshold) { isAlert = true;
Serial.println("Alert: Excessive vibration detected!");
}
// Trigger alerts if necessary if (isAlert) {
digitalWrite(alertPin, HIGH); lcd.setCursor(0, 0); lcd.print("ALERT: Check		"); lcd.setCursor(0, 1); lcd.print("System Status!	");
} else {
digitalWrite(alertPin, LOW);
}
delay(1000); 
}
*****************************************************************************************************************************************************************************************************************************
