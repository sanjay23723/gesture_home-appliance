# gesture_home-appliance
Gesture control home appliances are devices that can be controlled using hand or body gestures, eliminating the need for physical contact or remote controls. These appliances use sensors and cameras to detect and interpret gestures, allowing users to interact with devices in a more intuitive and natural way.
#include <Arduino.h>
#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_ADXL345_U.h>

// Define gesture recognition thresholds
#define GESTURE_THRESHOLD 10

// Define appliance control pins
#define APPLIANCE_PIN 13

Adafruit_ADXL345_Unified accel = Adafruit_ADXL345_Unified(12345);

void setup() {
  Serial.begin(9600);
  accel.begin();
  pinMode(APPLIANCE_PIN, OUTPUT);
}

void loop() {
  // Read accelerometer data
  sensors_event_t event;
  accel.getEvent(&event);

  // Calculate gesture
  int gesture = calculateGesture(event.acceleration.x, event.acceleration.y, event.acceleration.z);

  // Control appliance based on gesture
  if (gesture == 1) {
    digitalWrite(APPLIANCE_PIN, HIGH); // Turn on appliance
  } else if (gesture == 2) {
    digitalWrite(APPLIANCE_PIN, LOW); // Turn off appliance
  }

  delay(50);
}

int calculateGesture(float x, float y, float z) {
  // Calculate gesture based on accelerometer data
  if (x > GESTURE_THRESHOLD && y < -GESTURE_THRESHOLD) {
    return 1; // Gesture 1: Turn on appliance
  } else if (x < -GESTURE_THRESHOLD && y > GESTURE_THRESHOLD) {
    return 2; // Gesture 2: Turn off appliance
  } else {
    return 0; // No gesture recognized
  }
}
