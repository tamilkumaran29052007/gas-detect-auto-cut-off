# gas-detect-auto-cut-off
Arduino-based gas leakage detection and automatic gas cut off system
#include <Servo.h>

const int gasSensorPin = A0;
const int buzzerPin = 8;
const int servoPin = 9;

// Adjust this after checking Serial Monitor
const int gasThreshold = 400;

// Servo positions
const int valveOpenAngle = 0;
const int valveCloseAngle = 90;

Servo gasValve;

void setup()
{
  Serial.begin(9600);

  pinMode(gasSensorPin, INPUT);
  pinMode(buzzerPin, OUTPUT);

  gasValve.attach(servoPin);

  // Initial safe condition
  digitalWrite(buzzerPin, LOW);
  gasValve.write(valveOpenAngle);

  Serial.println("GAS LEAKAGE AUTO CUT-OFF SYSTEM");
  Serial.println("MQ sensor warming up...");

  delay(20000);

  Serial.println("System Ready");
}

void loop()
{
  int gasValue = analogRead(gasSensorPin);

  Serial.print("Gas Value: ");
  Serial.println(gasValue);

  if (gasValue >= gasThreshold)
  {
    // Gas detected
    digitalWrite(buzzerPin, HIGH);

    // Close gas valve
    gasValve.write(valveCloseAngle);

    Serial.println("GAS LEAKAGE DETECTED!");
    Serial.println("BUZZER: ON");
    Serial.println("VALVE: CLOSED");
  }
  else
  {
    // Normal condition
    digitalWrite(buzzerPin, LOW);

    // Keep valve open
    gasValve.write(valveOpenAngle);

    Serial.println("Gas Level: NORMAL");
    Serial.println("BUZZER: OFF");
    Serial.println("VALVE: OPEN");
  }

  delay(500);
}

