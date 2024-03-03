# Arduino-project
the simulation  for this project is link given below
https://www.tinkercad.com/things/93fMEjvQmxW-arduino-with-lm35-sensor

main code for this project in tinkercad
// Define constants for temperature thresholds
const int TEMP_THRESHOLD_LOW = 30; // Temperature threshold below which LED blinks faster
const int TEMP_THRESHOLD_HIGH = 30; // Temperature threshold above which LED blinks slower

// Define variables to hold temperature sensor reading and LED state
int sensorValue = 0;
int ledState = LOW;

// Define variables to keep track of time
unsigned long previousMillis = 0;
const long intervalLowTemp = 250; // Blink interval for low temperature
const long intervalHighTemp = 500; // Blink interval for high temperature

void setup() {
  pinMode(LED_BUILTIN, OUTPUT); // Set the built-in LED pin as an output
  Serial.begin(9600); // Initialize serial communication for debugging
}

void loop() {
  // Read the analog value from the LM35 temperature sensor
  sensorValue = analogRead(A0);

  // Convert the analog value to temperature in Celsius
  float temperatureC = (sensorValue * 5.0 * 100.0) / 1024.0;

  // Print the temperature for debugging
  Serial.print("Temperature: ");
  Serial.print(temperatureC);
  Serial.println(" C");

  // Check if the temperature is below the low threshold
  if (temperatureC < TEMP_THRESHOLD_LOW) {
    // Blink the LED at a faster interval
    blinkLED(intervalLowTemp);
  } 
  // Check if the temperature is above the high threshold
  else if (temperatureC > TEMP_THRESHOLD_HIGH) {
    // Blink the LED at a slower interval
    blinkLED(intervalHighTemp);
  }
}

void blinkLED(long interval) {
  unsigned long currentMillis = millis(); // Get the current time

  // Check if it's time to toggle the LED
  if (currentMillis - previousMillis >= interval) {
    // Save the last time the LED was toggled
    previousMillis = currentMillis;

    // Toggle the LED state
    if (ledState == LOW) {
      ledState = HIGH;
    } else {
      ledState = LOW;
    }

    // Apply the new LED state
    digitalWrite(LED_BUILTIN, ledState);
  }
}
