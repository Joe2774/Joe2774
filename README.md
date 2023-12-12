/* Mazen project */
#define BLYNK_TEMPLATE_ID "TMPL2bYxwOku6"
#define BLYNK_TEMPLATE_NAME "Vision at Risk"
#define BLYNK_AUTH_TOKEN "wBxEtomvzar7jnfiZ9tWm6hiM2N3iAsx"

/* Comment this out to disable prints and save space */
#define BLYNK_PRINT Serial


#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "Wifi name ";
char pass[] = "password";

BlynkTimer timer;
//      =========================================    //
//* D3 ECHO ,, D1 >> TRIG 
const int trigPin = D6;
const int echoPin = D7;
//define sound velocity in cm/uS
#define SOUND_VELOCITY 0.034
long duration;
float distanceCm;
float Factor  = 0.096; // another range factor : 0.0625 
unsigned int glucoma; 
//      ==========================================   //

void sendSensor()
{
   // Clears the trigPin
  digitalWrite(trigPin, LOW);
  delay(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delay(10);
  digitalWrite(trigPin, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  // Convert to eyes ghmeasure 190 mmHG to 21 mmHG glucoma 
    duration = pulseIn(echoPin, HIGH);
    glucoma = duration * Factor ; // 10  
  Serial.println("glucoma ");
  Serial.print(glucoma);
  Serial.println(" mmHG");
  if (glucoma > 21) {/* nothing here */ }
  delay(4000);
  if (isnan(glucoma)) {
    Serial.println("Failed to read from Ultrasonic sensor!");
    return;
  }
  Blynk.virtualWrite(V0, glucoma);
}

void setup()
{
  // Debug console
  Serial.begin(115200);
  // ultrasonic_init();
pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
// pinMode(LED_ON, OUTPUT); // Sets the trigPin as an Output
pinMode(echoPin, INPUT); // Sets the echoPin as an Input

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  // Setup a function to be called every second
  timer.setInterval(1000L, sendSensor);
}

void loop()
{
  Blynk.run();
  timer.run();
}
