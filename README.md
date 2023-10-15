# IOT-Disposer
![image](https://github.com/priyanka8324/IOT-Disposer/assets/129300687/64eb8372-d50f-4e2d-be73-5ad8715abbb4)
The Voice-Activated Smart Waste Disposal System is a cutting-edge solution designed to address various challenges associated with traditional proximity-based sensors and manual waste disposal methods. This innovative project combines Arduino technology with Bluetooth connectivity and a servo motor to create an efficient, hygienic, and user-friendly waste disposal system.

One of the key advantages of this system is its immunity to false triggers caused by trespassers or animals, a common issue with proximity-based sensors. By utilizing voice commands through a mobile Bluetooth app, this project ensures that the waste disposal process is initiated only when instructed by the user, eliminating unnecessary lid openings.

The system's design focuses on hygiene concerns associated with manual lid operation. With a simple voice command like "open" issued to the Google Voice Assistant via the Bluetooth app, the system swiftly responds. The Bluetooth module receives the command and transmits it to the connected Arduino, which then activates the servo motor. This motor efficiently rotates the lid 90 degrees, allowing for convenient waste disposal without the need for physical contact. This automated process not only enhances hygiene but also provides a touchless and user-friendly experience.

Additionally, the system addresses the issue of animal scavenging. With the lid securely closed, the chances of animals gaining access to the waste are minimized. The "close" command in the Google Voice Assistant triggers the Bluetooth module to communicate with the Arduino, causing the servo motor to return the lid to its closed position at 0 degrees.

In summary, the Voice-Activated Smart Waste Disposal System offers a comprehensive solution for efficient, hygienic, and animal-resistant waste disposal. By harnessing the power of Arduino, Bluetooth technology, and voice commands, this project represents a significant step forward in waste management efficiency and user convenience.

#include <Servo.h>
#include <SoftwareSerial.h>

Servo myservo;
int TX = 11;
int RX = 10;
SoftwareSerial bluetooth(TX, RX);

int servoPosition = 0;
int targetPosition = 0; // Desired position for the servo
const int increment = 1; // Increment for smoother movement

void setup() {
  myservo.attach(9);
  Serial.begin(9600);
  bluetooth.begin(9600);
  myservo.write(servoPosition);
}

void loop() {
  if (bluetooth.available() > 1) {
    String command = bluetooth.readString();

    if (command == "open" || command == "Open") {
      Serial.println("Opening...");
      targetPosition = 180; // Set the desired position
    }  
    else if (command == "close" || command == "Close") {
      Serial.println("Closing...");
      targetPosition = 0; // Set the desired position
    }
  }

  // Gradually move the servo toward the target position
  if (servoPosition < targetPosition) {
    servoPosition += increment;
  } else if (servoPosition > targetPosition) {
    servoPosition -= increment;
  }

  // Update the servo position
  myservo.write(servoPosition);

  // Add a small delay to control the speed of movement
  delay(10);
}
