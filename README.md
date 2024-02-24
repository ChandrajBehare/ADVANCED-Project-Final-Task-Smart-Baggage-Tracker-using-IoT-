# ADVANCED-Project-Final-Task-Smart-Baggage-Tracker-using-IoT-
Project Summary: In response to the common issue of lost luggage, smart baggage trackers offer a solution by detecting the bag's location in case of theft and sending immediate coordinates to the user's phone.

Below is an advanced example of source code for a smart baggage tracker using an IoT platform like Arduino. This code utilizes GPS and GSM modules to track the location of the baggage and send real-time coordinates to the user's phone via SMS:

Source Code

#include <TinyGPS++.h>
#include <SoftwareSerial.h>

#define GPS_RX_PIN 4 // GPS module RX pin
#define GPS_TX_PIN 3 // GPS module TX pin
#define GSM_RX_PIN 6 // GSM module RX pin
#define GSM_TX_PIN 7 // GSM module TX pin

TinyGPSPlus gps;
SoftwareSerial gpsSerial(GPS_RX_PIN, GPS_TX_PIN); // RX, TX
SoftwareSerial gsmSerial(GSM_RX_PIN, GSM_TX_PIN); // RX, TX

const char* phoneNumber = "+1234567890"; // Replace with your phone number

void setup() {
  Serial.begin(9600);
  gpsSerial.begin(9600);
  gsmSerial.begin(9600);

  // Initialize GSM module
  gsmSerial.println("AT");
  delay(1000);
  gsmSerial.println("AT+CMGF=1"); // Set SMS mode to text
  delay(1000);
}

void loop() {
  while (gpsSerial.available() > 0) {
    if (gps.encode(gpsSerial.read())) {
      if (gps.location.isValid()) {
        float latitude = gps.location.lat();
        float longitude = gps.location.lng();

        // Send location coordinates via SMS
        sendSMS(latitude, longitude);
      }
    }
  }
}

void sendSMS(float latitude, float longitude) {
  Serial.print("Sending SMS...");
  
  gsmSerial.print("AT+CMGS=\"");
  gsmSerial.print(phoneNumber);
  gsmSerial.println("\"");
  delay(1000);
  gsmSerial.print("Latitude: ");
  gsmSerial.print(latitude, 6);
  gsmSerial.print("\nLongitude: ");
  gsmSerial.print(longitude, 6);
  gsmSerial.println((char)26); // End SMS transmission
  delay(1000);

  Serial.println("Location sent.");
}


This code sets up an Arduino with GPS and GSM modules to track the location of the baggage. It reads GPS data using the TinyGPS++ library and sends the coordinates to the user's phone via SMS using the GSM module.

Please note:

1.Make sure to connect the GPS module to the designated RX and TX pins on the Arduino, and similarly for the GSM module.
2.Replace "AT+CMGF=1" with the appropriate AT command if your GSM module requires a different command to set SMS mode to text.
3.Replace "+1234567890" with the phone number where you want to receive the location updates.
4.Ensure that the GSM module is properly configured to send and receive SMS messages.

This code provides a foundation for a smart baggage tracker system and can be expanded with additional features such as battery management, geofencing, and remote tracking via a web interface.





