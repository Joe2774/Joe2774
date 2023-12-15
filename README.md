#define BLYNK_TEMPLATE_ID "TMPL2bYxwOku6"
#define BLYNK_TEMPLATE_NAME "Vision at Risk"
#define BLYNK_AUTH_TOKEN "wBxEtomvzar7jnfiZ9tWm6hiM2N3iAsx"
/* Mazen project */
#include <BlynkSimpleEsp8266.h>
#include <SoftwareSerial.h>
/* Fill-in information from Blynk Device Info here */




/* Comment this out to disable prints and save space */
#define BLYNK_PRINT Serial

const char SSID[]   = "0";     // Network SSID (name) Youssef
const char PASS[]   = "12345678";    // 123456789 Network password (use for WPA, or use as key for WEP)
/* blynk timer */
BlynkTimer timer;

#define TRIG_PIN D7
#define ECHO_PIN D6

//define sound velocity in cm/uS
#define SOUND_VELOCITY 0.034
 long duration, distance;
float Factor  = 0.096; // another range factor : 0.0625 
unsigned int glucoma; 


void sendglucomaSensor()
{
   // Clears the trigPin
 digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Measure the duration of the pulse on the echo pin
  duration = pulseIn(ECHO_PIN, HIGH);

  // Calculate distance in centimeters (speed of sound is 343 meters/second)
  distance = duration * 0.034 / 2;

  // Print the distance to the Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  glucoma = duration * Factor ; // 10  
  Serial.println("IOP ");
  Serial.print(glucoma);
  Serial.println(" mmHG");
  
  Blynk.virtualWrite(V0, glucoma);
}
void setup() {
  Serial.begin(115200); // Starts the serial communication
    Blynk.begin(BLYNK_AUTH_TOKEN, SSID, PASS);

  // ultrasonic_init();
pinMode(TRIG_PIN, OUTPUT); // Sets the trigPin as an Output
// pinMode(LED_ON, OUTPUT); // Sets the trigPin as an Output
pinMode(ECHO_PIN, INPUT); // Sets the echoPin as an Input
  timer.setInterval(1000L, sendglucomaSensor);


 }

void loop() 
{ 
  Blynk.run();
  timer.run();
  
}
